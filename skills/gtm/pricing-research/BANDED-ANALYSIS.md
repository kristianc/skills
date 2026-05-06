# BANDED-ANALYSIS.md

How to turn segment-level WTP distributions and Van Westendorp outputs into a banded price recommendation with confidence intervals.

## What a band is

A band is a price range targeted at a specific segment, with a positioning that makes the segment self-select into it. A band has five attributes:

- **Target segment** — named, drawn from step 2.
- **Price range** — `(low, mid, high)`. Mid is the recommended target; low and high are the bounds of the recommendation.
- **Confidence interval on the optimum** — typically 80% (see METHODOLOGY.md for why not 95%). This is *not* the same as `(low, high)`; the CI sits on the mid.
- **Positioning statement** — what makes this band distinct from neighbours.
- **Differentiation** — the feature, capacity, or service difference that justifies the price gap and prevents cannibalisation.

A band without differentiation is not a band, it is a discount waiting to be discovered.

## Constructing the band

For each segment, the band is built from three inputs:

1. The segment's WTP distribution (median, CV, with confidence on parameters).
2. The Van Westendorp points (PMC, PME, IPP, OPP) with confidence intervals.
3. The revenue mix optimum for this segment (computed in REVENUE-MIX.md).

Working construction:

```
band_mid    = revenue-mix optimum for this segment, constrained to RAP
band_low    = max(PMC_median, IPP_median × 0.85)
band_high   = min(PME_median, IPP_median × 1.30)
band_CI_80  = bootstrap CI on band_mid (combining WTP and α uncertainty)
```

The mid is the target. The low and high are positioning bounds — prices the seller can charge in different competitive contexts, with the mid being the default. The CI on the mid is the honest statement of how well the analysis pins down the target.

Why both `(low, high)` and CI? They answer different questions:

- `(low, high)` answers "how much room do we have to position?" — a strategic range based on the RAP and segment dynamics.
- CI answers "how well do we actually know where the optimum is?" — an analytical range based on the data quality.

Both belong in the report. They are different kinds of uncertainty.

## Confidence intervals — how to construct

The skill defaults to 80% CIs. Construct via bootstrap, propagating uncertainty from all upstream sources:

```python
import numpy as np
import pandas as pd

def banded_recommendation_with_ci(
    segment, wtp_sampler, alpha_ranges, n_iter=1000, ci=0.80, capture_floor=None
):
    """
    Returns the band's (low, mid, high) and the CI on mid.

    wtp_sampler: callable returning fresh WTP samples each call
    alpha_ranges: {'TC': (lo, hi), 'B': (lo, hi), 'E': (lo, hi)}
    capture_floor: optional minimum capture rate to enforce on band_mid
    """
    rng = np.random.default_rng()
    mids, lows, highs = [], [], []

    for _ in range(n_iter):
        wtp = wtp_sampler()
        a_TC = rng.uniform(*alpha_ranges['TC'])
        a_B  = rng.uniform(*alpha_ranges['B'])
        a_E  = rng.uniform(*alpha_ranges['E'])

        # Generate VW points
        too_exp = wtp
        exp     = wtp * a_E
        barg    = wtp * a_B
        tc      = wtp * a_TC

        # Quick analytical approximations of the four points
        # (or call vw_curves() if precision matters)
        IPP = np.median(np.concatenate([barg, exp]))  # rough
        PMC = np.percentile(np.concatenate([tc, exp]), 30)
        PME = np.percentile(np.concatenate([too_exp, barg]), 70)
        OPP = np.median(np.concatenate([tc, too_exp]))

        # Mid: in this draw, take the IPP-OPP midpoint as a placeholder
        # In real use, pass through the revenue-mix optimiser
        mid = (IPP + OPP) / 2
        if capture_floor is not None:
            mid = max(mid, capture_floor)
        mid = max(PMC, min(mid, PME))  # clamp to RAP

        low  = max(PMC, IPP * 0.85)
        high = min(PME, IPP * 1.30)

        mids.append(mid)
        lows.append(low)
        highs.append(high)

    lo_q, hi_q = (1 - ci) / 2, 1 - (1 - ci) / 2
    return {
        'low':  np.median(lows),
        'mid':  np.median(mids),
        'high': np.median(highs),
        'mid_ci': (np.quantile(mids, lo_q), np.quantile(mids, hi_q)),
        'samples': {'mids': mids, 'lows': lows, 'highs': highs},
    }
```

