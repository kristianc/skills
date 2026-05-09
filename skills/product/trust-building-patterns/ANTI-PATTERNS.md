# ANTI-PATTERNS.md

Trust-destroying patterns to find and remove. Organized by type. For each anti-pattern: how to spot it in code, why it destroys trust, what to replace it with, and what code patterns to look for.

The asymmetry of trust: building trust requires dozens of consistent honest signals over time. Destroying it requires one dishonest moment. These anti-patterns are the dishonest moments.

---

## Dark patterns

Designs that trick users into actions they didn't intend. These are trust blockers — fix before anything else.

### Forced continuity

**What it is:** A free trial that automatically converts to a paid subscription without clear notice, relying on the user forgetting to cancel.

**How to spot it in code:**
- Subscription creation at trial signup that schedules automatic billing
- No pre-charge notification email in the email/notification system
- Trial-end logic that converts without an explicit user action
- Credit card required at signup with no clear statement about auto-conversion

**Why it destroys trust:** The user feels tricked. Even if they wanted to continue, discovering an unexpected charge converts a potential advocate into a detractor. The company's revenue depends on forgetfulness, not value.

**Replace with:**
- Clear statement at signup: "Your trial ends [date]. We'll email you 3 days before. No charge without your confirmation."
- Pre-charge notification email with a one-click option to cancel or continue
- Trial expiration that requires explicit conversion: "Your trial ended. [Start subscription] or [Export your data]"

### Sneak into basket

**What it is:** Adding items, services, or charges to the user's purchase without explicit consent. Pre-selected add-ons, bundled services that appear during checkout.

**How to spot it in code:**
- Checkout flow that adds line items the user didn't explicitly select
- Pre-checked checkboxes for add-on services or extended warranties
- Default-selected options that increase the total (annual plan pre-selected when user clicked monthly)

**Why it destroys trust:** The user expected to pay X and is being asked to pay X+Y. Even if they notice and remove it, they now suspect every subsequent interaction.

**Replace with:**
- Nothing pre-selected. All add-ons are opt-in.
- The price the user saw on the pricing page matches the checkout total exactly. No additions.

### Confirmshaming

**What it is:** Framing the rejection of an offer in a way that makes the user feel guilty or stupid for declining.

**How to spot it in code:**
- Modal dismiss buttons with self-deprecating text: "No thanks, I don't want to save money," "I prefer to stay unprotected," "No, I'll pay full price"
- Newsletter popups where the decline option is phrased as a negative self-statement
- Upgrade prompts where the dismiss is emotionally charged

**Why it destroys trust:** It's manipulative. The user recognizes they're being emotionally pressured, which triggers suspicion of every other interaction. Even users who comply feel resentful.

**Replace with:**
- Neutral decline language: "No thanks," "Not now," "Skip," "Dismiss"
- Respect the user's decision without commentary on what it means about them

### Roach motel (easy in, hard out)

**What it is:** Making signup effortless but cancellation difficult. Account deletion requires calling a phone number. Cancellation is buried in settings behind multiple confirmation pages.

**How to spot it in code:**
- No cancellation route in user-facing settings/account pages
- Cancellation flow with more than 2 steps (confirm intent, confirm consequences)
- Cancellation that requires contacting support (look for "contact us to cancel" strings)
- Account deletion that differs significantly in friction from account creation
- Retention flows with multiple pages of offers, surveys, and emotional appeals before the actual cancel action

**Why it destroys trust:** Every prospective user who researches "how to cancel [product]" and finds complaints recalculates their signup risk. Cancellation friction is visible to the market, not just to churning users. It's a tax on acquisition.

**Replace with:**
- Cancellation accessible from account settings, 2 clicks maximum: "Cancel subscription" then "Confirm cancellation"
- Clear statement of what happens: when access ends, what happens to data
- Optional (truly optional) one-question feedback: "Mind sharing why?" with "Skip" as prominent as "Submit"
- No guilt, no emotional appeals, no discount offers in the cancellation flow itself. (Offer retention deals in normal product communication, not at the exit door.)

### Hidden costs

**What it is:** Prices displayed on the marketing/pricing page that don't include fees revealed at checkout or in the first invoice.

**How to spot it in code:**
- Checkout flow that adds line items not present on the pricing page (setup fees, platform fees, tax displayed differently than expected)
- Per-seat pricing where "seat" is defined differently than the user expects (active users vs. provisioned users)
- Overage charges described only in terms of service, not on the pricing page
- Different price display formats between pricing page and checkout (monthly rate shown as annual on pricing, monthly at checkout)

**Why it destroys trust:** Price surprise at checkout is one of the most-studied trust violations in commerce. The user feels deceived, even if every fee is technically disclosed somewhere. "Technically disclosed" is not transparent.

**Replace with:**
- The price on the pricing page is the price at checkout. Period.
- If there are variable costs (overages, per-seat), show the formula on the pricing page with a calculator or examples
- If tax is additional, say so on the pricing page, not at checkout

### Bait and switch

**What it is:** Advertising one thing and delivering another. A feature prominently displayed in marketing that requires an upgrade in practice. A "free" tool that gates core functionality behind payment.

