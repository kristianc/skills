---
name: pricing-research
description: Produce a detailed pricing analysis grounded in segmentation, targeting and positioning, with banded recommendations carrying confidence intervals, Van Westendorp price sensitivity analysis, and an estimated optimal revenue mix. Use when the user wants to price a product or service from first principles, restructure pricing tiers, evaluate willingness to pay, build a pricing recommendation backed by quantitative analysis, or assess whether current pricing is leaving revenue on the table. Explicitly ignores the product's existing pricing — the recommendation is grounded in value to the customer and market structure, not in what is currently charged.
---

# Pricing Research

Produce a defensible pricing recommendation by working from value, segments, and willingness to pay — not from current prices, not from competitor anchors, not from cost-plus.

The deliverable is a structured report covering segmentation/targeting/positioning, per-segment willingness-to-pay distributions with confidence intervals, banded price recommendations, Van Westendorp price sensitivity analysis, and an estimated optimal revenue mix.

## Core posture

**Existing prices are excluded from the recommendation.** They appear in the report only at the end, in a "current vs recommended" comparison section. They are not used as anchors, sanity checks, or starting points for the analysis. This is the explicit point of the skill: to surface what the price *should* be based on market structure and value, independent of accumulated pricing history.

**Competitor prices are excluded as anchors.** They are admissible only as evidence about reference-price formation — what a typical buyer expects this category to cost. The recommendation is built from segments and value, not from where competitors landed.

**Cost is not a pricing input.** Cost is a viability check on the floor. If the recommended price is below cost, the business is wrong — but the price is still right.

## Glossary

Use these terms exactly. Full definitions in GLOSSARY.md.

- **Segment** — a group of customers whose willingness to pay clusters separately, driven by distinct value, alternatives, or constraints.
- **Willingness to Pay (WTP)** — the maximum a customer would pay before walking. Distribution-valued, never point-valued.
- **Reference price** — what customers expect things "like this" to cost, formed by adjacent category experience.
- **Value pool** — total economic value the product creates for a segment.
- **Capture rate** — fraction of the value pool captured by price.
- **Band** — a price range targeted at a specific segment, with a specific positioning.
- **Tier** — a productized band, with feature differentiation that justifies the price difference.
- **OPP / PMC / PME / IPP / RAP** — Van Westendorp outputs (see VAN-WESTENDORP.md).
- **Revenue mix** — distribution of expected revenue across tiers.

## Key principles

Full list in METHODOLOGY.md.

- **Price reflects value, not cost.** Cost-plus is what a regulated utility does. It is not pricing.
- **One price for "the market" is almost always wrong.** WTP varies across segments by orders of magnitude. The recommendation is a tier structure, not a number.
- **Confidence is part of the recommendation.** A point estimate without a confidence interval hides what is actually known.
- **Revenue is a portfolio.** The right structure maximises expected revenue (or profit) across the segment mix, not price for any single customer.
- **Existing prices are evidence about pricing history, not about value.** Re-derive from segments first; compare at the end.

## Process

### 1. Frame

Establish what is being priced and for whom. Cover:

- What the product/service does, and the unit of pricing (per seat, per use, per month, per outcome, per package).
- The customer (organisations, individuals, both) — at the level needed to segment them.
- The job being done and the alternatives considered, including no-purchase / status quo.
- Geography and currency.
- Data available: any survey results, customer interviews, deal history, win/loss notes.
- The existing price — captured here, then set aside until step 7.

Ask the user. Don't infer the framing.

### 2. Segment

Identify 2–5 segments using SEGMENTATION.md. Pricing segmentation is not marketing segmentation — segments here must differ in **what they are willing to pay**, not just in messaging or demographics.

For each segment, name:
- Distinct value drivers (what specifically the product enables).
- Alternatives realistically considered (including in-house build and status quo).
- Reference prices — what adjacent categories anchor expectations on.
- Approximate market size, with confidence.

### 3. Build value and WTP distributions per segment

Estimate the value pool per segment. Translate to a WTP distribution (typically lognormal) with explicit parameters and confidence. Methodology in METHODOLOGY.md.

If real survey or transaction data exists, use it. If not, build the WTP distribution from value, alternatives, and reference prices, with a wider confidence interval reflecting the modeling.

### 4. Van Westendorp analysis per segment

Run the four-question analysis per segment to find the Range of Acceptable Prices, the Optimal Price Point, the Indifference Price Point, and the marginal points (PMC and PME). See VAN-WESTENDORP.md, which covers both real-survey processing and how to model the four curves from a WTP distribution when survey data is absent.

Always report the four points with confidence intervals, never as point estimates.

### 5. Construct banded recommendations

For each band, recommend a price range with a confidence interval, using BANDED-ANALYSIS.md. A band has:
- A target segment.
- A low / mid / high price.
- A confidence interval on the optimum (typically 80% — see BANDED-ANALYSIS.md for why not 95%).
- A positioning statement (what makes this band distinct from adjacent bands).
- The feature/value differentiation that justifies it.

### 6. Estimate optimal revenue mix

Model expected revenue per band given segment sizes, conversion-at-price, and cannibalisation. See REVENUE-MIX.md. Output: expected revenue mix (% per tier, $ per tier), total expected revenue with CI, and sensitivity of total revenue to price changes within each band.

### 7. Report and compare to existing pricing

Produce the report using REPORT-TEMPLATE.md. Only at the end, after the recommendation is fully derived, include a section comparing recommended pricing to existing pricing. This section names the gap, hypothesises why it exists (under-pricing for capture, over-pricing relative to value, legacy of original cost structure, positioning drift), and notes migration considerations — but does not retro-fit the recommendation toward the current price.

## When to push back

- **User provides only existing pricing and asks "is this right?"** Explain the skill works the other way: it derives a recommendation from segments and value, then compares. Proceed only if they want that.
- **User has no view of segments and resists segmenting** ("we just have customers"). Push back. One-price recommendations from this skill exist but are explicitly inferior to tiered structures, and the report will say so.
- **User wants a single number, not a range.** Refuse the framing. Confidence intervals are part of the deliverable; collapsing them hides what is actually known.
- **User wants a "quick estimate" without segment data.** Offer it, but caveat clearly: without segment evidence, the WTP distributions are modeled rather than observed, and the confidence intervals will be wide. Do not produce false precision.

## Tools

The analytical work in steps 3–6 typically requires Python (sampling distributions, computing Van Westendorp curves, bootstrap CIs, revenue mix optimisation). Each methodology file contains reference code that should be adapted to the specific case, not copy-pasted.

For market sizing, comparable category pricing (as reference-price evidence, not anchor), and segment evidence, use web search.
