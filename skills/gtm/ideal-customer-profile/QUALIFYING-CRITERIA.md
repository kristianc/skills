# QUALIFYING-CRITERIA.md

How to build criteria that are observable, testable, and actually useful for qualifying prospects. The difference between an ICP that sits in a document and one that changes behavior is entirely in the quality of the criteria.

## The observable rule

Every qualifying criterion must be evaluable from one of these sources, in order of preference:

1. **Publicly available information** — company website, LinkedIn, job postings, press releases, app store listings, GitHub repos, regulatory filings. Best criteria can be checked before any contact.
2. **A 15-minute discovery call** — questions a sales rep or founder can ask early in the conversation without requiring trust or disclosure of sensitive information.
3. **Product signup data** — information collected during onboarding that reveals fit (team size, current tools, use case selection).

If a criterion requires months of relationship to evaluate, it is not a qualifying criterion — it is a retention predictor. Important, but different. Qualifying criteria must be usable *before* you invest in the prospect.

## Observable vs. inferrable

**Observable** means you can point to specific evidence. "Has a dedicated data team" is observable — check LinkedIn for data analyst, data engineer, or analytics manager titles at the company.

**Inferrable** means you're guessing based on proxies. "Values data-driven culture" is inferrable — you might guess it from job postings or blog content, but you can't confirm it without deep inside knowledge.

The distinction matters because inferrable criteria produce false positives. A company that posts about data-driven culture in their blog may or may not actually have one. A company with three data analysts on LinkedIn definitely has a data team.

**The test:** Can two different salespeople evaluate this criterion for the same prospect and reach the same answer? If reasonable people would disagree, the criterion is inferrable, not observable.

When a criterion is important but hard to observe directly, find the observable proxy:

| What you actually mean | Observable proxy |
|---|---|
| "They care about customer experience" | "Has a dedicated CX or Customer Success team of 3+" |
| "They're growing fast" | "Headcount grew 20%+ in the last 12 months (LinkedIn data)" |
| "They have budget for this" | "Currently paying for [comparable tool category] OR just raised Series A+" |
| "They have technical sophistication" | "Engineering team has public GitHub repos or uses CI/CD tools (visible in job postings)" |
| "They need this urgently" | "Posted a job for [relevant role] in the last 60 days" or "Current vendor announced EOL" |
| "They're not happy with their current solution" | "Switched [category] vendors in the last 18 months" or "Active in community forums asking about alternatives" |

## The four categories of criteria

### 1. Company characteristics

What the company is, independent of timing or situation. These are the most stable criteria and the easiest to evaluate from public data.

**What to specify:**
- Size (employees, revenue, or both). Use ranges, not vague buckets. "50-200 employees" is a criterion. "Mid-market" is not — it means different things in different contexts.
- Industry or vertical, if the product's value is industry-dependent. Be specific: "B2B SaaS" is more useful than "technology." Omit industry if the product is genuinely horizontal.
- Tech stack, if the product requires integration or technical compatibility. "Uses Salesforce as primary CRM" or "runs on AWS" is a criterion. "Modern tech stack" is not.
- Stage or funding, if it correlates with budget and need. "Series A to Series C" or "bootstrapped with $1M+ ARR" gives a decision-useful range.
- Geography, if relevant to product capabilities (language, compliance, time zone support).

**Common mistakes:**
- Making company size the primary criterion when situation matters more. A 500-person company with no data team is a worse fit for a data tool than a 50-person company with a dedicated analyst.
- Using industry as a proxy for what you actually mean. If the real criterion is "has a compliance requirement for audit trails," say that — it's more specific and more observable than listing regulated industries.
- Specifying criteria so narrowly that the addressable market is too small to sustain the business. If qualifying criteria match fewer than 500 companies in the world, either the criteria are too tight or the business won't work.

### 2. Situation characteristics

What's happening in the company's world that creates the need. Situation criteria are the most predictive of value fit but also the hardest to evaluate from the outside.

**What to specify:**
- The problem they're currently experiencing. Not the category of problem — the specific symptom. "Reporting takes the data team 2+ days per month to produce manually" is a situation. "Needs better reporting" is a category.
- What they're currently using and why it's insufficient. "Using Metabase but has outgrown the single-node deployment" tells you they've already tried, hit limits, and need something more. "No BI tool" tells you they might not know they need one.
- Team structure relevant to the product. "Has at least one person whose job includes [relevant function]" ensures someone will champion and use the product.
- A constraint or pressure that makes the status quo untenable. "Customer base has grown past the point where manual onboarding is feasible" describes a situation with genuine urgency.

**How to make situation criteria observable:**
- Job postings reveal team structure, tools, and priorities. A company hiring a "Head of Data" is in a different situation than one hiring a "Junior Analyst."
- Tech stack can often be identified from job postings, built-with tools, or public integrations.
- Company blog posts and conference talks reveal current challenges and priorities.
- Growth signals (funding announcements, hiring velocity, office expansions) are publicly visible.

### 3. Buyer characteristics

Who within the company buys, champions, and uses the product. Buyer criteria are evaluated during discovery, not from public data.

