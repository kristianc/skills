# ICP-DOCUMENT.md

The deliverable format for the ICP, and guidance on writing each section so it's specific enough to use and not so narrow it excludes real customers.

## Deliverable structure

The ICP document has four sections:

1. Qualifying criteria
2. Disqualifying criteria
3. Narrative
4. Validation results

Each section serves a different audience and purpose. Qualifying criteria are for reps qualifying leads. Disqualifying criteria are for saying no. The narrative is for aligning the team on who the customer is. Validation results are for confidence that the ICP is grounded in reality.

## Section 1: Qualifying criteria

Organized into four categories (see QUALIFYING-CRITERIA.md for how to build each one):

**Company characteristics** — what the company is, independent of timing.
**Situation characteristics** — what's happening that creates the need.
**Buyer characteristics** — who buys, champions, and uses the product.
**Timing characteristics** — what event creates urgency.

### What good looks like

Good qualifying criteria share these properties:

- **Observable.** Each criterion can be evaluated from public data or a short discovery call. Two people evaluating the same prospect reach the same conclusion.
- **Specific enough to filter.** The criteria exclude at least 70% of the addressable market. If most companies pass all criteria, the criteria are too loose to be useful.
- **Broad enough to sustain the business.** The criteria must match enough prospects to support the revenue target. If qualifying criteria match fewer than your target number of customers, they're too tight.
- **Prioritized.** Not all criteria are equally important. Mark criteria as "must-have" (all must be true) or "strong signal" (most should be true). A prospect that matches all must-haves and most strong signals is a clear qualify. A prospect that misses a must-have is a clear disqualify regardless of other signals.
- **Actionable by non-experts.** A new sales rep in their first week should be able to apply the criteria without needing the founder to explain what they "really mean."

### Example: well-structured qualifying criteria

> **Must-have:**
> - B2B SaaS company, 50-500 employees
> - Has at least one dedicated data or analytics role (data analyst, data engineer, analytics manager)
> - Currently using a BI tool (Metabase, Looker, Tableau, Mode) or spreadsheet-based reporting
> - Annual software budget of $50K+ for data/analytics tools (discoverable via current tool stack)
>
> **Strong signals:**
> - Hired or posted a job for a data team lead in the last 6 months
> - Series A to Series C (or bootstrapped with $5M+ ARR)
> - Engineering team uses a modern data stack (dbt, Snowflake, BigQuery, Redshift)
> - Has expressed dissatisfaction with current reporting in public forums, reviews, or during outreach

### Example: poorly structured qualifying criteria

> - Mid-market companies
> - Tech-savvy teams
> - Values data-driven decisions
> - Growing and innovative
> - Needs better analytics

Every criterion here fails the observable test. None of them would produce agreement between two evaluators. "Mid-market" is undefined. "Tech-savvy" is subjective. "Values data-driven decisions" is unverifiable. "Growing and innovative" is what every company claims. "Needs better analytics" is a category, not a situation.

## Section 2: Disqualifying criteria

Disqualifying criteria are not the inverse of qualifying criteria. They are independent signals that a prospect will not succeed, even if they match the qualifying criteria on paper.

### What good looks like

Good disqualifying criteria are:

- **Specific and observable.** Same standard as qualifying criteria.
- **Grounded in evidence.** Each disqualifier should trace to actual churned customers or failed deals. "We disqualify companies with no internal champion because our last 5 churned accounts all lacked one" is grounded. "We disqualify companies that don't seem committed" is not.
- **Limited in number.** Three to seven disqualifiers is typical. More than ten suggests the qualifying criteria aren't specific enough — you're using disqualifiers to compensate for loose qualifiers.
- **Hard to game.** The best disqualifiers describe structural misfit, not attitude. "Requires on-premise deployment" is structural. "Not bought in to the vision" is attitude, and the prospect will tell you whatever gets the deal moving.

### Common disqualifiers by category

