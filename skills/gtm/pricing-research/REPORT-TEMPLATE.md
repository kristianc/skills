# REPORT-TEMPLATE.md

The structure of the deliverable. The pricing analysis is only as useful as the report explaining it — recommendations without traceable reasoning don't get adopted.

The report is a markdown document. Substantial — typically 8–20 pages of equivalent length. Tables and charts inline. Each section has a defined purpose; do not skip sections, do not rearrange.

## Required sections in order

### 1. Executive Summary

One page. The recommendation in headline form, with the load-bearing facts. A reader who reads only this section should walk away with the correct gist.

Includes:

- The recommended tier structure (table: tier name, target segment, mid price, 80% CI, low–high range).
- Total expected revenue with CI, and the revenue mix percentage per tier.
- The single most important caveat (data regime, key assumption, biggest sensitivity).
- One sentence on how the recommendation differs from current pricing (with the actual comparison reserved for section 9).

No methodology in this section. No charts. Compress.

### 2. Framing

What is being priced. The unit of pricing. The geography and currency. The data available and its regime (surveyed / modeled / mixed). The boundaries of what this analysis covers and does not cover.

This is also where the "existing pricing has been excluded from the analysis" statement appears explicitly. Make it visible — the reader will otherwise assume it influenced the result.

### 3. Segmentation, Targeting, Positioning

For each segment (2–5 in total):

- **Name** — used consistently throughout the rest of the report.
- **Definition** — the WTP-driving variables that put a customer in this segment.
- **Size** — addressable size with confidence range and source.
- **Value drivers** — what specifically the product enables for this segment.
- **Alternatives** — what they would do otherwise (including status quo).
- **Reference prices** — adjacent categories that anchor expectations.
- **Targeting decision** — whether the recommendation serves this segment, and at which band.
- **Positioning statement** — one sentence: "for [segment], [product] is the [category] that [differentiator], priced at [band positioning]."

A summary table at the end of the section, segment × {size, target band, positioning gist}.

### 4. WTP Analysis per Segment

For each segment:

- **WTP distribution parameters** — median and CV with confidence ranges.
- **Distribution chart** — histogram or PDF with shaded uncertainty.
- **Value pool estimate** — and the implied capture rate at the recommended mid.
- **Data regime** — surveyed (with n) / modeled (with α assumptions) / mixed.
- **Confidence statement** — narrow / moderate / wide CI, and why.

Summary table: segment × {WTP median, CV, value pool, capture rate}.

### 5. Van Westendorp Analysis

For each segment, a Van Westendorp panel:

- **Four-curve chart** (TC, B, E, TE on a common price axis), with PMC, PME, IPP, OPP marked.
- **Table of the four points** with 80% CIs.
- **Range of Acceptable Prices** explicit.
- **Interpretation paragraph** — what the curves indicate about this segment's price psychology. Are they quality-sensitive (high PMC)? Tightly clustered (narrow RAP)? Polarised (multiple inflection points)?

If the analysis is modeled rather than surveyed, the α multipliers used are stated and justified.

### 6. Banded Recommendations

The core deliverable. For each band:

- **Target segment** — named, from section 3.
- **Recommended mid** — with 80% CI.
- **Recommended range (low, high)** — strategic positioning bounds.
- **Capture rate at mid** — sanity check vs category norms.
- **Differentiation** — what makes this band distinct from neighbours, in one sentence.
- **Position relative to RAP** — where mid sits within the segment's RAP (e.g., "62nd percentile, leaning toward PME-side").

Cross-band table: tier × {target segment, mid, CI, low, high, capture rate, tier spread vs neighbours}.

A combined chart: all bands plotted on a price axis with their CIs, overlaid on segment WTP distributions. The reader should see the alignment of bands to segments at a glance.

### 7. Revenue Mix Analysis

- **Optimal price set** — the prices from REVENUE-MIX.md grid search or optimisation.
- **Total expected revenue** — with CI.
- **Revenue mix per tier** — % and $ per tier.
- **Customer mix per tier per segment** — the cannibalisation table.
- **Sensitivities** — revenue change for ±10% per tier price; identify the load-bearing tier.
- **Affinity matrix used** — disclosed as the load-bearing assumption.

A chart: stacked bar of revenue per tier, with CI whiskers on each tier and on the total.

If profit mode was run, a parallel set of outputs.

### 8. Sensitivities and Risks

- **Top three sensitivities.** The three assumptions whose movement changes the recommendation most. State the assumption, the recommendation under it, and the recommendation if it moves ±20%.
- **Data regime risks.** If WTP is modeled, what happens if the actual segments behave differently than modeled.
- **Behavioural risks.** Stated WTP overstates revealed WTP by 20–40%; if the WTP estimates are stated-preference, the recommendation has a downward correction baked in.
- **Cannibalisation risk.** What happens if cross-tier leakage is higher than modeled.
- **Competitor response risk.** Briefly: how the recommendation changes if a major competitor cuts prices or launches a new tier. (Brief because this skill does not do competitive game theory; just flag the dependency.)

### 9. Comparison to Existing Pricing

**The only section where existing prices appear.**

- **Side-by-side table** — current vs recommended, per tier or per segment.
- **Gap analysis** — by how much, in which direction, at which tiers.
- **Hypothesis on why the gap exists** — drawn from the patterns in METHODOLOGY.md (cost-plus legacy, anchor compression, volume-pricing trap, positioning drift, founder discomfort). Pick one or two; don't reach for all.
- **Migration considerations** — at the level of "things to think about," not a plan. Grandfathering, communication windows, churn risk, customer-success implications. Explicitly: this skill does not produce a migration plan, only the destination.

### 10. Methodology Note

A short appendix. One paragraph each:

- The WTP distributional assumption (lognormal default) and why.
- The Van Westendorp model (surveyed vs modeled, α assumptions if applicable).
- The self-selection model (affinity matrix, source of values).
- The CI methodology (bootstrap, n_iter, ci level).

A reader who knows pricing methodology should be able to verify or critique each choice from this section.

## Tone and writing style

- **Confident, not hedged.** Recommendations are stated as recommendations, with confidence intervals. Don't bury them in qualifiers.
- **Show the load-bearing numbers.** Capture rate, segment size, CI width, sensitivity. These are the numbers a critical reader will check.
- **One chart per non-trivial section.** Charts are not decoration — they are the way most readers will actually consume the analysis. Make them legible at thumbnail size.
- **Plain language.** "Capture rate" is fine; "monetisation efficiency" is not. Match the glossary.
- **No marketing language.** "Best-in-class," "premium experience," and similar terms have no place in a pricing analysis.

## Length guidelines

- Executive summary: 1 page.
- Framing: 0.5–1 page.
- STP: 2–3 pages.
- WTP analysis: 2–3 pages.
- Van Westendorp: 2–4 pages (one panel per segment).
- Banded recommendations: 2 pages.
- Revenue mix: 2 pages.
- Sensitivities: 1 page.
- Comparison to existing: 1 page.
- Methodology note: 0.5 page.

Total: roughly 14–18 pages. Reports under 8 pages have skipped something; reports over 25 are padding.

## Optional supplementary deliverables

If the user wants them:

- **Spreadsheet** — segments, WTP distributions, bands, revenue mix scenarios as tabs in a single XLSX. Useful when the user wants to play with assumptions. See xlsx skill.
- **Slide summary** — a 5–8 slide deck of just the executive summary plus key charts. Useful for stakeholder presentation. See pptx skill.
- **Sensitivity dashboard** — an interactive HTML widget where the user can move price sliders and see the revenue mix update. Heavier lift; offer only if asked.

The default deliverable is the markdown report alone.