**What to specify:**
- Role and title of the typical buyer. Be specific enough to be useful: "VP of Engineering or Director of Platform Engineering" is better than "technical leader."
- Authority level. Can this person sign a contract, or do they need approval? The answer determines sales motion and deal velocity.
- Budget ownership. Does the buyer control discretionary budget, or does purchase require a formal procurement process? This affects speed to close.
- Technical capability. Does the buyer (or their team) have the skills to implement and use the product, or will they need professional services?

**Common mistakes:**
- Defining buyer characteristics that describe your ideal contact, not your actual buyer. If the product is bought by ops managers but you want to sell to executives, the ICP should reflect reality.
- Ignoring the champion vs. decision-maker distinction. The person who finds the product is often not the person who signs the contract. Criteria should address both.
- Assuming buyer characteristics are company characteristics. "Has a CTO" is a company characteristic. "CTO is actively involved in tooling decisions" is a buyer characteristic — and harder to verify.

### 4. Timing characteristics

What event has happened or is about to happen that creates urgency. Timing criteria are the most transient but also the strongest signal that a prospect will act.

**What to specify:**
- Trigger events that indicate active buying. "Just raised Series B" or "just hired first [relevant role]" are timing criteria.
- Seasonal or cyclical patterns, if applicable. "Q4 budget planning" or "annual compliance audit approaching" create predictable windows.
- Transition states. "Currently migrating from [old tool] to [new stack]" indicates a window where the prospect is actively rebuilding.
- Urgency signals. "Regulatory deadline in the next 6 months" or "current vendor announced end-of-life" create time pressure.

**How to make timing criteria observable:**
- Funding rounds are public (Crunchbase, press releases).
- Hiring for specific roles is public (LinkedIn, job boards).
- Vendor changes are sometimes visible (tech stack detection tools, case studies, community posts).
- Regulatory deadlines are public and affect entire industries simultaneously.

**The decay problem:** Timing criteria expire. A company that raised Series B twelve months ago is in a different state than one that raised last month. Specify the recency window: "raised Series B in the last 6 months" is a criterion with a shelf life. "Raised Series B" without a window is not useful — every Series C company also raised a Series B at some point.

## How to test criteria against real prospects

Once criteria are drafted, test them:

1. **Retrospective test.** Take the 5 best and 5 worst customers (or prospects). Evaluate each against every criterion. The best customers should match most qualifying criteria and few disqualifying ones. The worst should show the opposite pattern. If the criteria don't separate the groups, they're not predictive.

2. **Prospecting test.** Take 10 companies you've never evaluated. Apply the criteria using only publicly available information. For each company, you should be able to reach a clear qualify/disqualify decision in under 10 minutes. If you can't — either the criteria aren't observable enough, or they're too ambiguous.

3. **Inter-rater test.** Have two people independently evaluate the same 5 prospects against the criteria. They should agree on the qualify/disqualify decision for at least 4 of 5. If they don't, the criteria are ambiguous and need to be sharpened.

4. **Negative test.** Find 3 companies you're confident are NOT good fits. Confirm the criteria correctly disqualify them. If the criteria qualify companies you know are bad fits, the criteria are too loose.

## Handling criteria that are important but hard to observe

Some things that strongly predict success are genuinely hard to observe from outside: executive buy-in, internal data culture, willingness to change processes, quality of existing data. Don't discard these — handle them in one of two ways:

**Find the observable proxy.** Executive buy-in is hard to observe, but "the initiative is mentioned in the company's annual report or investor update" is observable. Internal data culture is hard to observe, but "has data team of 3+" is observable.

**Move it to discovery qualification.** Some criteria are only evaluable after a conversation. That's fine — categorize them as discovery criteria rather than targeting criteria. Targeting criteria filter the top of funnel (who to reach out to). Discovery criteria filter after first contact (who to continue pursuing). Both are part of the ICP, but they're used at different stages and should be labeled accordingly.

## Avoiding proxy criteria

A proxy criterion is one that correlates with what you actually care about but isn't the thing itself. Proxies are dangerous because the correlation breaks.

**Example:** "Enterprise company (1000+ employees)" as a proxy for "has budget for $50K+ annual contract." The proxy holds for most enterprises — but a 1000-person nonprofit might not have the budget, while a 200-person fintech might. If what you mean is "has budget for $50K+ annual contract," find a way to say that: "Currently spends $30K+/year on [category] tools (visible from tech stack or discoverable in first call)."

**The test for proxy criteria:** Ask "is there a company that matches this criterion but would be a bad fit?" and "is there a company that fails this criterion but would be a great fit?" If the answer to either is easily yes, you're using a proxy. Replace it with the underlying criterion, or add the proxy with a note that it's a proxy and what it's standing in for.

**When proxies are acceptable:** When the underlying criterion truly cannot be observed or proxied more directly, and the correlation is strong enough to be useful despite false positives and negatives. In this case, label the criterion as a proxy explicitly: "1000+ employees (proxy for sufficient budget — verify in discovery)." Honesty about proxies prevents them from calcifying into false precision.
