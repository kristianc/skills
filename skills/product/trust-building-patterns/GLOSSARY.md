# GLOSSARY.md

The vocabulary for trust work. Precise terms prevent vague diagnoses. "Users don't trust us" is not actionable. "The pricing page has no risk reduction signals and the cancellation flow is a roach motel" is.

## Core terms

**Trust signal** — a concrete, observable element in the product that reduces the user's perceived risk of a specific action. A logo bar is a trust signal. A money-back guarantee is a trust signal. "We're a great product" is not a trust signal — it's a claim without evidence. Trust signals must be verifiable or at minimum specific. The test: could a skeptical user confirm this independently?

*Common misuse:* Calling any positive-sounding copy a trust signal. "Join thousands of happy customers" is not a trust signal unless the number is specific and the happiness is evidenced.

**Trust moment** — a point in the user journey where the user must make a decision that requires trust. Entering an email address is a trust moment (will they spam me?). Entering a credit card is a trust moment (will they charge me unexpectedly?). Clicking "Deploy" is a trust moment (will this break production?). Not every interaction is a trust moment — only the ones where the user weighs risk before proceeding.

*Common misuse:* Treating the entire product as one trust decision. Trust is granular. A user might trust the product's competence but not its pricing honesty. Map the specific moments.

**Social proof** — evidence that other people have made the same trust decision and it worked out. Testimonials, case studies, usage numbers, logo bars, ratings, reviews. Social proof works because humans use others' decisions as a heuristic for their own. It fails when it's fake, generic, or irrelevant to the user's situation.

*Common misuse:* Displaying logos of companies that are not actually customers. Showing testimonials without verifiable attribution. Both destroy more trust than they build when discovered.

**Credibility marker** — evidence that the product or team is qualified to deliver on its claims. Domain expertise, team credentials, years in operation, patents, certifications, press coverage. Credibility markers answer "why should I believe you can do this?" as opposed to social proof which answers "have other people trusted you?"

*Common misuse:* Listing irrelevant credentials. A team member's degree from a prestigious university is a credibility marker for an education product, not for a CRM.

**Risk reduction** — any mechanism that lowers the cost of being wrong. Free trials, money-back guarantees, easy cancellation, data export, no credit card required. Risk reduction is the most powerful trust pattern because it makes trust partially unnecessary — even if the user's trust is misplaced, they can recover. The test: if the user decides this product isn't for them, what does it cost them to leave?

*Common misuse:* Offering a "free trial" that requires a credit card and auto-converts. This is risk transfer, not risk reduction — the risk of forgetting to cancel shifts from the company to the user.

**Dark pattern** — a user interface design that tricks users into doing something they didn't intend. Forced continuity, sneak into basket, confirmshaming, roach motels, misdirection, hidden costs, bait and switch. Dark patterns may temporarily boost conversion metrics but systematically destroy trust. They are the fastest way to accumulate trust debt.

*Common misuse:* Using "dark pattern" loosely for any design the user dislikes. Dark patterns are specifically deceptive — they exploit cognitive biases to produce actions the user would not choose if fully informed.

**Manufactured urgency** — creating artificial time pressure to force a decision. Countdown timers that reset, "only 3 left" when supply is unlimited, "this offer expires" when it doesn't. Manufactured urgency is a specific dark pattern that exploits loss aversion. Real urgency (a genuine limited-time offer with a real deadline) is not a dark pattern — it's honest communication.

*Common misuse:* Confusing real scarcity (event tickets, limited beta slots) with manufactured scarcity (SaaS seats that are unlimited by nature).

**Pricing transparency** — showing all costs, conditions, and limitations before the user commits. No hidden fees at checkout, clear feature gating between tiers, honest comparison tables that don't manipulate tier choice, clear cancellation terms. Pricing transparency answers "what will this actually cost me, including the costs of leaving?"

*Common misuse:* Showing the monthly price prominently while burying the fact that the displayed price requires annual commitment. The information is technically present but structurally hidden.

**Operational transparency** — visible evidence that the product is maintained, reliable, and honest about failures. Status pages, incident postmortems, changelogs, roadmap visibility, response time commitments. Operational transparency builds trust over time by demonstrating that the team handles problems honestly rather than hiding them.

*Common misuse:* A status page that always shows green, even during incidents users are experiencing. This destroys more trust than having no status page at all.

**Trust debt** — the accumulated erosion from small dishonest choices, analogous to technical debt. Each individually seems minor: a slightly exaggerated testimonial, a cancellation flow with one extra step, a comparison table that hides a competitor's advantage. But trust debt compounds. Users develop a background suspicion that makes every subsequent trust moment harder. Unlike technical debt, trust debt is often invisible to the team that created it — they see each choice in isolation, while the user experiences them as a pattern.

*Common misuse:* Treating trust debt as something that can be "paid down" with a single gesture. Trust debt requires systematic correction — finding and fixing every instance, not just adding a trust badge to the homepage.

**Conversion friction** — resistance that prevents a user from completing a desired action. Some friction is trust-related (asking for a credit card too early), some is mechanical (too many form fields), some is informational (not enough context to decide). This skill focuses on trust-related friction — the moments where the user would proceed if they trusted the product but hesitates because they don't.

*Common misuse:* Assuming all friction is bad. Some friction (confirming a destructive action, reviewing before payment) earns its keep by protecting the user. Removing trust-earning friction to boost conversion is how dark patterns are born.

**Trust moment** — see definition above. Worth emphasizing: the five canonical trust moments in SaaS are pre-signup, signup, post-signup, payment, and ongoing. Each has different trust needs. Mapping them is the first step of every audit.
