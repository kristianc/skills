# GLOSSARY.md

The vocabulary the skill uses. ICP work is plagued by terms that sound precise but mean different things to different people. The point of this glossary is to fix meaning so the deliverable is actually usable.

## Core terms

**Ideal Customer Profile (ICP)** — a set of qualifying and disqualifying criteria that describe which prospects will get disproportionate value from the product as it exists today. The output is a decision tool: given a prospect, anyone in the company can evaluate the criteria and say "this is us" or "this isn't us."

An ICP is NOT:
- **Not a persona.** A persona is a fictional character with a name, photo, and backstory. An ICP is a set of testable criteria. Personas describe who someone *is*; ICPs describe what *situation* someone is in. A persona says "Meet Sarah, VP of Marketing at a mid-size B2B company who values data-driven decisions." An ICP says "B2B SaaS company, 50-200 employees, that has outgrown spreadsheet reporting and hired or is hiring their first data team lead."
- **Not a demographic.** "Series B SaaS companies" is a firmographic filter, not an ICP. It tells you nothing about whether the prospect has the problem you solve or would get value from the product.
- **Not an aspiration.** The ICP describes who the product serves well *today*, not who it will serve after the next three quarters of roadmap. An aspirational ICP causes the company to sell to customers who churn, because the product doesn't actually deliver for them yet.
- **Not a TAM definition.** Total addressable market is everyone who *could* buy. The ICP is everyone who *should* buy — and the difference between those sets is where most wasted GTM effort lives.

**Qualifying criteria** — observable attributes that indicate a prospect is likely to get high value from the product, retain, and expand. "Observable" means evaluable from public information, a company's website, a LinkedIn profile, or a 15-minute discovery call. See QUALIFYING-CRITERIA.md.

**Disqualifying criteria** — observable attributes that indicate a prospect is likely to churn, consume disproportionate support, or require product changes that don't serve the broader customer base. Disqualifiers are not the absence of qualifiers — they are independent signals. A company can match every qualifying criterion and still be disqualified (e.g., qualifies on size and situation, but is disqualified because they require on-premise deployment the product doesn't support).

**Activation moment** — the point at which a customer first experiences the core value the product delivers. Not signup. Not onboarding complete. The moment where the product does the thing the customer bought it for. For a reporting tool, activation might be "built their first dashboard from live data." For a CRM, it might be "closed their first deal tracked in the system." The activation moment matters to ICP work because speed to activation varies dramatically across segments — and segments that can't reach activation within a reasonable window are poor ICP candidates regardless of other fit.

**Trigger event** — something that happens in a prospect's world that creates the need or opens the budget. A trigger event is not a chronic condition — it's a state change. "They need better reporting" is a chronic condition. "They just hired their first data analyst" is a trigger event. The distinction matters because trigger events create urgency: the prospect is actively looking for a solution, not passively aware of a problem. Good ICP criteria include at least one trigger event.

Common trigger events: new hire into a relevant role, funding round, broken process that caused a visible failure, compliance deadline, competitor move, executive mandate, outgrowing current tooling, vendor sunsetting a product they depend on.

**Situation** — the combination of circumstances that makes a prospect a fit. Situation is broader than trigger — it includes the chronic conditions, the team structure, the tech stack, the growth trajectory, and the trigger event that lit the fuse. An ICP describes a situation, not a type of company.

Example of a demographic description: "Mid-market SaaS company, 100-500 employees."
Example of a situation description: "A SaaS company that has grown past the point where the founding team can manage customer data in spreadsheets, has just hired or is hiring their first RevOps or Sales Ops person, and is evaluating CRMs for the first time or replacing one that was adopted ad hoc and never properly configured."

The situation description is longer. That's the point — it contains the information needed to qualify a prospect. The demographic description does not.

**Value fit** — the degree to which the product solves the prospect's primary or secondary problem. A prospect with high value fit gets the product's core value proposition; a prospect with low value fit is buying it for a peripheral feature or a use case the product handles poorly. Value fit is the single most predictive criterion for retention. See SCORING-FRAMEWORK.md.

**Expansion potential** — the degree to which a customer's usage and spend can grow after initial purchase. Expansion can be horizontal (more seats, more teams, more use cases) or vertical (moving to higher tiers, adding premium features). Segments with high expansion potential are worth more per acquisition dollar than segments that plateau quickly.

**Strategic fit** — the degree to which serving a prospect well makes the product better for the next prospect like them. High strategic fit means the features, integrations, and support patterns required by this segment are ones the product should be building anyway. Low strategic fit means serving this segment requires bespoke work that doesn't generalise.

## What good criteria look like vs. what bad criteria look like

| Bad criterion | Why it's bad | Good criterion |
|---|---|---|
| "Values data-driven decisions" | Not observable. Everyone claims this. | "Has a dedicated analytics or data role" |
| "Mid-market company" | Too vague — mid-market means different things to different people | "50-500 employees, $5M-$100M ARR" |
| "Needs better reporting" | Chronic condition, not a situation | "Currently using spreadsheets or a BI tool they've outgrown, and has a mandate to improve reporting this quarter" |
| "Tech-savvy team" | Not observable, not specific | "Engineering team uses CI/CD, has deployed at least one SaaS tool via API integration" |
| "Growing company" | Every company says they're growing | "Headcount has increased 20%+ in the last 12 months (visible on LinkedIn)" |
| "Frustrated with current solution" | You can't observe frustration from outside | "Currently using [specific competitor] and has posted in forums about [specific limitation]" |

## Terms people confuse

**ICP vs. buyer persona.** The ICP describes the *company and situation*. The buyer persona describes the *person who buys* within that company. Both are useful, but they answer different questions. The ICP answers "should we be selling to this company at all?" The buyer persona answers "once we've decided to sell, who do we talk to and what do we say?" Define the ICP first. Personas without an ICP are fictional characters shopping for a product without knowing if they need it.

**ICP vs. target market.** The target market is the set of companies you intend to pursue. The ICP is the subset that will actually succeed. The gap between target market and ICP is where most wasted sales and marketing effort lives.

**Qualifying vs. scoring.** Qualifying is binary: does this prospect meet the criteria, yes or no? Scoring is continuous: among qualified prospects, how strong is the fit? The ICP defines the qualifying line. Lead scoring systems should use ICP criteria as inputs, not replace them.
