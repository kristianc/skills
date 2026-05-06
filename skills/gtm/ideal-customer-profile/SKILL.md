---
name: ideal-customer-profile
description: Define who the product is actually for by working from capabilities, market signals, and value delivery to produce segment-level customer profiles with qualifying and disqualifying criteria. Use when the user wants to define their ICP, figure out who to sell to, segment their market, qualify leads, understand who gets the most value from the product, or stop selling to everyone.
---

# Ideal Customer Profile

Define who the product is for — not in demographic terms, but in terms of who gets disproportionate value from what the product actually does today.

An ICP is not a persona. It's a set of qualifying and disqualifying criteria specific enough that anyone in the company can look at a prospect and say "this is us" or "this isn't us." See GLOSSARY.md for precise definitions and common misuses of every term.

## Principles

- **Start from the product, not the market.** The ICP describes who gets the most value from what exists, not who you wish would buy it.
- **Disqualifying criteria are as important as qualifying ones.** Knowing who to say no to prevents the slow death of serving everyone poorly.
- **ICPs are testable.** Every criterion must be observable — evaluable from public information or a short discovery call. See QUALIFYING-CRITERIA.md.
- **One product can have multiple ICPs.** But they must be prioritized, and the primary ICP drives all defaults (pricing, messaging, feature priority, support level).

## Process

### 1. Audit current value delivery

Establish who the product already works for before defining who it's for. Cover:

- **Best customers** — highest retention, highest expansion, lowest support cost. Pre-launch: most engaged beta users or most excited prospects.
- **What they share** — not demographics. Situations: what problem, what they used before, what about their situation makes this product disproportionately valuable.
- **Worst customers** — highest churn, most support tickets, longest sales cycles, misaligned feature requests. What do they share?
- **Product reality** — what the product does well today, not the vision. Where it's strong, where it's duct tape.

If the user has no customers, work from capabilities and the problem. Who has this problem most acutely? Who has the fewest alternatives? Who would get value fastest?

### 2. Identify candidate segments

From patterns in step 1, identify 3-5 candidate segments. For each:

- **Situation** — what's happening in their world that creates the need.
- **Trigger event** — what makes them start looking (a hire, a funding round, a broken process).
- **Current solution** — what they do today (competitor, manual process, nothing).
- **Value delivered** — what changes in week 1, month 1, quarter 1.
- **Why they'd leave** — what would cause churn. This reveals whether the product can retain them.

Present candidates. Ask the user to react — which feel right, which are surprising, which are they avoiding.

### 3. Score and prioritize

Evaluate each segment across eight dimensions using SCORING-FRAMEWORK.md: value fit, reach, willingness to pay, speed to value, retention potential, expansion potential, support cost, strategic fit. Score each 1-5 with concrete evidence.

The highest total is the strongest candidate, but a single critical low score can override. See SCORING-FRAMEWORK.md for weighting and override rules.

### 4. Define the ICP

For the primary segment (and optionally secondary), produce the deliverable described in ICP-DOCUMENT.md:

- **Qualifying criteria** across four categories — see QUALIFYING-CRITERIA.md for how to build observable, testable criteria.
- **Disqualifying criteria** — observable signals of misfit.
- **Narrative** — a paragraph describing a situation, not a fictional person.
- **Validation results** from step 5.

### 5. Validate

Test the ICP against real data:

- Best 3 customers (or most promising prospects) — do they match the qualifying criteria?
- Worst 3 customers (or most skeptical prospects) — do they match the disqualifying criteria?
- Last 3 lost deals — which criteria did those prospects violate?

If the ICP doesn't predict past outcomes, revise. Iterate until it does.

## Rules

- Never define an ICP in purely demographic terms. "Mid-market SaaS companies" is a market segment, not an ICP. Add the situation, trigger, and need.
- Every qualifying criterion must be observable. See QUALIFYING-CRITERIA.md for the observable vs. inferrable distinction.
- The ICP must include disqualifying criteria. If it doesn't say who's NOT a fit, it's not specific enough.
- Don't build an aspirational ICP. The profile describes who gets value from the product as it exists, not the roadmap product.
- If the user can't identify best and worst customers, start there. The exercise doesn't work without ground truth.
