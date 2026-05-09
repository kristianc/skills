# TRUST-SIGNALS.md

The deep reference for trust signals at each moment of the user journey. For each signal: what good looks like, what bad looks like, when to use it, when it backfires, and the underlying principle.

---

## Pre-signup (visitor to consideration)

The user is asking: "Is this real? Is it credible? Will it do what it claims?" They have no relationship with the product yet. Everything is inference from what they can see.

### Logo bars

**What good looks like:**
- Real, recognizable companies that are actual customers
- Grayscale logos, consistent sizing, aligned in a single row
- Links to corresponding case studies or quotes
- Label says "Used by" or "Trusted by" — not "As featured in" unless that's actually true
- 5-8 logos maximum. Quality over quantity.

**What bad looks like:**
- Logos of companies that are not customers (integration partners displayed as if they were customers, or companies that used the product once during a free trial)
- Full-color logos competing with the product's brand
- Inconsistent logo sizes creating visual noise
- "As seen on" with press logos when the "coverage" was a paid placement or a listicle mention
- 20+ logos creating a NASCAR effect

**When it backfires:**
- When the user recognizes a logo and knows the company doesn't use the product
- When logos are from a different market than the user's (enterprise logos on a product targeting startups)
- When the logos are all unknown — they add visual clutter without adding credibility

**Underlying principle:** Social proof works when the user can identify with the proof. The question is not "are these impressive companies?" but "are these companies like mine?"

### Testimonials

**What good looks like:**
- Full name, role, company name, headshot — all verifiable
- Specific claim: "Reduced our deploy time from 45 minutes to 3 minutes" beats "Great product, saves us time"
- 3-5 testimonials. Enough to establish a pattern, few enough that each feels curated.
- Placed near the action they support — a testimonial about easy setup near the signup form, a testimonial about ROI near the pricing page
- Represent different customer types so different visitors can see themselves

**What bad looks like:**
- First name and last initial only ("Sarah T.") — unverifiable
- Stock photos instead of real headshots
- Vague praise: "Love it!", "Game changer!", "Best tool ever!"
- All testimonials from the same type of customer
- Testimonials on a dedicated /testimonials page no one visits, instead of placed contextually

**When it backfires:**
- When the testimonial is obviously solicited and curated to be positive (all 5 stars, no nuance)
- When the claims are unverifiable or clearly exaggerated
- When the headshot is recognizably a stock photo

**Underlying principle:** Testimonials are borrowed credibility. The borrowing only works when the lender is credible. A specific claim from a real, identifiable person transfers credibility. A vague claim from an anonymous source transfers nothing.

### Usage numbers

**What good looks like:**
- Specific numbers: "12,847 teams" not "thousands of customers"
- Meaningful metrics: active users or paying customers, not "accounts created" or "downloads"
- Contextual: "Processing 2.3M events/day" tells the user the system is proven at scale
- Updated: stale numbers ("since 2019") suggest the product stopped growing

**What bad looks like:**
- Round numbers that are obviously estimated: "10,000+ customers" (probably 6,000)
- Vanity metrics: "1M downloads" for a SaaS product (downloads are not users)
- Unqualified: "50,000 users" — are these active? paying? accounts that signed up and never returned?

**When it backfires:**
- When the number is small and drawing attention to it highlights the product's immaturity. 12 customers is not social proof — it's a reason to worry about viability.
- When the number is suspiciously large for the product's age and market

**Underlying principle:** Specificity signals honesty. "12,847" says "we counted." "10,000+" says "we rounded up."

### Press and media

**What good looks like:**
- Links to actual articles in recognizable publications
- The coverage is substantive — a review, a feature story, an interview
- Displayed modestly: publication logo, one-line quote, link to the full article

**What bad looks like:**
- "As seen on" with logos of publications that never covered the product
- Paid placements or sponsored content displayed as editorial coverage
- Listicle mentions ("Top 50 Tools for 2024") displayed as featured coverage

**When it backfires:**
- When a user clicks through and finds a sponsored post or a passing mention in a listicle. The gap between the implied coverage and the actual coverage destroys trust.

**Underlying principle:** Press coverage is third-party validation. It only works when the third party is genuinely independent and the coverage is genuinely substantive.

### Team and about page

