# SCORING-RUBRIC.md

Three axes for evaluating trust at each trust moment. Each axis is scored 1-5. The axes are independent — a page with strong social proof can still score 1 on honesty if it uses dark patterns.

**Trust blocker rule:** An honesty score below 3 on any trust moment is a blocker. Fix it before improving signal strength or risk reduction. A single dishonest moment poisons all the authentic ones — users don't average trust signals, they weight the worst one.

---

## Axis 1: Signal strength

Are trust signals present, relevant, and credible at this trust moment?

### Score 1 — No signals

No trust signals of any kind. The user must decide to trust the product based solely on the product's claims about itself. No social proof, no credibility markers, no third-party validation, no evidence.

**Examples at this level:**
- A pricing page with no testimonials, no logos, no usage numbers, no security badges — just the price and a "Buy now" button
- A signup page with no indication of who else uses the product or why they should be trusted
- An about page that says "We're passionate about solving this problem" with no team information, no background, no credentials

**Why this is a 1:** The user has nothing external to anchor their trust. Every product claims to be good. Without evidence, the user must take a leap of faith — and most won't.

### Score 2 — Signals present but generic

Trust signals exist but could apply to any product. Generic testimonials, meaningless badges, round numbers. The signals don't make this product specifically more trustworthy.

**Examples at this level:**
- A testimonial that says "Great product! Highly recommend." — John D., no company, no specifics
- "Trusted by thousands of customers" — unverifiable, round, meaningless
- A "Secure" badge that's self-issued, not from a recognized authority
- Logos that are integration partners displayed as if they were customers

**Why this is a 2:** Signals are present so the page isn't naked, but they don't withstand scrutiny. A skeptical user reads generic signals as filler and may become more suspicious than if no signals were present.

### Score 3 — Credible but incomplete

Some trust signals are authentic and specific, but they don't cover the user's actual concerns at this moment. A testimonial about ease of use on the pricing page (where the concern is cost, not usability). Security badges but no social proof. Logos but no specifics about what those companies actually use the product for.

**Examples at this level:**
- Real testimonials with full attribution, but all from the same customer type — a startup user can't see themselves
- Legitimate usage numbers, but no context: "12,847 teams" — doing what? in what industry?
- SOC 2 badge on the homepage but no data handling information near the signup form
- Strong logo bar but no case studies or quotes to add depth

**Why this is a 3:** The signals are honest, which is good. But they're misplaced or incomplete — they don't address what the user is worried about at this specific moment. Trust is context-dependent.

### Score 4 — Strong and relevant

Trust signals are authentic, specific, and matched to the user's concerns at this trust moment. Social proof at the decision point. Security signals near data-sensitive actions. Pricing transparency at the payment step. Different customer types represented so multiple visitor profiles can see themselves.

**Examples at this level:**
- Testimonials near the signup form from customers describing easy onboarding
- Usage numbers that are specific and meaningful: "Processing 2.3M events/day for 12,847 teams"
- Pricing page with clear comparison, no hidden fees, and a testimonial about ROI
- Case studies linked from the logo bar, each telling a specific story
- SOC 2 and GDPR badges near the signup form where users are about to enter personal data

**Why this is a 4 (not 5):** The signals are strong and well-placed, but there may be gaps. The ongoing trust moment (status page, changelog, incident history) might not be addressed. Or one customer segment isn't represented in social proof. The foundation is solid but not comprehensive.

### Score 5 — Comprehensive and contextual

Every trust moment has relevant, authentic, credible signals. Social proof is segmented by customer type. Trust signals are placed at the point of decision, not in a disconnected section. Security and privacy are addressed where data is collected. Pricing is transparent with no hidden costs. The ongoing relationship has visible operational transparency. Different user personas encounter signals relevant to their specific concerns.

**Examples at this level:**
- Pre-signup: segmented testimonials, specific numbers, press coverage with links, team page with domain expertise
- Signup: low friction, no credit card, clear next steps, social proof at the form
- Post-signup: immediate value delivery, data transparency, accessible support
- Payment: complete pricing visibility, honest comparison, easy cancellation, data export
- Ongoing: public status page, incident communication history, active changelog, roadmap visibility

**Why this is a 5:** Every trust moment is addressed. Signals are authentic, specific, contextual, and comprehensive. The user has evidence at every point where they need to decide to trust.

---

## Axis 2: Honesty

Is anything misleading, hidden, or manipulative? This is the most important axis — dishonesty is a trust blocker regardless of how strong other signals are.

### Score 1 — Actively deceptive

