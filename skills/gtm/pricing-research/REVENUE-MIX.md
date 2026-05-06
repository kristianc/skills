# REVENUE-MIX.md

How to estimate the optimal revenue mix across the recommended bands. This is what turns a set of per-segment price recommendations into a coherent business: which tiers should generate what share of revenue, how sensitive total revenue is to the prices chosen, and where the load-bearing assumptions are.

## What "optimal" means here

Maximise expected revenue (or expected profit if marginal cost is known) across the full segment-tier matrix, given:

- Segment sizes (with confidence).
- Per-segment WTP distributions.
- A self-selection model (which tier a customer in each segment chooses).
- Cannibalisation between tiers.
- Optionally: marginal cost per tier, customer acquisition cost per segment.

The output is the price set `(p_1, p_2, ..., p_k)` that maximises expected revenue, plus the resulting revenue mix and sensitivities.

This is distinct from "OPP per segment" because the OPP is computed in isolation. The revenue mix optimum accounts for the fact that pricing tier 1 too low pulls customers from tier 2, and that pricing tier 2 too high pushes customers down to tier 1 or out entirely.

## The self-selection model

When a customer with WTP `w` is offered tiers `(p_1 < p_2 < ... < p_k)` with perceived values `(v_1 < v_2 < ... < v_k)`, they choose the tier maximising surplus `(v_j − p_j)` subject to `p_j ≤ w`. If no tier produces non-negative surplus they reject all.

The hard part is `v_j` — the *perceived* value of tier j to a customer in segment i. Two practical approaches:

### Approach A: Tier-affinity (default)

Each segment has a "natural" tier (the one targeted at it). Define `v_j` for a customer in segment `i` as:

```
v_ij = w × affinity(i, j)
```

Where `affinity(i, j)` ∈ [0, 1] reflects how well tier j fits segment i. Affinity = 1 for the natural tier, declining for tiers above (over-served, paying for unused capacity) and below (under-served, missing features).

Working defaults for a 3-tier B2B SaaS structure with segments small/mid/enterprise:

```
affinity matrix (rows = segments, cols = tiers):
            Tier 1 (Small)  Tier 2 (Mid)  Tier 3 (Enterprise)
Small         1.00            0.85         0.60
Mid           0.70            1.00         0.85
Enterprise    0.40            0.75         1.00
```

The diagonal is 1. Off-diagonal reflects feature/capacity fit. Tune from the differentiation rationale built in BANDED-ANALYSIS.md — strong differentiation produces sharper off-diagonal drops; weak differentiation produces flatter affinities and more cannibalisation.

### Approach B: Feature-bundle valuation

If you have explicit feature value-per-segment data, sum the feature values per tier instead of using a multiplicative affinity. More precise when data exists, more brittle when data is hand-waved. Default to Approach A unless feature value data is real.

## The simulation

```python
import numpy as np
import pandas as pd

def simulate_revenue_mix(
    segments,           # list of dicts: {'name', 'size', 'wtp_sampler'}
    tiers,              # list of prices [p_1, ..., p_k]
    affinity,           # np.array shape (n_segments, n_tiers)
    n_customers=10000,  # samples per segment
    marginal_cost=None, # optional list per tier; None = revenue mode
    rng=None,
):
    rng = rng or np.random.default_rng()
    n_seg = len(segments)
    n_tier = len(tiers)
    tiers = np.asarray(tiers)

    # Per-tier accumulators
    revenue_per_tier = np.zeros(n_tier)
    customers_per_tier_per_segment = np.zeros((n_seg, n_tier))
    no_purchase = np.zeros(n_seg)

    for i, seg in enumerate(segments):
        wtp = seg['wtp_sampler'](n_customers)
        # Per customer, perceived value at each tier
        v = wtp[:, None] * affinity[i, None, :]      # (n_customers, n_tiers)
        # Affordable mask: tier price <= WTP
        affordable = tiers[None, :] <= wtp[:, None]
        # Surplus: v - p where affordable, -inf otherwise
        surplus = np.where(affordable, v - tiers[None, :], -np.inf)
        # Best tier per customer; -1 if none affordable / none positive
        best = np.argmax(surplus, axis=1)
        any_pos = np.max(surplus, axis=1) >= 0
        best = np.where(any_pos, best, -1)

        for j in range(n_tier):
            chose = (best == j)
            count = chose.sum()
            customers_per_tier_per_segment[i, j] = count / n_customers * seg['size']
        no_purchase[i] = (best == -1).sum() / n_customers * seg['size']

    customers_per_tier = customers_per_tier_per_segment.sum(axis=0)
    revenue_per_tier = customers_per_tier * tiers

    if marginal_cost is not None:
        profit_per_tier = customers_per_tier * (tiers - np.asarray(marginal_cost))
    else:
        profit_per_tier = None

    return {
        'tiers': tiers,
        'customers_per_tier': customers_per_tier,
        'customers_per_tier_per_segment': customers_per_tier_per_segment,
        'no_purchase_per_segment': no_purchase,
        'revenue_per_tier': revenue_per_tier,
        'total_revenue': revenue_per_tier.sum(),
        'profit_per_tier': profit_per_tier,
        'total_profit': None if profit_per_tier is None else profit_per_tier.sum(),
        'revenue_mix_pct': revenue_per_tier / revenue_per_tier.sum() if revenue_per_tier.sum() > 0 else np.zeros(n_tier),
    }
```