**What good looks like:**
- Real people with real names, roles, and professional backgrounds
- Domain expertise signals: "Previously led infrastructure at [known company]" or "PhD in cryptography"
- Founding story that explains why this team is credible for this problem
- Photos that look real (not studio-perfect headshots that feel corporate)

**What bad looks like:**
- No about page at all — the user cannot learn who is behind the product
- Generic bios with no domain connection
- Stock photos or AI-generated headshots
- Founding story that's purely emotional without explaining domain credibility

**When it backfires:**
- When the team is very small and displaying it highlights lack of resources for a product that implies enterprise capability. In this case, focus on domain expertise rather than team size.

**Underlying principle:** People trust people. An anonymous product asks for more faith than a product with visible, qualified humans behind it.

---

## Signup (consideration to commitment)

The user is asking: "What am I giving up? What do I get? Can I leave if this doesn't work?" The cost of commitment matters now.

### Form field economy

**What good looks like:**
- Only the fields necessary for the next step. Email and password for signup. Name can wait until onboarding.
- Each field has a visible reason: if you ask for company size, explain why ("so we can set up your workspace")
- Progressive disclosure: collect more information as the user gets more value, not all upfront

**What bad looks like:**
- Requiring phone number, company name, role, team size, and "how did you hear about us" before the user sees the product
- Required fields with no explanation of why they're needed
- Marketing-motivated fields (industry, company size) disguised as product-necessary fields

**Why it matters:** Every form field is a trust cost. The user is spending personal information. They need to believe the exchange is fair.

### Free trial without credit card

**What good looks like:**
- "Start free — no credit card required" stated explicitly
- The trial delivers real value without artificial limitations designed to frustrate
- Clear communication about when the trial ends and what happens

**What bad looks like:**
- Requiring a credit card for a "free" trial — this is risk transfer, not risk reduction
- A trial so limited it doesn't demonstrate the product's value
- Auto-converting to paid with an email buried in fine print as the only notice

**Why it matters:** Requiring a credit card for a free trial communicates "we're counting on you forgetting to cancel." Not requiring one communicates "we're counting on the product being good enough that you'll choose to pay."

### Value clarity at signup

**What good looks like:**
- Explicit statement of what happens next: "You'll set up your first project in about 2 minutes"
- Expectations match reality — if it says 2 minutes, it takes 2 minutes
- Preview of the product (screenshots, demo, video) so the user knows what they're getting

**What bad looks like:**
- "Sign up to get started" with no indication of what "getting started" involves
- Overpromising: "Set up in 30 seconds" when the real setup takes 20 minutes
- The first experience after signup is a blank dashboard with no guidance

**Why it matters:** Uncertainty is the enemy of trust. The user is about to invest time. Telling them exactly what that investment looks like reduces the perceived risk.

### Social proof at the decision point

**What good looks like:**
- A relevant testimonial adjacent to the signup form
- A usage number ("Join 12,847 teams") near the CTA
- Proof that matches the action: setup-focused testimonial near signup, ROI-focused near pricing

**What bad looks like:**
- All social proof on the homepage hero, none near the signup form
- Social proof that's disconnected from the decision being made

**Why it matters:** Social proof is most effective at the moment of decision. A testimonial on the homepage establishes awareness; a testimonial next to the signup button reduces signup friction.

---

## Post-signup (first use to activation)

The user is asking: "Did it deliver? Is my data safe? Can I get help?" The product must earn the trust it was just granted.

### Promise delivery

**What good looks like:**
- If the landing page said "set up in 5 minutes," the product guides the user through setup in 5 minutes
- First value delivery is fast — the user should experience the product's core value in the first session
- Onboarding matches the signup's implied promise

**What bad looks like:**
- The first experience is a blank screen with no guidance
- Setup takes significantly longer than promised
- The user has to figure out how to get value — the product assumes knowledge it hasn't provided

**Why it matters:** The gap between marketing promises and product reality is the highest-velocity trust destroyer. Every unmet promise converts goodwill into suspicion.

### Data transparency

**What good looks like:**
- Clear explanation of what data was collected and how it's used, accessible from account settings
- Privacy policy that is readable by humans, not just lawyers
- Data handling practices are visible in the product, not just in legal documents
- GDPR/CCPA compliance surfaced proactively, not just reactively

**What bad looks like:**
- No visible privacy or data handling information after signup
- A privacy policy that contradicts the product's marketing claims about data
- Data collection that goes beyond what the user expected based on the signup flow