The product uses dark patterns, makes false claims, or deliberately misleads users. Fake testimonials, manufactured urgency, hidden costs, bait and switch, forced continuity. The design is optimized to trick users into actions they wouldn't choose if fully informed.

**Examples at this level:**
- Countdown timer that resets on page reload
- "Only 3 spots left" for a SaaS with unlimited capacity
- Testimonials with stock photos and fabricated names
- Free trial that auto-converts with no warning
- Cancellation that requires calling a phone number

**Why this is a 1:** The product is designed to deceive. Every trust signal on the site is suspect because the user (correctly) concludes the company prioritizes its conversion metrics over user welfare. One deceptive element casts doubt on everything else.

### Score 2 — Misleading by omission

No outright lies, but important information is structurally hidden. Annual pricing displayed as monthly without clear labeling. Limitations buried in fine print. Marketing claims that the ToS contradicts. Features described without tier restrictions. The information is technically available but designed to be missed.

**Examples at this level:**
- Pricing page shows "$9/mo" but checkout reveals this requires annual commitment (actual monthly price is $15)
- "Unlimited storage" with an asterisk linking to a fair-use policy with 10GB effective limit
- "Cancel anytime" but cancellation requires 30 days notice per the ToS
- Feature comparison table that omits a competitor's key advantage

**Why this is a 2:** The user who discovers the omission feels tricked — and they're right. The information was structured to be missed. "Technically disclosed" is not honest if the disclosure is designed to be overlooked.

### Score 3 — Honest but not transparent

No deception or misleading omissions, but the product doesn't proactively share information the user would want. Pricing is accurate but doesn't explain what happens when you cancel. Data collection is standard but not explained. Error messages don't admit fault. The product is honest when asked but doesn't volunteer information.

**Examples at this level:**
- Pricing is accurate, but overage charges aren't mentioned until they occur
- The product collects analytics data, which is described in the privacy policy, but no plain-language summary exists
- Cancellation works fine, but the process and consequences aren't described until you try to cancel
- Errors say "Something went wrong" without admitting whether it's the user's fault or the product's

**Why this is a 3:** Nothing is dishonest, but the product doesn't make it easy for users to make fully informed decisions. The user must go looking for information that should be volunteered. This is the minimum acceptable level — below 3 is a blocker.

### Score 4 — Proactively transparent

The product shares relevant information before the user needs to ask. Pricing includes all costs and consequences. Data practices are explained in plain language. Error messages are honest about cause and fault. The cancellation process and its consequences are clearly described.

**Examples at this level:**
- Pricing page explains overage charges, shows a cost calculator, and describes what happens when you downgrade or cancel
- A plain-language privacy summary is accessible from account settings, not just the legal privacy policy
- Error messages say "our servers are having trouble" (when they are) rather than generic "something went wrong"
- Cancellation page clearly states: access ends on [date], data available for export for 30 days, re-subscribe anytime

**Why this is a 4 (not 5):** Information is shared proactively at the points where it matters most, but there may be gaps in edge cases or less common scenarios. The standard trust moments are handled well.

### Score 5 — Radically honest

The product goes beyond proactive transparency to actively help users make good decisions, even when those decisions might reduce revenue. Suggesting a lower tier when usage fits it. Recommending a competitor when the product isn't the right fit. Publishing pricing methodology. Showing how the product's claims compare to independent measurements.

**Examples at this level:**
- Pricing page includes: "Most teams our size need the Team plan. If you're not sure, start with Free — you can upgrade anytime without losing data."
- Status page includes full incident history with honest postmortems, including root causes that were embarrassing
- Comparison page includes areas where competitors are genuinely better: "If X is your primary need, [Competitor] may be a better fit"
- Downgrade flow suggests the right plan: "Based on your usage, the Starter plan covers everything you've used this month"

**Why this is a 5:** The product treats users as partners in the decision, not targets of a conversion funnel. Radical honesty builds the deepest trust because it signals that the company's interests are aligned with the user's. Users who feel helped rather than sold to become advocates.

---

## Axis 3: Risk reduction

How easy is it for the user to reverse their decision if things go wrong?

### Score 1 — Locked in

The user cannot reverse their decision without significant cost. No free trial (or a trial that requires a credit card and auto-converts). No refund policy. No data export. Cancellation is difficult or impossible through self-service. The user's investment (time, data, money) is trapped.

**Examples at this level:**
- No free trial — payment required before product access
- No refund policy stated, or a policy that requires escalation to claim
- No data export — the user's data is trapped in the product
- Cancellation requires calling support or sending certified mail
- Long-term contracts with no exit clause

**Why this is a 1:** The user must make a high-stakes bet with no recourse. Every commitment is irreversible. Only users who are highly confident (or desperate) will proceed, and those who do will be anxious.

