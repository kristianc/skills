# POSITIONING-DOCUMENT.md

The deliverable format. Each section of the positioning document, with guidance on what good looks like, common mistakes, and how to test the document once written.

## The sections

### Category

**What it is:** One sentence naming what the product is — the mental shelf the customer puts it on.

**What good looks like:** A category the customer already recognizes. "Open-source analytics platform." "Collaborative design tool." "API-first payment processing." The customer reads the category and immediately knows which other products to compare against.

**Common mistakes:**
- **Inventing a category.** "Revenue intelligence orchestration platform" tells the customer nothing. They'll assign you to a category anyway — and it'll be the wrong one because you didn't pick it.
- **Category too broad.** "Software for businesses" doesn't help anyone compare. The category should narrow the competitive set.
- **Category too narrow.** "Real-time columnar analytics for Kafka event streams" might only have one entrant. If the customer doesn't self-identify as someone who buys this, the category is too narrow.

**Test:** Could a target customer hear the category and, without further explanation, name 2-3 other products in it? If yes, the category is real. If they look confused, it's invented or too narrow.

### Target customer

**What it is:** Who the product is for — specifically. Role, company profile, situation. And who it is NOT for.

**What good looks like:** "Engineering teams at Series A-C SaaS companies (20-200 employees) who need to understand product usage without dedicating an analyst. Not for data teams at enterprises who need a full BI stack — they need Looker or Tableau."

The "not for" is as load-bearing as the "for." It's the proof that a real positioning choice has been made. If the product is for everyone, it's for no one, and the positioning document is aspirational, not strategic.

**Common mistakes:**
- **Too broad.** "Companies that need analytics." Which companies? What size? What role is the buyer?
- **Describing the product instead of the customer.** "Companies that need real-time event processing" describes a product need, not a customer. "Growth teams running A/B tests who can't wait 24 hours for batch results" describes a customer.
- **Omitting the "not for."** Every positioning choice excludes someone. If the document doesn't name who's excluded, the team will pursue every lead, and the product will drift toward serving no one well.

**Test:** Could a salesperson read this and, within 30 seconds of a discovery call, know whether a prospect is in or out? If it takes five minutes of qualification to figure out, the target customer description isn't specific enough.

### Key differentiator

**What it is:** The wedge, stated as a claim. One or two sentences that articulate what the product does better than alternatives, for the target customer, on the axis that matters most.

**What good looks like:** "Fastest time to first insight — a product engineer can answer a usage question in under 60 seconds, with no SQL, no analyst, and no data pipeline to configure. Competitors require a data team to set up and maintain."

The differentiator is specific (60 seconds, no SQL), verifiable (a prospect could test this in a trial), and positioned against a structural constraint (competitors require data teams because their architecture assumes one).

**Common mistakes:**
- **Vague claims.** "Best-in-class analytics" — on what axis? Measured how? Compared to whom?
- **Aspirational claims.** "Will be the fastest" — positioning is about what's true now, not what's on the roadmap.
- **Claims that require trust.** "Our customers love us" — how would a prospect verify this before buying? Claims should be testable.
- **Multiple claims masquerading as one.** "Fastest, easiest, and most affordable" — if you're winning on every axis, you're either lying or you haven't made a positioning choice. Pick the axis that matters most and commit.

**Test:** The verifiability test from GLOSSARY.md. Could a prospect test this claim in a free trial, a POC, or a reference call? If the only way to evaluate it is to take your word for it, it's not a claim — it's an assertion.

### Proof points

**What they are:** 3-5 specific, testable facts that support the key differentiator claim. Each proof point should independently provide evidence for the claim.

**What good looks like:**
- "Average time from signup to first query: 4.2 minutes (measured across 200 onboardings)."
- "No-code query builder handles 90% of common product questions without SQL."
- "Zero-config SDKs for React, React Native, Python, and Node — paste one line and events start flowing."
- "Autocapture means no manual instrumentation — the product tracks clicks, pageviews, and form submissions out of the box."

Each proof point is specific (numbers, capabilities, measurements), testable (a prospect could verify it), and connected to the claim (they all support "fastest time to first insight").

**Common mistakes:**
- **Features without connection to the claim.** "We have 200 integrations" — this supports an integration-breadth claim, not a time-to-insight claim. Every proof point must connect back.
- **Metrics without context.** "99.9% uptime" — compared to what? Is that good for this category? Is the competitor at 99.99%?
- **Social proof as proof point.** "Trusted by 500 companies" — this is a trust signal, not a proof point for any specific claim. Use it in marketing, not in the positioning document.
- **Too many proof points.** More than 5 dilutes the argument. If you need 8 proof points to support the claim, the claim is probably too broad — narrow it.

**Test:** For each proof point, ask: "If this were the ONLY evidence I had for the claim, would it move the needle?" If not, it's filler — cut it or replace it.

### Competitive frame

**What it is:** A narrative of how the product relates to each major alternative. Not a feature matrix — a story about different choices for different customers.

**What good looks like:**