**Why it matters:** The user just gave you their data. They want to know you're treating it responsibly. Transparency here is cheap to provide and expensive to omit.

### Support accessibility

**What good looks like:**
- Contact information visible and easy to find — not hidden behind 5 clicks
- Clear expectations: "We respond within 4 hours" and they actually do
- Multiple channels appropriate to the audience (chat for consumer, email for B2B, documentation for developers)
- A real human is reachable, not just a chatbot loop

**What bad looks like:**
- No visible support contact anywhere in the product
- A chatbot that loops without ever connecting to a human
- "Contact support" leads to a form that goes to a black hole
- Response time promises that aren't kept

**Why it matters:** The user needs to know they can get help if something goes wrong. Hidden support signals "we don't want to hear from you."

---

## Payment (activation to monetization)

The user is asking: "What does this actually cost? What am I locked into? What if I change my mind?" This is the highest-stakes trust moment.

### Pricing clarity

**What good looks like:**
- All costs visible on the pricing page — monthly, annual, per-seat, overages
- If displaying annual price, the monthly equivalent is also shown and the commitment is clear
- Feature comparison between tiers is honest — not structured to manipulate tier choice
- Total cost at checkout matches expectations from the pricing page, with no surprise fees

**What bad looks like:**
- Displaying annual price per month without clearly labeling it as annual commitment
- Adding fees at checkout that weren't on the pricing page (setup fees, platform fees, "processing fees")
- Feature comparison tables that hide competitor advantages or bury important limitations
- "Contact sales" as the only pricing for the tier most users need

**Why it matters:** Price surprise at checkout is the most-studied trust-destroying pattern in e-commerce research. The user feels tricked, even if the information was technically available. "Technically available" is not the same as transparent.

### Honest comparison tables

**What good looks like:**
- Features listed that genuinely differ between tiers, not padded with features that are the same across all tiers
- Limitations stated clearly: "Up to 5 users" not just "Team access" with the limit in fine print
- If comparing to competitors, the comparison is verifiable and fair
- The recommended tier is the one that genuinely fits most users, not the most expensive

**What bad looks like:**
- Graying out features on lower tiers to make them look inferior, when the feature differences don't matter for the tier's target user
- "Unlimited" with an asterisk leading to a fair-use policy that imposes limits
- Competitor comparison that cherry-picks categories where you win and omits where they win

**Why it matters:** Users read comparison tables carefully because they're making a spending decision. Manipulation is detected more often than marketers think, and the cost is permanent: the user now suspects every claim on the page.

### Cancellation clarity

**What good looks like:**
- "Cancel anytime" is literally true — the user can cancel from their account settings in 2 clicks
- What happens after cancellation is clearly stated: when access ends, what happens to data, whether there's a grace period
- No guilt-tripping, no emotional manipulation, no dark-pattern retention flows
- Data export is available before and after cancellation

**What bad looks like:**
- Cancellation requires contacting support, calling a phone number, or navigating a multi-page guilt trip
- "Cancel anytime" in marketing but the actual cancellation is hidden or difficult
- Retention flows that use confirmshaming ("Are you sure? You'll lose all your data")
- Unclear what happens to data post-cancellation

**Why it matters:** The harder it is to leave, the less trust a prospective user has in signing up. Cancellation friction doesn't retain users — it prevents new ones. Every user who sees a complaint about cancellation difficulty on social media recalculates their risk.

### Money-back guarantee

**What good looks like:**
- If offered, the guarantee is unconditional and frictionless — the user emails, they get a refund
- The guarantee period is clearly stated
- No hoops: no "you must have used the product for at least 14 days," no "tell us why" requirements for the refund itself

**What bad looks like:**
- A guarantee with conditions that make it hard to claim
- A guarantee that requires escalation or negotiation
- A guarantee that's prominently displayed but rarely honored in practice

**Why it matters:** A money-back guarantee is the purest form of risk reduction. It says "we'll eat the cost if you're not satisfied." But only if it's real. A guarantee with friction is worse than no guarantee — it's a broken promise.

### Data export

**What good looks like:**
- User can export all their data in a standard format (CSV, JSON, API)
- Export is accessible from account settings, not hidden
- Export includes all data, not just a subset
- "Your data is yours" is a verifiable claim, not just copy