**How to spot it in code:**
- Feature flags or tier checks on functionality that's presented without qualification in marketing
- Landing page copy that describes capabilities only available on higher tiers without noting the tier
- Free tier that technically exists but is so limited that no real use case fits within it

**Why it destroys trust:** The user invested time based on a promise. Discovering the promise was conditional after the investment feels like a scam. Even if the paid version is worth it, the relationship started with deception.

**Replace with:**
- If a feature requires a specific tier, say so where the feature is described
- Free tiers should be genuinely useful for their target use case, not just a demo
- Marketing copy should describe what the user gets at the tier they're likely evaluating

### Misdirection

**What it is:** Using visual hierarchy, layout, or copy to steer users toward the option the company prefers rather than the option the user would choose if clearly informed.

**How to spot it in code:**
- CTA buttons where the company-preferred option is large/colored and the user-preferred option is small/gray/text-only
- Cookie consent banners where "Accept all" is a prominent button and "Manage preferences" is a text link
- Unsubscribe flows where "Keep receiving emails" is the primary button and "Unsubscribe" is secondary
- Plan selection where the most expensive option has the strongest visual emphasis regardless of fit

**Why it destroys trust:** Users notice when they're being steered. Modern users are especially attuned to cookie consent manipulation and upsell misdirection. Being caught manipulating is worse than not converting — it's a credibility event.

**Replace with:**
- Equal visual weight for options that are genuinely equal choices
- The option that's best for the user should be most prominent, not the option that's best for the business
- Cookie consent: "Accept" and "Reject" get equal visual treatment. "Manage preferences" is visible, not hidden.

---

## Fake trust signals

Elements that look like trust signals but aren't grounded in reality. These are often more damaging than having no trust signals, because discovery converts the user from uninformed to actively suspicious.

### Stock photo testimonials

**How to spot it:** Reverse image search on testimonial photos. Generic names without verifiable companies. The same stock photo used across multiple products.

**Why it destroys trust:** The user who discovers a stock photo testimonial concludes every testimonial on the site is fake. One fake testimonial poisons all real ones.

**Replace with:** Real testimonials from real customers, or no testimonials. If you don't have real ones yet, earn them. A section that says "Trusted by 47 teams" with no testimonials is more honest than three fake ones.

### Unverifiable numbers

**How to spot it:** Round numbers that sound impressive but can't be checked. "Trusted by 10,000+ companies" with no way to verify. Numbers that conflate different metrics (signups counted as "customers," downloads counted as "users").

**Why it destroys trust:** Sophisticated buyers (especially B2B) test claims. If "10,000 companies" can't be reconciled with the product's age, market size, or any independent data, the number reads as fabricated.

**Replace with:** Specific, honest numbers. If you have 247 paying customers, say 247. Specificity signals honesty. If the number is small, consider whether displaying it helps or hurts — sometimes "Join our early customers" is better than "Join our 12 customers."

### Logos of non-customers

**How to spot it:** Logo bars that include integration partners, companies that signed up for a free trial but never became customers, or companies whose employees use the product personally but not on behalf of the company.

**Why it destroys trust:** If someone from one of those companies sees their logo, they'll call it out. One public "we don't use this product" demolishes the entire logo bar's credibility.

**Replace with:** Only display logos of companies that have given explicit permission or are verifiable current customers. Integration partner logos belong in an "Integrates with" section, clearly labeled.

### Fake urgency and scarcity

**How to spot it in code:**
- Countdown timers that reset on page reload or when the cookie is cleared
- "Only X left" where X doesn't correspond to real inventory or capacity
- "This offer expires" where the offer is permanent
- Session-based "special pricing" that every visitor sees

**Why it destroys trust:** Users test urgency claims. They open an incognito window. They return the next day. When the "expiring" offer is still there, every future communication from the product is read with suspicion.

**Replace with:** If there's real urgency (event date, genuine limited capacity, seasonal pricing), communicate it with specifics and let the user verify. If there's no real urgency, don't manufacture it.

---

## Dishonest copy

Words that sound impressive but mislead, hide, or manipulate.

### Vague superlatives

**How to spot it:** Claims that sound good but say nothing verifiable. "Best-in-class," "world-class," "industry-leading," "cutting-edge," "revolutionary." No evidence, no comparison, no specifics.

**Why it destroys trust:** Sophisticated users read superlatives as a signal that the product can't make specific claims. "Best-in-class" reads as "we can't tell you exactly how we're better."

**Replace with:** Specific, verifiable claims. "Deploys in under 3 minutes" instead of "lightning-fast deployment." "99.95% uptime over the last 12 months" instead of "industry-leading reliability."

### Buried limitations

**How to spot it in code:**
- Marketing copy that says "unlimited" with an asterisk or fine print link to a fair-use policy
- Feature descriptions that omit tier restrictions
- "Free forever" with conditions buried in terms of service
- Rate limits, storage caps, or user limits described only in documentation, not on the pricing page

