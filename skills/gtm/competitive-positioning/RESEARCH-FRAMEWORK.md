# RESEARCH-FRAMEWORK.md

How to research competitors systematically — what to look at, how to distinguish claims from reality, and how to document findings consistently.

## The non-negotiable rule

**Use web search for every competitor.** Do not rely on memory, training data, or general knowledge for competitor details. Positioning built on stale information is positioning built on fiction. Competitors ship features, change pricing, pivot messaging, and get acquired. The research must reflect what is true today, not what was true when the model was last trained.

For each competitor, search for: their current homepage, their pricing page, their documentation or product pages, their changelog or release notes, recent reviews (G2, Capterra, Reddit, Hacker News), and community discussions. If you cannot find current information on a competitor, say so — a gap in research is better than a fabrication.

## What to research for each alternative

### 1. Claims — what they say about themselves

Look at:

- **Homepage hero copy.** The first sentence on the homepage is what they think their positioning is. Capture it verbatim.
- **Product pages / feature pages.** How they describe what the product does. Note the axes they emphasize (speed, ease, power, integrations, etc.).
- **Pricing page.** Not just the numbers — how they frame the tiers, what language they use ("starter," "pro," "enterprise"), what features they gate, whether they publish prices at all.
- **Sales collateral.** If available — case studies, whitepapers, comparison pages ("us vs. them"). These reveal which competitors they consider threats and which axes they think they win on.
- **Job postings.** What they're hiring for reveals what they're building next. A sudden batch of ML engineer postings tells you something about their roadmap.

Capture exact quotes. "We help teams move faster" is different from "Sub-second query performance across petabyte-scale datasets." The specificity (or vagueness) of their claims is itself informative.

### 2. Reality — what they actually deliver

Look at:

- **Documentation.** The docs reveal what the product actually does versus what the marketing says. Gaps between the two are common: a claimed "AI-powered" feature that turns out to be a rules engine, an "enterprise-ready" platform whose docs show no SSO integration.
- **Changelogs / release notes.** Shipping velocity is visible here. A product that shipped 3 features in the last 12 months is different from one that ships weekly. Also reveals what they're investing in — which is a better signal of strategic direction than their blog posts.
- **Public reviews (G2, Capterra, TrustRadius).** Filter for recent reviews (last 12 months). Look for patterns in complaints, not individual reviews. One person complaining about setup time is noise; fifteen people complaining about setup time is signal. Note what customers praise, too — it reveals which axes the product actually delivers on.
- **Community forums, Reddit, Hacker News.** Unfiltered user sentiment. Search for "[competitor name] review," "[competitor name] vs," "[competitor name] problems." Community discussions surface the gaps between claims and reality that curated review sites miss.
- **Integrations / ecosystem pages.** The breadth and depth of integrations is verifiable. Count them. Check whether they're maintained or stale.
- **Status pages / incident history.** For infrastructure and platform products, uptime claims are verifiable against their status page history.

### 3. Pricing model

Beyond the numbers:

- **Pricing metric** — per seat, per event, per GB, per project, flat rate. The metric reveals who the pricing punishes at scale (per-seat punishes large teams, per-event punishes high-volume use cases).
- **What's gated** — which features are in which tier. Feature gating reveals what they consider premium (and by implication, what their high-WTP customers value).
- **What's hidden** — do they publish prices? If not, it usually means enterprise sales motion, custom pricing, or prices high enough to scare away comparison shoppers.
- **Free tier / trial structure** — what you can do for free, for how long, and what you lose when the trial ends.

### 4. Structural constraints

This is the most important part of the research and the hardest. You're looking for decisions the competitor has made that durably limit what they can do.

**Architecture:**
- What is their core technical architecture? (Monolith vs. microservices, relational vs. columnar, cloud-native vs. legacy, single-tenant vs. multi-tenant.)
- What does this architecture make easy and what does it make hard?
- Would changing it require a rewrite or a migration that breaks existing customers?

**Business model:**
- How do they make money? (Subscription, usage, marketplace, services, ads.)
- Which customer segment generates the majority of their revenue?
- Would serving a different segment require them to change their pricing, their sales motion, or their support model?

**Market commitments:**
- Who are their biggest customers? What have they promised them?
- Are they publicly traded with quarterly revenue commitments?
- Have they raised a round with specific growth expectations attached to a particular market?

**Team and culture:**
- What do their job postings suggest about their technical direction?
- What do their engineering blog posts reveal about what they consider hard problems?

The output of structural constraint analysis is a sentence like: "Competitor X built on PostgreSQL and serves enterprise customers with dedicated instances. This means they can't offer the sub-second cross-tenant analytics that a columnar, multi-tenant architecture enables, without a fundamental rebuild."

### 5. Who they're optimized for

Every product is best for someone. Identify:

- The customer profile that gets the best experience (fastest setup, most features, most attention, best pricing).
- The customer profile that gets the worst experience (features they need are missing, pricing punishes their usage pattern, support tiers exclude them).

This reveals the gap: who is underserved by this competitor? That gap may be where the user's product wins.

## Distinguishing structural from temporary

The single most common mistake in competitive positioning is treating a temporary gap as a structural constraint. The test:

| Question | Structural | Temporary |
|---|---|---|
| Could they fix it with a team of 5 in a quarter? | No | Yes |
| Would fixing it undermine their existing business? | Yes | No |
| Would fixing it require rebuilding a core system? | Yes | No |
| Would fixing it alienate their primary customer segment? | Yes | No |
| Have they known about it for 2+ years and not fixed it? | Likely structural | Check why |

If you can't determine whether a constraint is structural, label it "uncertain — may be temporary" and don't build positioning on it. Better to have fewer positioning axes built on durable constraints than more axes built on assumptions.

## Documenting findings

For each competitor, produce a consistent research summary:

```
## [Competitor Name]

**What they claim:** [Homepage positioning, key claims, exact quotes]

**What they deliver:** [Evidence from docs, reviews, community — where claims hold up, where they don't]

**Pricing:** [Model, metric, tiers, gating, notable structure]

**Structural constraints:** [Architecture, business model, and market decisions that limit them]

**Optimized for:** [Which customer gets the best experience]

**Underserves:** [Which customer gets a poor experience or is excluded]

**Sources:** [URLs consulted, dates accessed]
```

Always include sources. Positioning decisions will be revisited; when they are, the team needs to know where the original research came from and how fresh it was.

## When research is thin

Sometimes you can't find enough current information about a competitor. This happens with:

- Pre-revenue startups (no reviews, thin docs, marketing-only website).
- Enterprise products with no public pricing or documentation.
- Open-source tools with no commercial entity.

When research is thin, say so explicitly. Mark the competitor's assessment as "low confidence" and note what you couldn't find. Do not fill gaps with assumptions — a positioning document that says "we don't know enough about Competitor X to position against them" is more useful than one that positions against a hallucinated version of Competitor X.