### Score 2 — Technically reversible but costly

Reversal is possible but designed to be difficult. Free trial exists but requires a credit card. Refunds are "available" but require contacting support and justification. Data export exists but is limited or in a proprietary format. Cancellation works but involves a guilt-tripping retention flow.

**Examples at this level:**
- Free trial with credit card — reversible if the user remembers to cancel
- "Contact support for a refund" — possible but friction-laden
- Data export that covers some data but not all (no export of settings, integrations, or history)
- Cancellation flow with 4 pages of retention offers before the actual cancel button

**Why this is a 2:** The user can leave, but the company is making it harder than it needs to be. The friction communicates "we don't want you to leave" rather than "we want you to stay because we're good."

### Score 3 — Reasonably reversible

The user can reverse their decision through standard mechanisms. Free trial without credit card. Refund policy exists and is honored, though it may have conditions. Data export exists for core data. Cancellation is self-service but may involve a brief survey or a single confirmation page.

**Examples at this level:**
- 14-day free trial, no credit card, clear expiration notice
- Refund within 30 days with a simple email request
- CSV export of primary data (but not all metadata, integrations, or settings)
- Cancellation in settings with a confirmation page explaining consequences

**Why this is a 3:** The user can leave without unreasonable friction. The exit isn't hostile. But it could be easier — and the easier it is, the more trust it builds for the next user considering signing up.

### Score 4 — Easy to reverse

The product actively makes it easy to reverse decisions. Generous free trial. Frictionless refund policy. Comprehensive data export in standard formats. Two-click cancellation with clear consequences. The user feels like they can leave at any time.

**Examples at this level:**
- 30-day free trial, no credit card, email before expiration, easy conversion or departure
- "No questions asked" refund within 30 days — email and it's done
- Full data export (JSON, CSV) covering all user data, accessible from settings
- Cancellation: Settings > Cancel > Confirm. Clear statement of access end date and data retention.
- Downgrade option available alongside cancellation — "You can keep a free account with limited features"

**Why this is a 4 (not 5):** Leaving is genuinely easy, but the product doesn't go out of its way to make it painless or to proactively help the user make the right decision about whether to stay or go.

### Score 5 — Proactively reduces risk

The product actively works to ensure the user can leave easily and makes the prospect of leaving a non-event. Data portability is a feature, not an afterthought. The product helps users evaluate whether they should stay. The exit experience is as considered as the entry experience.

**Examples at this level:**
- Free trial with no credit card, 3-day warning email, and a clear comparison of "what you used" vs. "what the paid plan includes"
- Immediate, automated refunds — no human required, no questions
- Full data export including configurations, integrations, and history, in standard formats with documentation
- Cancellation with a confirmation that includes: "Your data will be available for export for 90 days. You can re-subscribe anytime and pick up where you left off."
- Usage-based suggestions: "You've used 3% of your plan this month — would the Starter plan work better?"

**Why this is a 5:** The product treats churn as the user's right, not the company's failure. The exit is as thoughtful as the entry. This level of risk reduction paradoxically reduces churn — users who feel free to leave are more comfortable staying.

---

## Aggregating scores

### By trust moment

Score each of the five trust moments (pre-signup, signup, post-signup, payment, ongoing) on all three axes. This produces a 5x3 matrix.

### Trust blockers

Any honesty score below 3 = fix immediately. A single dishonest moment undermines all trust signals. The blocker must be resolved before investing in signal strength or risk reduction improvements.

### Priority order

1. **Fix honesty blockers** — any score below 3 on honesty
2. **Improve risk reduction** — highest leverage for conversion, especially at signup and payment
3. **Strengthen signals** — add and improve trust signals at each moment

### Overall trust health

- **Healthy:** No axis below 3 on any moment. Average across all moments is 3.5+.
- **At risk:** One or two moments have an axis at 2. No blockers but trust is leaking.
- **Blocked:** Any moment has honesty at 1 or 2. Trust is actively being destroyed.

### When scores disagree

If two reviewers score differently, the disagreement almost always means they're evaluating from different user perspectives. Resolve by pinning the user profile: "Score this as a first-time visitor from a startup," "Score this as an enterprise buyer doing due diligence." Different users have different trust needs, and the same signals read differently to different audiences.

### When 3 is acceptable

Not every trust moment needs a 5. Administrative settings, developer documentation, and low-traffic pages can live at 3 without damaging overall trust. The high-traffic moments — landing page, signup, pricing, cancellation — are where 4-5 scores matter most. Invest scoring effort where users actually make trust decisions.
