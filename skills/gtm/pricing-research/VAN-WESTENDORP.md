# VAN-WESTENDORP.md

The Price Sensitivity Meter, originally Peter van Westendorp (1976). Run per segment. Always per segment — running it across the whole market mashes incompatible WTP distributions and produces output that looks specific but means nothing.

## The four questions

For a clearly described product/service:

1. At what price would you consider this **so cheap that you would doubt the quality** (Too Cheap)?
2. At what price would you consider this a **bargain** — a great buy for the money (Bargain / Cheap)?
3. At what price would this **start to feel expensive**, but you would still consider it (Expensive)?
4. At what price would this be **too expensive to consider** at all (Too Expensive)?

Per respondent, you get four numbers, ordered: TC ≤ B ≤ E ≤ TE (this ordering is enforced in clean survey data; if not enforced, the respondent didn't understand the question and the response should be discarded).

## The four cumulative curves

For each price `p`:

- **TC(p)** = fraction of respondents whose Too Cheap answer ≥ p. Decreasing in p.
- **B(p)** = fraction whose Bargain answer ≥ p. Decreasing in p.
- **E(p)** = fraction whose Expensive answer ≤ p. Increasing in p.
- **TE(p)** = fraction whose Too Expensive answer ≤ p. Increasing in p.

Each curve runs from 0 to 1 across the price range.

## The four intersection points

- **PMC** — Point of Marginal Cheapness — `TC(p) = E(p)`. Below this, more people think it is too cheap to trust than think it is acceptably expensive. Practical floor.
- **PME** — Point of Marginal Expensiveness — `TE(p) = B(p)`. Above this, more people think it is too expensive than think it is a bargain. Practical ceiling.
- **IPP** — Indifference Price Point — `B(p) = E(p)`. Equal numbers calling it cheap or expensive. The "median acceptable price."
- **OPP** — Optimal Price Point — `TC(p) = TE(p)`. Minimises the count of people rejecting the price as too-something. Most often quoted as "the optimum."

The **Range of Acceptable Prices (RAP)** is `[PMC, PME]`.

## What the four points actually mean

- **PMC** is mostly relevant when "perceived quality" is a real category concern (premium goods, professional services, anything with trust signals). For commodity B2B SaaS where quality skepticism is low, PMC is often well below where you'd want to price anyway, and is informational rather than load-bearing.
- **PME** is the more practical ceiling. Customers can rationalise paying a price they call "expensive," but they walk away at "too expensive." Pricing above PME loses volume sharply.
- **IPP** is roughly where the typical buyer thinks the market price *should be*. Useful as a sanity check on positioning: if your recommended price sits well above IPP, you are positioning premium, and the marketing must support it.
- **OPP** has the most contested interpretation. It minimises *resistance*, but resistance and revenue are not the same thing. OPP is not the price that maximises revenue — that is solved separately in REVENUE-MIX.md. Treat OPP as a centre of gravity, not as the answer.

## Choosing the price point to recommend

This is the part where pricing reports go wrong. The Van Westendorp output is four points and a range. The recommendation is one band per segment with an interval. Translation rules:

- **Recommended mid** — usually between IPP and OPP, leaning toward IPP for share-maximising positioning and toward OPP (and above) for revenue-maximising. The optimal mid is determined by REVENUE-MIX.md, not by VW alone.
- **Recommended low** — close to PMC if quality skepticism is real; otherwise close to IPP – σ. Below this, you're capturing customers but underpricing the segment.
- **Recommended high** — close to PME. Above this, conversion drops sharply.

Don't recommend the OPP as "the price." The four points are a constraint set; the actual recommendation is built from them in step 5.

## Survey processing — when you have the data

```python
import numpy as np
import pandas as pd
from scipy.interpolate import interp1d

def vw_curves(responses: pd.DataFrame, price_grid: np.ndarray):
    """
    responses: DataFrame with columns ['too_cheap', 'bargain', 'expensive', 'too_expensive']
    price_grid: array of prices to evaluate the curves on
    Returns: dict of curves and the four intersection points
    """
    n = len(responses)

    # Cumulative fractions at each price on the grid
    tc = np.array([(responses['too_cheap']     >= p).sum() / n for p in price_grid])
    b  = np.array([(responses['bargain']       >= p).sum() / n for p in price_grid])
    e  = np.array([(responses['expensive']     <= p).sum() / n for p in price_grid])
    te = np.array([(responses['too_expensive'] <= p).sum() / n for p in price_grid])

    def crossing(curve_a, curve_b):
        diff = curve_a - curve_b
        sign_changes = np.where(np.diff(np.sign(diff)))[0]
        if len(sign_changes) == 0:
            return None
        i = sign_changes[0]
        # linear interp
        x0, x1 = price_grid[i], price_grid[i+1]
        y0, y1 = diff[i], diff[i+1]
        return x0 - y0 * (x1 - x0) / (y1 - y0)

    return {
        'curves': {'TC': tc, 'B': b, 'E': e, 'TE': te, 'price': price_grid},
        'PMC': crossing(tc, e),
        'PME': crossing(te, b),
        'IPP': crossing(b, e),
        'OPP': crossing(tc, te),
    }
```

Confidence intervals via bootstrap:

```python
def vw_bootstrap(responses, price_grid, n_boot=1000, ci=0.80):
    points = {'PMC': [], 'PME': [], 'IPP': [], 'OPP': []}
    rng = np.random.default_rng()
    for _ in range(n_boot):
        sample = responses.sample(n=len(responses), replace=True, random_state=rng.integers(1<<32))
        out = vw_curves(sample, price_grid)
        for k in points:
            if out[k] is not None:
                points[k].append(out[k])
    lo, hi = (1 - ci) / 2, 1 - (1 - ci) / 2
    return {k: (np.quantile(v, lo), np.median(v), np.quantile(v, hi)) for k, v in points.items()}
```

Sample size matters. n < 30 produces CIs so wide they're functionally useless; report them but warn explicitly. n > 200 gives clean curves. Real pricing surveys aim for n > 100 per segment.

## Modeling the four curves without survey data

When no survey data exists, build the four curves from the segment's modeled WTP distribution. The mapping between WTP and the four answers is psychological, not mechanical, but the working model is:

A respondent with WTP `w` answers:

- **Too Expensive** ≈ `w`. The price at which they walk is their WTP, by definition.
- **Expensive** ≈ `w × α_E` where `α_E ∈ [0.75, 0.90]`. The price at which they wince but proceed.
- **Bargain** ≈ `w × α_B` where `α_B ∈ [0.50, 0.75]`. The price that feels like a deal.
- **Too Cheap** ≈ `w × α_TC` where `α_TC ∈ [0.20, 0.50]`. The price that triggers quality doubt.

The α multipliers are category-dependent:

- **High-trust/quality-sensitive categories** (professional services, premium consumer): α_TC closer to 0.50, narrower spread overall. Customers are sensitive to cheapness as a quality signal.
- **Commodity/utility categories** (cloud storage, basic SaaS): α_TC closer to 0.20 or even lower. Customers don't infer quality from price; cheaper is better until the product is unusable.
- **Status/luxury categories**: α_B and α_E closer together, α_TC very high (0.50+). Cheapness destroys the value proposition.

Sketch:

```python
def model_vw_from_wtp(wtp_samples, alpha_TC=0.35, alpha_B=0.65, alpha_E=0.85, price_grid=None):
    """
    Given samples from a segment's WTP distribution, generate the four VW responses
    per respondent and compute the four curves on the price grid.
    """
    too_expensive = wtp_samples
    expensive     = wtp_samples * alpha_E
    bargain       = wtp_samples * alpha_B
    too_cheap     = wtp_samples * alpha_TC

    responses = pd.DataFrame({
        'too_cheap': too_cheap,
        'bargain': bargain,
        'expensive': expensive,
        'too_expensive': too_expensive,
    })
    if price_grid is None:
        price_grid = np.linspace(too_cheap.min(), too_expensive.max(), 200)
    return vw_curves(responses, price_grid)
```

Confidence intervals when modeling: bootstrap not just the WTP samples but also the α multipliers across plausible category ranges. The resulting CIs will be wider than survey-derived CIs — that is correct, not a flaw of the method.

```python
def model_vw_with_uncertainty(wtp_sampler, alpha_ranges, price_grid, n_iter=500, ci=0.80):
    """
    wtp_sampler: function returning a fresh array of WTP samples each call
    alpha_ranges: dict of (low, high) for each alpha multiplier
    """
    points = {'PMC': [], 'PME': [], 'IPP': [], 'OPP': []}
    rng = np.random.default_rng()
    for _ in range(n_iter):
        wtp = wtp_sampler()
        a_TC = rng.uniform(*alpha_ranges['TC'])
        a_B  = rng.uniform(*alpha_ranges['B'])
        a_E  = rng.uniform(*alpha_ranges['E'])
        out = model_vw_from_wtp(wtp, a_TC, a_B, a_E, price_grid)
        for k in points:
            if out[k] is not None:
                points[k].append(out[k])
    lo, hi = (1 - ci) / 2, 1 - (1 - ci) / 2
    return {k: (np.quantile(v, lo), np.median(v), np.quantile(v, hi)) for k, v in points.items()}
```

## What to report from this step

For each segment:

- The four curves, plotted (TC, B, E, TE) on the same price axis.
- The four intersection points with confidence intervals: PMC, PME, IPP, OPP — each as `(low, median, high)`.
- The Range of Acceptable Prices: `[PMC_median, PME_median]`.
- The data regime (surveyed vs modeled) and the implied confidence in the output.
- Category α assumptions, when modeling.

Do not yet recommend a price. The recommendation is constructed in step 5 (BANDED-ANALYSIS.md), using these points as constraints alongside the revenue mix optimisation.

## Common Van Westendorp failure modes

- **Mixed-segment data.** Running PSM across segments with different WTP distributions produces curves with misleading shapes — often bimodal, with shallow intersections that look like clean answers but mask the underlying structure. Run per segment.
- **Anchoring leakage.** Asking PSM questions after showing the current price contaminates the responses. Don't.
- **Free-text answers.** Customers offering round numbers ($10, $50, $100) cluster at thresholds that don't match the underlying psychology. Treat the curves as approximations of an underlying continuous distribution; don't over-interpret point estimates.
- **Reading OPP as "the price."** OPP is the centre of resistance, not the maximiser of revenue. Treat all four points as inputs, not answers.
- **Ignoring the RAP.** Many reports collapse VW to one number. The interval `[PMC, PME]` is information; don't throw it away.