**Why it destroys trust:** The user who hits a limit they didn't know about feels lied to. The feeling is proportional to how hard the limitation was to find before committing.

**Replace with:** State limitations where they matter. "Unlimited" means unlimited. If there's a fair-use policy, call it out: "Generous limits for normal use — see our fair-use policy." If a feature has tier restrictions, show them on the pricing page.

### Marketing/ToS mismatch

**How to spot it:** Compare the claims on the marketing site with the terms of service. Look for contradictions: marketing says "your data is yours," ToS grants the company a broad license to use customer data. Marketing says "cancel anytime," ToS has a 30-day notice requirement.

**Why it destroys trust:** Users who read the ToS (especially enterprise buyers and their legal teams) will catch contradictions. Each contradiction is interpreted as deliberate deception, not as an oversight.

**Replace with:** Align terms of service with marketing claims. If the ToS needs a specific provision, reflect that honestly in marketing. "Your data is yours — see our data terms for the specifics" is honest. "Your data is yours" followed by a ToS that grants a broad usage license is not.

---

## Privacy hostility

Treating user privacy as an obstacle rather than a right.

### Pre-checked consent

**How to spot it in code:**
- Checkboxes for marketing emails, data sharing, or analytics that are checked by default
- Cookie consent banners where all categories are pre-selected
- Registration forms where "receive marketing communications" is opt-out rather than opt-in

**Why it destroys trust:** The user didn't consent — they failed to un-consent. This is legally problematic in many jurisdictions and ethically problematic everywhere. Users who notice feel manipulated.

**Replace with:** All consent checkboxes unchecked by default. The user opts in to what they want. This may reduce conversion rates for marketing lists, but the users on the list actually want to be there.

### Dark-pattern cookie banners

**How to spot it in code:**
- "Accept all" as a prominent button, "Manage preferences" as a text link or secondary style
- No "Reject all" option
- Preference management that requires toggling 20 categories individually to reject
- Banner that reappears if the user doesn't click "Accept"
- Pre-selected non-essential cookie categories in the preference manager

**Why it destroys trust:** Cookie banners are the most common dark pattern on the web. Users are increasingly aware of the manipulation. A manipulative cookie banner is the first thing many users see — it sets the tone for the entire relationship.

**Replace with:** Equal visual weight for "Accept" and "Reject." Clear preference management with category-level toggles. Non-essential categories off by default. The banner respects the user's choice and doesn't reappear.

### Unnecessary data collection

**How to spot it in code:**
- Registration forms requiring data that isn't used by the product (phone number for a SaaS with no phone features, birthday for a project management tool)
- Analytics tracking that captures more than what's needed for product improvement
- Third-party tracking pixels for advertising on a product that doesn't advertise to its own users

**Why it destroys trust:** Every data point collected is a trust cost. The user asks "why do they need this?" If there's no good answer, the conclusion is "they're going to sell it or use it against me."

**Replace with:** Collect only what the product needs to function. If you need data for analytics, be explicit about it and make it opt-in. If a field is optional, mark it optional and explain why it's helpful.

---

## Cancellation friction

Patterns that make leaving difficult, hostile, or confusing. This category is severe enough to warrant its own section.

### Multi-page retention flows

**How to spot it in code:**
- Cancellation route that passes through 3+ pages before reaching the actual cancel action
- Each page offers a different retention incentive (discount, feature upgrade, pause option)
- The actual cancel button appears only after multiple "Are you sure?" gates
- Progress indicators that don't show how many steps remain

**Why it destroys trust:** The user has already decided to leave. Making the exit painful confirms their decision was right and generates social media complaints that prospective users will find.

**Replace with:** Two steps maximum. Step 1: "Cancel subscription" button in settings. Step 2: Confirmation with clear consequences ("Your access ends [date]. Your data will be available for export for 30 days."). Optional single feedback question with "Skip" equally prominent.

### Guilt-tripping cancellation copy

**How to spot it in code:**
- Emotional language in cancellation flows: "We'll miss you," "Are you sure you want to lose access to X?," "Your team will lose their work"
- Framing cancellation as loss rather than choice
- Listing features the user will "lose" instead of clearly stating what happens

**Why it destroys trust:** Guilt-tripping at exit poisons the user's memory of the entire product experience. A user who had a positive experience will remember the guilt trip, not the product.

**Replace with:** Factual, neutral language. "Your subscription will end on [date]. You can export your data before then. If you change your mind, you can re-subscribe anytime." No emotion, no loss framing, no guilt.

### Hidden cancel button

**How to spot it in code:**
- No "Cancel subscription" option in account settings or billing page
- Cancellation requires navigating to a URL not linked from any settings page
- "Cancel" button styled as a text link in a color close to the background
- Cancellation option labeled as something else ("Manage plan" leading to a buried cancel link)

**Why it destroys trust:** The user is actively looking for the cancel button. Not finding it generates immediate frustration and the conclusion that the company is deliberately making it hard. This is one of the most commonly shared complaints on social media.

**Replace with:** "Cancel subscription" in the billing or account settings, styled as a normal interactive element. Not hidden, not renamed, not requiring a treasure hunt.