For real recommendations, pass `mid` through the revenue-mix optimiser rather than using the IPP-OPP midpoint placeholder above. The optimiser accounts for cross-band cannibalisation that this single-segment view does not.

## Tier spread between bands

Adjacent bands need to be far enough apart that customers self-select correctly. Empirical rule:

- **Below 1.5x** between adjacent mids: tiers cannibalise. High-WTP customers in the upper segment pick the lower tier and you give up margin.
- **1.5–3x**: the safe zone for most products. Differentiation can plausibly justify the gap.
- **Above 3x**: tiers feel like different products. Acceptable for genuinely different offerings (self-serve vs enterprise) but suspicious within a single product line.

If the WTP-derived band mids violate the 1.5x rule, the segmentation is probably finer than it needs to be. Merge adjacent segments.

If the band mids exceed 3x, the segmentation may be combining genuinely different products. Consider splitting into separate offerings rather than tiers of one offering.

## Differentiation — what justifies the gap

Bands need *differential value* across segments, not just different prices. A higher band must give the targeted segment something the lower band doesn't. Common differentiation axes:

- **Capacity gates** — seats, requests, storage, projects. Effective when usage scales with WTP (which it usually does in B2B).
- **Feature gates** — advanced features unlocked at higher tiers. Effective when the gated feature is *differentially* valued. Useless if every segment wants it equally — that just creates resentment.
- **Service gates** — support SLA, dedicated CSM, training. Effective for enterprise tiers; mostly ignored by smaller segments.
- **Compliance / control gates** — SSO, audit logs, custom contracts. Often the cleanest enterprise differentiator because procurement explicitly demands them.
- **Usage commitment** — annual vs monthly, prepayment discounts. Differentiates by buying behaviour rather than feature, useful when feature gates are unavailable.

The differentiation should be discoverable by the segment without sales involvement. If a self-serve customer has to talk to sales to figure out which tier they belong in, the gating is wrong.

## How many bands

In the analysis, there is one band per segment that is being targeted. In the recommendation:

- **2–3 productized tiers** is typical. More than three usually means tiers are too close together and most customers default to "the second one" regardless.
- **A separate "enterprise" or "custom" tier** is common as the top end, where pricing is negotiated rather than published. This is fine; the published price for that tier is "talk to us," and the analysis still produces a target for the negotiation.
- **A free tier** is a strategic decision, not a pricing one. If the user wants one, treat it as a customer-acquisition channel into the lowest paid tier; do not analyse it as a price band.

## Quality checks before moving on

For each band, before passing to revenue mix:

- **Mid sits inside the RAP.** If `band_mid < PMC` or `band_mid > PME`, something is wrong upstream. Investigate.
- **CI doesn't cross zero or cross adjacent bands.** A CI that overlaps the adjacent band's CI means the bands aren't actually distinct at the data quality you have. Either merge them or report them as one band with a wider range.
- **Differentiation is named.** If you can't say in one sentence what justifies this band's gap from the next one down, the band is not yet defendable.
- **Capture rate is plausible.** `band_mid / value_pool` should land in a sane range for the category (10–30% is typical for B2B SaaS; check METHODOLOGY.md). If it's outside, double-check the value pool estimate.

## What to report

Per band:

| Attribute | Value |
|---|---|
| Target segment | Named |
| Recommended mid | $X (80% CI: $Y–$Z) |
| Recommended range (low, high) | $A – $B |
| Position relative to RAP | mid sits at the Nth percentile of RAP |
| Capture rate | mid / value pool |
| Differentiation | one-sentence statement |
| Data regime | surveyed / modeled / mixed |

Plot all bands on a common price axis with their CIs, alongside the segment WTP distributions, so the user can see the full picture in one image.