**What bad looks like:**
- No export capability at all
- Export is limited to a subset of data
- Export requires contacting support
- The export format is proprietary or unusable without the product

**Why it matters:** Data portability is the ultimate risk reduction for SaaS. It answers "what if I need to leave?" with "you take everything with you." The easier the answer, the less the user worries about commitment.

---

## Ongoing (retention to advocacy)

The user is asking: "Is this product alive? Does it respect me? Will it be here next year?" Trust must be maintained, not just established.

### Status page

**What good looks like:**
- Public status page (status.example.com) accessible without authentication
- Honest, real-time updates during incidents
- History of past incidents with resolution times
- Proactive communication: the status page reflects the problem before users report it

**What bad looks like:**
- No status page
- A status page that always shows green, even during incidents
- Incidents acknowledged only after users complain on social media
- Vague status updates: "We are investigating an issue" with no specifics or timeline

**Why it matters:** A status page that lies is worse than no status page. Users check the status page when they're experiencing a problem. If it says "all systems operational" while the product is down, trust in every future status update is destroyed.

### Incident communication

**What good looks like:**
- Admitting the problem immediately, not waiting until it's fixed
- Specifics: what's affected, what's not, estimated time to resolution
- Honest updates even when the news is bad: "We don't have a fix yet, still investigating"
- Postmortem after resolution: what happened, why, what you're doing to prevent it
- Communication in the channels users are already in (email, in-app, status page) — not just Twitter

**What bad looks like:**
- Silence during incidents
- Minimizing: "Some users may experience intermittent issues" when the product is completely down
- No postmortem — the incident is never explained
- Blaming third parties without taking responsibility for the user's experience

**Why it matters:** Incidents are inevitable. How you handle them determines whether trust increases ("they were honest and fixed it fast") or decreases ("they lied about it and pretended it didn't happen").

### Changelog

**What good looks like:**
- Regular updates showing the product is improving
- Specific: what changed, why, how it helps users
- Accessible from within the product (not just a blog post)
- Includes bug fixes and small improvements, not just big features — this shows ongoing care

**What bad looks like:**
- No changelog — users can't tell if the product is being maintained
- Changelog that only shows marketing-worthy features, hiding bug fixes and maintenance
- Months-long gaps between entries

**Why it matters:** A changelog is evidence that the product is alive. Silence breeds worry: "Is this product maintained? If something breaks, will anyone fix it?"

### Roadmap visibility

**What good looks like:**
- Appropriate transparency about direction without overpromising specific features or dates
- Public roadmap at a high level: "We're working on integrations this quarter"
- Accepting feedback and showing it influences direction
- Honest about what's not on the roadmap: "We don't plan to build X because Y"

**What bad looks like:**
- Complete opacity: "We don't share our roadmap" with no alternative communication
- Overpromising: specific features with specific dates that repeatedly slip
- A roadmap that's clearly marketing — only the features that sound exciting, with no evidence of execution

**Why it matters:** Roadmap visibility answers "will this product grow with me?" Opacity forces the user to bet on your future without evidence. Appropriate transparency gives them a basis for confidence — or an honest signal to look elsewhere.

---

## Cross-cutting signals

### Security and privacy signals

These apply across multiple trust moments:

- **Compliance badges** (SOC 2, GDPR, HIPAA): display only if genuinely certified. Link to verification. Place near data-sensitive actions (signup, payment), not just the footer.
- **Data handling explanations**: plain-language summary of what you collect, store, and share. Not the privacy policy — a human-readable version of it.
- **Cookie consent**: respect the user's choice. No dark-pattern banners that make "Accept all" prominent and "Manage preferences" tiny. Pre-select nothing.
- **HTTPS, security headers, CSP**: these are table stakes. Their absence is a trust signal (negative). Their presence is expected, not differentiating.
- **Privacy-first defaults**: opt-out of data sharing by default, not opt-in. This communicates "we respect your data" more than any badge.

### Honest copy across all moments

- No claims that can't be verified
- No manufactured urgency or fake scarcity
- CTAs that describe the action honestly ("Start free trial" not "Get started" when getting started requires a credit card)
- No guilt-tripping ("No thanks, I don't want to grow my business")
- Fine print consistent with headline claims
- Terms of service that don't contradict marketing language
