# GLOSSARY.md

The vocabulary the skill uses. Pricing is a field with a lot of borrowed and overloaded terms — the point of this glossary is to fix the meaning of the ones the skill relies on.

## Core terms

**Segment** — a group of customers whose willingness to pay clusters separately from other groups, driven by one or more of: distinct value derived, distinct alternatives, distinct constraints (budget, procurement process), distinct reference prices. A segment is real to the extent that pricing it differently from neighbours produces more revenue than pricing it the same. See SEGMENTATION.md.

**Targeting** — the deliberate decision to serve some segments at a given price band and not others. A pricing recommendation that does not name who it is *not* for is incomplete.

**Positioning** — the segment-facing answer to "what is this, and why is it priced this way?" Positioning ties value to price band; it is the bridge between value analysis and what the customer experiences.

**Willingness to Pay (WTP)** — the maximum a single customer would pay before walking away from the purchase. Always distribution-valued in this skill — every segment has a WTP *distribution*, not a WTP number.

**Reservation price** — synonym for individual WTP.

**Reference price** — what a customer expects things "like this" to cost, formed by adjacent category experience and recent purchases. Distinct from WTP: a customer can be willing to pay $200 (their WTP) for something they expect to cost $50 (their reference), and they will feel ripped off at $150 even though it is below WTP. Reference prices shape the *Van Westendorp* "too cheap" and "too expensive" responses heavily.

**Value pool** — the total economic value the product creates for a customer or segment over the relevant time horizon. Often expressed as: hours saved × loaded labour rate, or revenue enabled, or cost avoided, or risk reduced × loss expected. The value pool is the ceiling on sustainable price.

**Capture rate** — the fraction of the value pool captured by the seller via price. Sustainable capture rates vary by category but are usually 10–30% for B2B SaaS, lower for commodities, higher for must-have, monopoly-position, or status goods.

## Distribution terms

**WTP distribution** — the probability distribution of WTP across customers in a segment. Usually right-skewed; lognormal is the default working assumption unless data suggests otherwise.

**Conversion at price p** — for a given price p, the fraction of customers in a segment who would purchase. Equals 1 – CDF(p) for the WTP distribution. Also written `S(p)` for survival function.

**Demand curve** — the conversion-at-price function plotted across p. Downward-sloping for any normal segment.

**Confidence interval** — a range expected to contain the true value with stated probability. Used here on three things: WTP distribution parameters (modeling uncertainty), Van Westendorp price points (sampling uncertainty), and total expected revenue (combined).

## Van Westendorp terms

The Price Sensitivity Meter (PSM), originally Peter van Westendorp, 1976.

**Too Cheap (TC) curve** — the cumulative fraction of respondents who say a given price or higher is "too cheap to trust." Decreasing in p.

**Cheap / Bargain (B) curve** — cumulative fraction who say a given price or higher is a "bargain." Decreasing in p.

**Expensive (E) curve** — cumulative fraction who say a given price or lower is "expensive but I'd consider it." Increasing in p.

**Too Expensive (TE) curve** — cumulative fraction who say a given price or lower is "too expensive to consider." Increasing in p.

**Point of Marginal Cheapness (PMC)** — intersection of TC and E. Below this, more people think the price signals poor quality than think it is acceptably expensive. Practical floor.

**Point of Marginal Expensiveness (PME)** — intersection of TE and B. Above this, more people consider it too expensive than consider it a bargain. Practical ceiling.

**Indifference Price Point (IPP)** — intersection of B and E. Equal numbers calling it cheap or expensive. Often interpreted as the "median acceptable price" or the typical price the market expects.

**Optimal Price Point (OPP)** — intersection of TC and TE. Minimises total resistance (the count of people who think it is too cheap or too expensive). The point most often quoted as "the optimum," though the framing is contested — see VAN-WESTENDORP.md.

**Range of Acceptable Prices (RAP)** — the interval [PMC, PME]. Prices in this range have majority acceptance.

## Banding terms

**Band** — a price range targeted at a specific segment. Typically described by `(low, mid, high)`.

**Tier** — a productized band, with feature, support, or capacity differentiation that gives customers a reason to self-select into it. Bands without differentiation are arbitrage targets.

**Tier spread** — the ratio between adjacent tier mid-prices. Below ~1.5x, tiers cannibalise. Above ~3x, tiers feel like different products (which may or may not be intended).

**Feature gating** — using product features to differentiate tiers. Effective when the gated feature is differentially valued across segments — useless if every segment wants it equally.

**Capacity gating** — using usage caps (seats, requests, storage) to differentiate. Effective when usage scales with willingness to pay.

## Revenue mix terms

**Conversion rate at tier price** — the fraction of a segment that purchases the tier at the offered price.

**Self-selection model** — the rule by which a customer chooses among offered tiers. Default model: pick the tier maximising perceived (value − price), reject all if no tier produces positive surplus.

**Cannibalisation** — high-WTP customers in a high segment choosing a lower tier because the value gap doesn't justify the price gap. The reason tier spread and feature gating matter.

**Expected revenue per tier** — sum across segments of (segment size × conversion at the tier × tier price), accounting for self-selection.

**Revenue mix** — expected revenue per tier as a percentage of total expected revenue. The "shape" of the business at the recommended pricing.

**Revenue sensitivity** — how total expected revenue changes for a 10% (or other) movement in tier prices. Indicates which prices are load-bearing.

## Things that are not in this glossary on purpose

- **"Premium," "luxury," "value-for-money"** — marketing words. Translate to band positioning before using.
- **"Affordability"** — undefined without specifying the segment.
- **"Fair price"** — every customer thinks the fair price is one they would pay. Use WTP, reference price, and capture rate instead.
- **"Sweet spot"** — usually means OPP without saying so. Say OPP.
