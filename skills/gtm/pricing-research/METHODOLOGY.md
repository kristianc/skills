# METHODOLOGY.md

The reasoning behind the skill's process. Read this if you need to defend a methodological choice, or if a step in SKILL.md isn't producing useful output and you need to understand what it's actually trying to do.

## Why value, not cost-plus

Cost-plus pricing answers a different question than this skill asks. Cost-plus tells you the price at which the *seller* is indifferent (margin = target). Value-based tells you the price at which the *buyer* is indifferent (price = WTP, or price = some target capture of value).

These usually disagree. A SaaS feature with $0.10/customer marginal cost can credibly sell for $200/seat/month if the value pool justifies it. A physical good with $40 cost can be unsellable at $50 if the segment's WTP distribution sits below $50.

**Cost is a viability check, not an input.** If the value-based recommendation is below cost, the conclusion is "this segment is unservable," not "raise the price to cover cost." Raising the price above WTP doesn't sell more units — it sells fewer. The right move is either to find a different segment, change the cost structure, or change the product.

## Why segment first

Willingness to pay varies by orders of magnitude across segments — not by tens of percent. A small business and a Fortune 500 buying nominally the same product often differ in WTP by 10–100x.

Single-price recommendations leave money on the table in two directions:

- **Pricing for the median customer.** High-WTP customers pay far less than they would have. Volume is fine, ARPU is low.
- **Pricing for the high segment.** Mid- and low-WTP customers don't convert at all. ARPU is fine, volume collapses.

Tiered pricing recovers both. The recommendation is therefore almost always a tier structure, even when the user wants a single number.

The exception is when segments are genuinely indistinguishable — same WTP, same alternatives, same value derived. This is rarer than founders think. Push the user to articulate why segments would have the same WTP before accepting a single-price recommendation.

## Why distributions, not points

Within any segment, individual customers vary in WTP. Real demand curves are smooth, not step functions. A point estimate of "the WTP for this segment is $X" hides:

- The width of the distribution (which determines volume sensitivity to price changes).
- The skew (right-skew is typical: a long tail of high-WTP customers).
- The probability of conversion at a given price (which is `1 - CDF(p)`, not `p < WTP`).

Point estimates lead to under-recognised cannibalisation, over-confident revenue forecasts, and the false impression that there is "a" right price.

## Default distributional assumption

Use **lognormal** for WTP within a segment unless evidence suggests otherwise.

Lognormal is right-skewed, strictly positive, and one-tailed enough to model both "most customers cluster around a value" and "a small fraction will pay much more." It fits empirical WTP data well across most categories.

Parameters: median `m` and a coefficient of variation `cv` (or equivalently, μ and σ in log space). For most B2B segments, `cv` ≈ 0.4–0.7. Wider CVs (0.8+) for emerging categories where customers haven't anchored yet; narrower (0.3) for mature, well-priced categories.

```python
import numpy as np

def lognormal_wtp(median, cv, n_samples=10000, rng=None):
    """Sample WTP values from a lognormal with given median and CV."""
    rng = rng or np.random.default_rng()
    sigma = np.sqrt(np.log(1 + cv**2))
    mu = np.log(median)
    return rng.lognormal(mean=mu, sigma=sigma, size=n_samples)
```

Don't assume normal. Negative-WTP samples are nonsensical; symmetric distributions miss the high-payer tail that drives premium-tier revenue.

## Translating value to WTP

The value pool sets the ceiling on sustainable WTP, but the actual WTP distribution sits below the value pool by a capture rate that depends on:

- **Alternatives.** What the customer would do otherwise. WTP for a product where the alternative is "do without" is closer to the full value pool. WTP for a product with capable substitutes sits at the substitute's price plus a switching premium.
- **Reference prices.** What customers expect things "like this" to cost. Strong reference prices compress WTP distributions toward the reference, regardless of value.
- **Buyer sophistication.** Sophisticated buyers (e.g., enterprise procurement) extract more capture for themselves, narrowing seller capture rate. Naive buyers leave more on the table.
- **Substitutability of the value.** Value that is hard to translate to dollars (delight, peace of mind, status) supports higher capture rates than value that is easy to translate (hours × wage rate).

Working translation:

```
WTP_median ≈ value_pool × capture_rate
capture_rate ≈ baseline (0.10–0.30 typical for B2B SaaS)
              × alternative_factor (0.5 if strong sub, 1.0 if none)
              × reference_factor (drag toward reference price)
              × sophistication_factor (0.7 if procurement, 1.0 if line-of-business buyer)
```

These multipliers are estimates and the model has wide bounds. Reflect the uncertainty in the CV of the resulting distribution. When using modeled (not surveyed) WTP, default to `cv ≥ 0.6`.

## Confidence in WTP distributions

Three sources of uncertainty stack:

1. **Sampling uncertainty** — even with real survey data, parameter estimates have CIs.
2. **Modeling uncertainty** — when WTP is estimated from value rather than measured, the parameters are guesses.
3. **Behavioural uncertainty** — what people say their WTP is differs from what they pay. Stated-preference data over-states WTP by 20–40% typically.

A pricing recommendation derived from real, behavioural (transaction) data deserves a tight CI on its outputs. One derived from stated-preference surveys deserves a moderate CI. One derived from modeling alone deserves a wide CI.

The report must state which regime each segment is in. A single recommendation that mixes regimes (one segment with transaction data, another modeled) should report differential confidence per band.

## Why 80% intervals, not 95%

The skill defaults to 80% confidence intervals on price recommendations, not 95%.

The reason is communication, not statistics. A 95% CI on a modeled WTP distribution is so wide it functions as "we have no idea." An 80% CI is narrower and forces the seller to engage with the trade-off rather than dismiss it as noise. Decisions made with 80% CIs and explicit acknowledgment of the residual 20% are typically better than decisions made with 95% CIs that get mentally collapsed to the midpoint.

If the user wants 95%, give it — but also provide 80% alongside, and explain.

## What "ignore existing pricing" means in practice

It does not mean pretending the existing price doesn't exist. It means:

- The analysis pipeline (steps 2–6) does not use the existing price as an input.
- Segment WTP estimates are not built around the existing price.
- The recommended bands are not constrained to be near the existing price.
- The Van Westendorp curves are not adjusted to make the existing price look like the OPP.

It does mean, in step 7:

- The existing price is reported.
- The gap between recommendation and existing is named.
- A hypothesis about *why* the gap exists is offered. Common patterns:
  - **Cost-plus legacy.** Original price was set on cost margin; never revisited.
  - **Anchor compression.** Original price was set at competitor benchmark; never tested.
  - **Volume-pricing trap.** Price held low to drive volume; segment WTP was higher all along.
  - **Positioning drift.** Product evolved upmarket but pricing didn't follow.
  - **Founder discomfort.** Owners undercharge for what they've built; common in services.

The hypothesis is not load-bearing — the recommendation stands either way. But it is useful for the migration plan, which is about how to move from current to recommended pricing without losing more revenue than the recommendation gains.

## Migration is a separate problem

This skill produces the destination, not the route. Moving an installed customer base from one price to another involves grandfathering decisions, communication, churn risk, and timing — none of which is in the scope of the recommendation itself.

The report should note migration considerations as a final section, but not solve them.