"Unlike Amplitude, which is built for data teams running complex behavioral analytics, [Product] is built for the product engineer who wants an answer in 60 seconds. Amplitude is the right choice for teams with dedicated analysts who need cohort funnels, predictive models, and data governance. We're the right choice for teams where the engineer who ships the feature also needs to measure it.

Unlike Mixpanel, which started simple but has grown toward enterprise complexity, [Product] maintains a deliberately narrow scope — product usage questions, answered fast. Mixpanel is better for teams that need marketing analytics alongside product analytics. We're better for teams that only need product analytics and don't want to pay for (or navigate around) marketing features.

Unlike building on Metabase + your data warehouse, [Product] requires no pipeline. The warehouse approach is right for teams that already have a data stack and an analyst to maintain it. We're right for teams that don't."

**Common mistakes:**
- **Feature matrix thinking.** "We have X, they don't have Y" — this is a comparison, not a frame. A competitive frame explains WHY the differences exist (different customers, different architectural choices, different strategic bets).
- **Dismissing competitors.** "Competitor X is outdated / expensive / hard to use" — this is trash talk, not positioning. Frame competitors as making legitimate choices for different customers.
- **Ignoring the status quo.** "Do nothing" is usually the biggest competitor. Frame the status quo explicitly: what the customer does today, what it costs them, and why they should change.
- **Positioning against too many competitors.** Frame the 2-4 alternatives the target customer actually considers. If you're framing against 8 competitors, the target customer is too broad.

**Test:** Could you show the competitive frame to a prospect who uses a competitor and have them say "that's fair — they're right that [competitor] is better for [other use case]"? If the frame is honest, competitors' customers will recognize it as accurate. If they'd object, it's not a frame — it's spin.

### What we give up

**What it is:** An explicit statement of what the product sacrifices for its positioning. The honesty check.

**What good looks like:** "By optimizing for speed-to-insight and engineer self-serve, we give up: deep behavioral analytics (no predictive cohorts, no statistical significance testing on funnels), data governance controls (no role-based access to specific metrics, no audit log), and multi-product analytics (we track one product at a time — teams running multiple products need a data warehouse approach)."

The "give up" section makes the positioning credible. It demonstrates that a real choice has been made, not a fantasy where the product wins everywhere.

**Common mistakes:**
- **Nothing is given up.** If the document doesn't name trade-offs, the positioning is either dishonest or the team hasn't made a real choice. Every positioning choice has a cost — name it.
- **Trivial sacrifices.** "We give up a complex enterprise sales motion" — this isn't a sacrifice if the target customer doesn't want that anyway. The sacrifice must be something the target customer might occasionally wish they had.
- **Hiding the sacrifice in euphemism.** "We focus on simplicity" — this means "we're less powerful." Say it directly.

**Test:** Does the "give up" section name something a churned customer might cite as their reason for leaving? If not, it's not naming real sacrifices.

## Testing the document

Once the positioning document is written, run these tests:

### The bar test

Read the positioning document aloud in 30 seconds. If you can't, it's too long or too vague. Positioning should be immediately communicable — if the team can't internalize it, it won't influence decisions.

### The competitor test

Show the competitive frame to someone who uses a competitor. Would they say "that's a fair description of what [competitor] is good at and what it isn't"? If the frame is dishonest or dismissive, it won't survive contact with a prospect who knows the competitor's product.

### The exclusion test

Does the document exclude someone? If the target customer section doesn't turn anyone away, and the "what we give up" section doesn't name real sacrifices, the positioning hasn't made a choice.

### The sales test

Could a salesperson use this document — without modification — to run a discovery call? Could they determine in the first 2 minutes whether a prospect is a fit? Could they articulate the differentiator without jargon? If not, the document is strategic theory, not operational positioning.

### The roadmap test

Does the positioning document imply product priorities? If the team reads it and can't identify which features to build next and which to decline, the positioning is too vague to be useful.

## How the positioning document relates to other GTM artifacts

The positioning document is not written in isolation. It connects to:

- **ICP (Ideal Customer Profile)** — the target customer section of the positioning document should align with the ICP. If they disagree, one is wrong.
- **Launch brief** — a launch brief for a feature should reference the positioning document to ensure the feature is framed in terms of the wedge. A feature that doesn't connect to the positioning is either a maintenance item or a sign that the positioning needs updating.
- **Pricing** — positioning and pricing are coupled. A product positioned on premium quality with bargain pricing sends a contradictory signal. A product positioned on accessibility with enterprise pricing excludes the target customer. The pricing-research skill (see `skills/gtm/pricing-research/`) addresses pricing independently, but the two analyses should be reconciled.
- **Sales collateral** — competitive frames from the positioning document flow directly into battlecards, objection-handling docs, and comparison pages. The positioning document is the source of truth; collateral is the derivative.
- **Brand voice** — the positioning document names what the product is and isn't. Brand voice expresses that positioning in tone, vocabulary, and style. Voice without positioning is aesthetics; positioning without voice is strategy that never reaches the customer.