**Product misfit:**
- Requires a capability the product doesn't have and won't build (e.g., on-premise deployment, specific compliance certification, integration with a system you don't support).
- Primary use case is one the product handles as a secondary feature, not a core strength.
- Scale requirements exceed what the product can deliver (number of users, data volume, API throughput).

**Organizational misfit:**
- No one's job depends on the problem the product solves. The product will be adopted superficially and abandoned.
- Buying process requires vendor capabilities you don't have (security audit, SOC 2, enterprise SLA with 99.99% uptime).
- Decision-making requires consensus across too many stakeholders — deal velocity drops below what your sales motion supports.

**Behavioral signals from past failures:**
- "Buying for compliance checkbox only" — they need to tell an auditor they have the tool, not actually use it. Will never activate.
- "Currently in an RFP process with 5+ vendors" — unless you have enterprise sales capacity, the cost of competing in a formal RFP is disproportionate.
- "Wants to customize everything" — the product is a platform in their mind. Support cost will exceed revenue.

### Disqualifiers that look right but aren't

- **"Too small."** Small companies can be great customers if they're in the right situation. Disqualify on budget constraint if that's what you mean, not on size.
- **"Not enough budget."** Budget is created, not just found. If the value is real and the pain is acute, budget follows. Disqualify on willingness to pay (no pain, plenty of free alternatives) rather than stated budget.
- **"No current tool in the category."** This can mean they're unsophisticated — or it can mean they've been waiting for the right solution. Probe the situation before disqualifying.

## Section 3: Narrative

The narrative is a paragraph (3-6 sentences) describing the ideal customer in human terms. Its purpose is alignment — when someone reads it, they should immediately think of real prospects or customers who match.

### The narrative describes a situation, not a fictional person

**Wrong approach (persona):**
> "Meet Alex, a 35-year-old VP of Data at a Series B SaaS company. Alex is data-driven and collaborative, loves hiking on weekends, and is frustrated by the current state of reporting at their company. Alex needs a tool that's easy to use and powerful enough for their growing team."

This is useless. The age, hobbies, and personality traits don't affect whether the product delivers value. The description is so generic it could describe thousands of people who are not good customers.

**Right approach (situation):**
> "A B2B SaaS company between 50 and 200 employees that has grown past the point where the founding team can produce reports in spreadsheets. They've recently hired or are about to hire their first dedicated data person — a data analyst or analytics engineer. That hire has been asked to 'build real reporting' but inherited a fragmented setup: some Metabase dashboards nobody trusts, some Google Sheets that are manually updated weekly, and a data warehouse that was set up during a hack week and never properly maintained. They need a tool they can deploy this quarter, not a six-month data infrastructure project."

This narrative is useful because:
- It describes a situation that can be verified (team size, hire, current tools).
- It identifies the trigger event (new hire).
- It specifies the constraint (quarter, not six months) that determines speed-to-value requirements.
- It names the alternative (fragmented current state) that the product replaces.
- A sales rep reading this can immediately think "yes, I talked to a company like this last week" or "no, my prospects don't match this."

### Writing tips

- **Use present tense.** The narrative describes what's true about the customer now, not their history.
- **Include the trigger.** What happened that made them start looking? Without a trigger, the narrative describes a chronic condition, not a buying moment.
- **Name the current state concretely.** "Using spreadsheets" is more useful than "manual processes." Name the specific tools, processes, or workarounds.
- **State the constraint that shapes their decision.** Time, budget, technical capability, team size — whatever determines how they'll evaluate solutions.
- **Avoid aspirational language.** Don't describe what the customer wants to become. Describe where they are and what they need right now.

## Section 4: Validation results

The validation section documents how the ICP was tested against real data. It provides evidence that the criteria work — or honest acknowledgment that they haven't been fully tested yet.

### What to include

**Positive validation:**
- For each of the best 3-5 customers (or most promising prospects), show which qualifying criteria they match and which they don't. If the best customers match most criteria, the qualifying criteria are working.

**Negative validation:**
- For each of the worst 3-5 customers (or most failed deals), show which disqualifying criteria they match. If the worst customers trigger disqualifiers, the disqualifying criteria are working.

**Gap analysis:**
- Any best customer who fails a qualifying criterion is evidence the criteria might be too narrow. Note it and consider whether the criterion should be relaxed or changed to a "strong signal" rather than a "must-have."
- Any worst customer who passes all qualifying criteria and fails no disqualifying criteria is evidence the criteria are missing something. Note it and investigate what observable attribute would have predicted the failure.

**Confidence assessment:**
- State how many data points the validation is based on. "Tested against 15 customers with 12 months of data" is high confidence. "Tested against 3 beta users" is low confidence — honest and useful, but the ICP should be treated as a hypothesis to be refined, not a finished document.

### What honest validation looks like

> **Validation: 8 customers, 6 months of data.**
>
> *Positive:* All 3 highest-retention customers match all must-have criteria and 3+ of 4 strong signals. The strongest customer matched all criteria.
>
> *Negative:* 2 of 3 highest-churn customers triggered the "no internal champion" disqualifier. The third churned customer passed all criteria — investigation revealed they were using the product for a use case we don't support well (real-time dashboards). Added "primary use case is batch reporting, not real-time monitoring" as a disqualifier.
>
> *Gap:* One high-retention customer is a 30-person company, below our 50-employee minimum. They have 4 data people (disproportionate to company size). Consider changing the company size criterion to "50+ employees OR 2+ dedicated data roles."
>
> *Confidence:* Medium. The pattern is consistent but the sample is small. Revisit after 20 customers.

## How the ICP connects to other GTM artifacts

The ICP is not a standalone document. It feeds directly into:

- **Positioning.** The ICP's situation, trigger, and current state become the "before" in the positioning narrative. Positioning that doesn't reference the ICP's situation is positioning for nobody in particular.
- **Pricing.** The ICP's willingness to pay, value fit, and expansion potential inform tier structure and price levels. See the pricing-research skill.
- **Launch brief.** The ICP determines the launch audience, the channels, the messaging, and the success metrics. A launch that targets a broader audience than the ICP will underperform on conversion and overperform on vanity metrics.
- **Sales qualification.** The qualifying and disqualifying criteria become the BANT or MEDDIC inputs. Sales teams that qualify against ICP criteria have higher win rates and lower churn.
- **Product roadmap.** The ICP's needs, activation requirements, and expansion paths inform feature priority. Features that serve the ICP are higher priority than features that serve adjacent segments — unless the strategy is deliberately expanding the ICP.
- **Content and demand generation.** The ICP's situation, trigger events, and current solutions determine what content to produce (write about the problems they have and the transitions they're going through, not about your product's features).
