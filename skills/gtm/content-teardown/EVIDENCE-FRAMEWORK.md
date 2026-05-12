# EVIDENCE-FRAMEWORK.md

How to evaluate the quality of evidence and arguments in written content. Evidence quality is what separates content that informs from content that merely asserts — and it's the primary driver of whether expert audiences trust or dismiss what they read.

---

## The evidence hierarchy

Not all evidence is equal. This hierarchy ranks evidence types from strongest to weakest. Stronger evidence earns stronger claims. Weak evidence supporting a strong claim is worse than no evidence at all — it signals that the writer knows the claim needs support but can't find any.

### Tier 1: Original data

The writer generated this data themselves — ran the experiment, surveyed the customers, analyzed the logs, counted the outcomes. Original data is the strongest evidence because it can't be found elsewhere and can be interrogated for methodology.

**What good looks like:** "We A/B tested our pricing page for 12 weeks across 4,200 visitors. The version with annual pricing shown first converted at 34% vs. 22% for monthly-first. Here's what we controlled for."

**What weak looks like:** "We saw improvements after making changes to our pricing page." (What changes? How much improvement? What timeframe? What's the baseline?)

### Tier 2: Named case study

A specific, named example with enough detail to be verifiable. The company is named, the outcome is specific, the timeframe is stated. The reader could, in theory, confirm this.

**What good looks like:** "When Stripe launched their pricing calculator in 2019, their self-serve conversion rate increased 12% within the first quarter, according to their engineering blog post from March 2020."

**What weak looks like:** "A leading fintech company improved conversions after adding a calculator to their pricing page." (Which company? By how much? When? How do you know?)

### Tier 3: Specific example

A concrete, detailed example that illustrates the point — but without the verifiability of a named case study. Still specific enough to be useful.

**What good looks like:** "Consider a 50-person SaaS company spending $8K/month on support tooling. If they can reduce ticket volume by 30% through better onboarding docs, that's $2,400/month in direct savings — enough to fund one contractor."

**What weak looks like:** "Companies can save money by improving their documentation."

### Tier 4: Logical argument

Reasoning from established principles or widely accepted premises to a conclusion. No empirical evidence, but the logic is sound and the premises are defensible.

**What good looks like:** "If your average deal size is $500/year and your sales cycle takes 3 weeks of a rep's time, the unit economics don't support outbound sales. The math: a rep costs $6K/month fully loaded and can run ~6 cycles simultaneously, so your CAC floor is $3K — 6x the annual contract value."

**What weak looks like:** "Outbound sales don't work for low-ACV products." (Why not? Where's the threshold? What assumptions drive this?)

### Tier 5: Anecdote

A personal story or observation that illustrates a point. Useful for making abstract claims concrete, but doesn't prove anything beyond "this happened to me once."

**What good looks like:** "Last year I spent three months trying to implement usage-based pricing. We rolled it back after customer complaints about unpredictable bills outweighed the revenue upside. Here's what I'd do differently." (Specific, honest about limitations, learns from it.)

**What weak looks like:** "I've seen companies struggle with usage-based pricing." (Which companies? What happened? What do you mean by "struggle"?)

### Tier 6: Assertion

A claim presented as fact without supporting evidence. Sometimes assertions are fine — commonly accepted facts don't need citations. But contested or surprising claims that are merely asserted lose credibility.

**What good looks like (acceptable assertion):** "Most SaaS companies price in tiers." (Commonly known, doesn't need a source.)

**What weak looks like:** "Companies that use value-based pricing grow 3x faster." (This is a specific, testable claim presented as fact. Where's the data?)

### Tier 7: Vague claim

The weakest form of evidence — claims supported by unspecified authority. "Studies show," "experts agree," "research suggests," "it's widely known that." These phrases actively damage credibility because they signal that the writer either can't find real evidence or is hoping you won't ask for it.

**Examples to flag:** "Studies show that..." (Which studies? By whom? Published where?) "Research suggests..." (Whose research? What methodology?) "Experts agree..." (Which experts? Do they actually agree, or did one expert say this once?) "It's widely known that..." (If it's widely known, it should be easy to cite a source.)

---

## Evaluating whether a conclusion is earned

A conclusion is "earned" when it follows necessarily — or at least plausibly — from the evidence presented. An unearned conclusion is one that leaps beyond what the evidence supports.

### Tests for earned conclusions

- **The removal test:** Remove the conclusion and read just the body. Does the evidence point naturally to this conclusion, or could it support several different conclusions equally well? If the evidence is ambiguous, the conclusion should acknowledge that.
- **The scope test:** Does the conclusion match the scope of the evidence? Evidence from one company supports conclusions about that company. It doesn't support conclusions about "all companies" without additional reasoning.
- **The alternative test:** Is there a different conclusion that fits the same evidence? If yes, does the writer address why their conclusion is more likely? Ignoring plausible alternatives is a sign of motivated reasoning.
- **The surprise test:** Would the conclusion surprise the writer's critics? If the conclusion is exactly what someone with this writer's known biases would say, the evidence needs to be especially strong to be convincing.

### Common patterns of unearned conclusions

- **Anecdote to universal:** "This worked for us, therefore it will work for you." Personal experience is Tier 5 evidence — it supports "this might work" conclusions, not "this will work" conclusions.
- **Correlation to causation:** "We changed X, then Y improved, therefore X caused Y." Without controlling for other variables, this is a story, not a proof.
- **Survivorship bias:** "These 5 successful companies all did X, therefore X leads to success." What about the 500 companies that did X and failed? The evidence only includes survivors.
- **Cherry-picked timeframe:** "Revenue grew 200% in Q3." What happened in Q4? Starting and ending dates for data can be chosen to tell any story.
- **False precision:** "This approach is 73% more effective." Precision signals rigor, but only if the methodology supports it. A made-up precise number is worse than an honest range.

---

## Evidence quality and audience trust

Different audiences have different evidence thresholds. Matching evidence quality to audience expertise is part of audience fit.

### Beginner audiences

Accept anecdotes and logical arguments more readily. Original data and named case studies are appreciated but not expected. The primary trust signal is clarity — does the writer explain the reasoning clearly enough that a newcomer can follow it?

### Intermediate audiences

Expect at least Tier 3-4 evidence for key claims. They'll notice vague claims and unsupported assertions. They trust writers who show their work and acknowledge limitations.

### Expert audiences

Demand Tier 1-2 evidence for non-obvious claims. They'll actively evaluate methodology, look for cherry-picking, and test conclusions against their own experience. Vague claims or unsupported assertions don't just fail to persuade — they actively damage the writer's credibility. For expert audiences, one unearned claim can discredit the entire piece.

### The mismatch problem

Using Tier 6-7 evidence with an expert audience is the most common evidence-audience mismatch. The writer treats "everyone knows this" claims as established fact, but the expert reader knows the reality is more nuanced. Conversely, over-citing obvious claims for a beginner audience creates unnecessary friction — they trust you on the basics and want you to get to the point.

---

## The difference between "interesting" and "true"

Content can be interesting without being true, and true without being interesting. Both are valid — but they serve different purposes and should be labeled honestly.

- **Interesting and true:** The best content. Original insight backed by evidence. Rare and valuable.
- **Interesting but unproven:** Hypotheses, thought experiments, frameworks, provocations. Valuable when labeled honestly ("I think..." "Here's my hypothesis..." "I can't prove this, but..."). Dangerous when presented as fact.
- **True but uninteresting:** Known facts, established wisdom, textbook summaries. Has its place — explainers, reference content, introductions for new audiences. But don't dress it up as insight.
- **Neither interesting nor true:** Vague assertions of conventional wisdom. "Companies need to focus on customer experience." True-ish, obvious, and adds nothing. This is the content that fails the delete test.

The writer's job is to know which quadrant they're in and be honest about it. "Here's what I think but can't prove" is more trustworthy than "here's what I know" when the evidence doesn't support certainty.