## Optimising the price set

Given the simulation, find the prices that maximise expected revenue (or profit). Two approaches:

### Grid search (default for ≤ 3 tiers)

Construct each tier's candidate range from BANDED-ANALYSIS.md (`band_low` to `band_high`), discretise into ~10 candidates, and evaluate every combination.

```python
from itertools import product

def grid_search_mix(segments, tier_ranges, affinity, n_steps=10, **sim_kwargs):
    grids = [np.linspace(lo, hi, n_steps) for (lo, hi) in tier_ranges]
    best = None
    for combo in product(*grids):
        # tiers must be increasing
        if list(combo) != sorted(combo):
            continue
        out = simulate_revenue_mix(segments, list(combo), affinity, **sim_kwargs)
        score = out['total_profit'] if out['total_profit'] is not None else out['total_revenue']
        if best is None or score > best['score']:
            best = {'tiers': combo, 'score': score, 'result': out}
    return best
```

### Numerical optimisation (4+ tiers, or fine-grained)

Use `scipy.optimize.minimize` with bounds from the band ranges. Negate the objective. Multiple restarts to avoid local optima. Slower but handles higher-dim cases.

## Confidence on total revenue

The point estimate of total revenue is misleading without a CI. Bootstrap by re-sampling segment sizes and WTP parameters:

```python
def revenue_mix_with_ci(
    segments_with_uncertainty,  # each segment has size_dist and wtp_param_dist
    tiers,
    affinity,
    n_iter=500,
    ci=0.80,
):
    totals = []
    mixes = []
    for _ in range(n_iter):
        segments = [s.sample() for s in segments_with_uncertainty]
        out = simulate_revenue_mix(segments, tiers, affinity)
        totals.append(out['total_revenue'])
        mixes.append(out['revenue_mix_pct'])
    lo, hi = (1 - ci) / 2, 1 - (1 - ci) / 2
    return {
        'total_revenue_median': np.median(totals),
        'total_revenue_ci': (np.quantile(totals, lo), np.quantile(totals, hi)),
        'mix_per_tier_median': np.median(mixes, axis=0),
        'mix_per_tier_ci': [
            (np.quantile([m[j] for m in mixes], lo), np.quantile([m[j] for m in mixes], hi))
            for j in range(len(tiers))
        ],
    }
```

## Sensitivities

For each tier, report how much total revenue changes if that tier's price moves ±10% (or ±20%). This identifies the load-bearing prices — the ones where the recommendation is most sensitive to being wrong.

```python
def sensitivities(segments, tiers, affinity, delta=0.10, **sim_kwargs):
    base = simulate_revenue_mix(segments, tiers, affinity, **sim_kwargs)['total_revenue']
    results = []
    for j, p in enumerate(tiers):
        up = list(tiers); up[j] = p * (1 + delta)
        dn = list(tiers); dn[j] = p * (1 - delta)
        r_up = simulate_revenue_mix(segments, up, affinity, **sim_kwargs)['total_revenue']
        r_dn = simulate_revenue_mix(segments, dn, affinity, **sim_kwargs)['total_revenue']
        results.append({
            'tier': j,
            'price': p,
            f'rev_at_+{int(delta*100)}%': r_up,
            f'rev_at_-{int(delta*100)}%': r_dn,
            'sensitivity_up_pct':   (r_up - base) / base * 100,
            'sensitivity_down_pct': (r_dn - base) / base * 100,
        })
    return pd.DataFrame(results)
```

A tier where +10% price change leaves total revenue nearly unchanged is under-priced (you're capturing the same customers but paying less per). A tier where +10% drops revenue significantly is at or above its sweet spot.

## Cannibalisation diagnosis

After running the simulation, examine `customers_per_tier_per_segment`. For each segment, the column sums show where they ended up:

- **Mostly in their natural tier**: clean self-selection. Bands are working.
- **Significant share in lower tier than natural**: cannibalisation. Either the differentiation isn't strong enough, or the upper tier is over-priced relative to its incremental value.
- **Significant share in higher tier than natural**: usually fine, but check that perceived value at higher tiers wasn't over-modeled.
- **Significant share in no-purchase**: under-served segment. Either lower the entry tier, add a tier below, or accept the segment is not addressable at current prices.

The report should include a small table showing where each segment ends up, in counts and percentages.

## Optional: profit mode

If marginal cost per tier is known, run the optimiser on profit instead of revenue. Common findings:

- The revenue-optimal mix and the profit-optimal mix differ.
- Profit optimum is typically slightly higher prices and slightly lower volume than revenue optimum.
- The gap is biggest when marginal costs vary significantly across tiers (e.g., enterprise tier with included support has higher marginal cost than self-serve).

If marginal cost data is available, report both modes — revenue-optimal as the "growth lens," profit-optimal as the "margin lens." The user picks based on business stage.

## Output of this step

- The optimal price set (one price per tier) with CI on each.
- Total expected revenue with CI.
- Revenue mix per tier (% and $) with CI.
- Customer mix per tier per segment (the cannibalisation table).
- Sensitivities (revenue change for ±10% in each tier).
- The affinity matrix used (for transparency — it's a load-bearing assumption).
- If profit mode: the same outputs run on profit.
