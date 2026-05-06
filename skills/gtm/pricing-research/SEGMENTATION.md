# SEGMENTATION.md

How to segment for pricing. Pricing segmentation is a different exercise from marketing segmentation, and confusing the two is the most common reason pricing analysis fails.

## The difference from marketing segmentation

**Marketing segmentation** groups customers by who they are and how to reach them — demographics, firmographics, channels, messaging. The test is "do they respond differently to outreach?"

**Pricing segmentation** groups customers by what they will pay. The test is "do they have different WTP distributions?"

These can be the same — large enterprises usually have higher WTP than freelancers, and they're also reachable through different channels. But often they are different. Two customers in the same firmographic bucket can have very different WTP if one is a casual user and the other is mission-critical. Two customers with very different demographics can have the same WTP if the value proposition lands the same way for both.

When in doubt, optimise for WTP. The whole point of segmenting is to charge different prices, and you can only do that if the segments differ in what they'll pay.

## What drives WTP differentiation

Five drivers, in rough order of impact:

### 1. Value at stake

How much value the product creates for this customer in the relevant period. A workflow tool used by 5 people creates roughly 5x the value of one used by one person. A risk-management product creates value proportional to the loss it prevents.

This is usually the strongest driver. If you can identify a quantitative value-at-stake variable (seats, revenue managed, transactions processed, hours saved), it is almost certainly a viable segmentation axis.

### 2. Alternatives

What the customer would do otherwise. Customers whose alternative is "do without" or "build it ourselves at huge cost" have higher WTP than customers with capable, cheap substitutes available.

### 3. Buyer sophistication and budget process

A line-of-business buyer with discretionary budget pays differently from a procurement-led buyer with formal RFP processes. The procurement buyer can be a high-WTP segment in absolute dollars, but extracts more discount per dollar of value.

Self-serve buyers behave differently from sales-assisted buyers. Self-serve has narrow WTP distributions clustered around reference prices; sales-assisted is wider and ranges higher.

### 4. Reference prices

What the customer thinks things "like this" should cost. A buyer coming from "I currently use a free tool" has reference price ≈ $0. A buyer coming from "I currently use Salesforce" has reference price ≈ enterprise software.

Different reference prices alone justify different segments even when value-at-stake is similar.

### 5. Constraints

Budget caps, currency, geography, regulation. A small business with a $50/month software budget is segment-distinct from one with a $500/month budget even if they derive similar value.

Hidden constraints matter too: "must be procurable on a credit card without approval" is a real WTP-shaping constraint at the boundary of $50–$200/month for many SaaS products.

## How to test if a segmentation is real

A proposed segmentation is real if it passes all four:

1. **Different WTP.** The segments must have meaningfully different WTP distributions. If the medians are within 30% of each other and the distributions overlap heavily, they are not pricing-distinct — they are marketing-distinct at best.

2. **Identifiable a priori.** You need to know which segment a customer is in *before* they buy, so you can show them the right tier. "Customers who churn quickly" might be a real segment but it can't drive pricing because you don't know who they are upfront.

3. **Addressable separately.** You need to be able to actually charge different prices. This is often handled via tier self-selection — gating features or capacity so the segments naturally choose different tiers — but if every segment ends up choosing the same tier, the segmentation collapsed in practice.

4. **Stable.** A segment that holds for a quarter and dissolves the next is too narrow to base pricing on. Pricing changes are slow and expensive; the segmentation has to outlast the implementation cycle.

If a proposed segmentation fails one of these, either reformulate it or merge it into a neighbour.

## How many segments

Two to five for the analysis. Outside this range, push back:

- **One segment** is the user refusing to segment. See SKILL.md "When to push back."
- **Two to three** is typical for early-stage products and clean B2C products.
- **Three to five** is typical for mature B2B SaaS and products with self-serve / mid-market / enterprise stratification.
- **Six+** usually means the user is segmenting by marketing personas rather than WTP. Force consolidation by asking "do segments A and B actually differ in what they would pay?"

## Common segment archetypes

Not every product fits one of these, but most do. Use them as a starting hypothesis and then test against the four criteria.

**B2B SaaS:**
- *Individuals / hobbyists* — single user, value at stake measured in hours, very low WTP, reference prices anchored on consumer software.
- *Small teams* — 2–20 users, line-of-business buyer with credit-card budget, reference prices anchored on prosumer software.
- *Mid-market* — 20–500 users, IT-influenced purchase, reference prices anchored on category leaders, expects volume discount.
- *Enterprise* — 500+ users, procurement-led, value-at-stake measured in deals/revenue/risk, reference prices anchored on enterprise software, expects custom pricing.

**B2C product:**
- *Casual / occasional user* — narrow use case, low frequency, low WTP, very price-sensitive.
- *Engaged user* — regular usage, valued routine, mid WTP.
- *Power user / enthusiast* — high engagement, identity tied to product category, high WTP, low price sensitivity.
- *Gift / occasion buyer* — distinct WTP distribution; pricing for them often differs from self-buyers.

**Services:**
- *Price shoppers* — comparing on price, low WTP, high churn.
- *Outcome buyers* — comparing on results, high WTP, willing to pay for premium.
- *Relationship buyers* — buying continuity and trust; WTP shaped by switching costs.

## Cannibalisation between segments

Even with a clean segmentation, real-world pricing has cross-segment leakage:

- **Downgrade leakage.** High-WTP customers pick the cheaper tier because the value differentiation doesn't justify the price gap. Mitigated by feature/capacity gating.
- **Identity collapse.** A small-business customer claims to be an individual to access the cheap tier. Mitigated by enforcement (license terms, technical limits) or by accepting it as cost of business.
- **Migration up/down.** Customers genuinely move between segments over time. Pricing must support the migration without punishing existing relationships.

Quantify expected leakage in REVENUE-MIX.md. Don't assume zero cannibalisation in the model — it always exists.

## Output of this step

For each segment:

- A short name (use it consistently throughout the report).
- The WTP-driving variables that put a customer in this segment.
- The estimated size of the segment in the addressable market, with confidence.
- The realistic alternatives this segment considers.
- The segment's reference prices (what categories they anchor on).
- A hypothesis about WTP median and CV, to be refined in step 3.

Write all of this down before moving on. The rest of the analysis depends on the segmentation being explicit and committed to.
