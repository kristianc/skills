# PATTERN-EXTRACTION.md

How to extract reusable patterns from a teardown and apply them to your own product. The goal is learning, not copying — steal the structure, not the surface.

---

## The difference between copying and learning

**Copying** is taking a specific design and reproducing it. You see a site with a dark hero, gradient accent, and floating testimonial card — you make yours look the same. This fails because:

- The surface is tailored to their audience, brand, and context
- Without understanding why it works, you copy the wrong aspects
- Two products that look identical communicate "we couldn't think of our own approach"

**Learning** is understanding the underlying structure that makes something effective, then building your own version for your context. You see the same site and understand: "they place a trust signal (testimonial) adjacent to the primary CTA, creating a trust-conversion loop — the proof appears at the moment of decision." Now you can implement that principle with your own proof, your own CTA, your own visual language.

**The test:** If you can explain why the pattern works in one sentence without referencing the source site's specific implementation, you've learned. If you can only describe what it looks like, you're copying.

---

## Identifying transferable patterns

Not every pattern is transferable. What works on one site may fail on another because of audience, context, or brand differences.

### Universal patterns (transfer freely)

Patterns that work because of human psychology, not because of a specific context:

- **Trust signals adjacent to decision moments** — works for any product because humans need reassurance at the moment of commitment
- **Progressive disclosure** — works for any complex product because cognitive load is universal
- **Social proof specificity** — "12,847 teams" always outperforms "thousands of customers" because specific = credible
- **Single dominant CTA** — works for any page because decision paralysis is universal
- **Value prop above the fold** — works for any site because attention is scarce universally

### Audience-dependent patterns (transfer with adaptation)

Patterns that work for a specific audience type:

- **Dark mode / terminal aesthetics** — signals "for developers" but alienates non-technical buyers
- **Enterprise design language** (conservative palette, formal typography) — signals credibility to enterprise buyers but feels corporate to startups
- **Playful copy and bold colors** — works for consumer/creative audiences but undermines credibility for security or finance products
- **Technical depth in the hero** — works for developers who want to see the code immediately, not for executives who want business outcomes

**How to adapt:** Identify what the pattern achieves (the function), then find the equivalent signal for your audience. "Terminal aesthetics" achieves "this is for technical people" — if your audience is technical but not developers (data scientists, security engineers), find the equivalent signal for that group.

### Industry-dependent patterns (transfer with caution)

Patterns that only work within a specific market context:

- **"No credit card required"** — powerful in markets where competitors require one, meaningless in markets where free trials are universal
- **Pricing page with annual toggle** — standard in SaaS, confusing in other contexts
- **Comparison tables against named competitors** — works when you're the underdog, backfires when you're the leader (draws attention to alternatives)
- **Compliance badges** — critical in healthcare/finance, irrelevant in creative tools

**How to evaluate transfer:** Ask "does my audience have the same expectation or concern that this pattern addresses?" If not, the pattern doesn't transfer regardless of how well it's executed elsewhere.

---

## Adapting a pattern to a different context

### Step 1: Name the principle

Strip away the surface and state what the pattern achieves in one sentence.

- "They use customer logos linked to case studies" becomes "Social proof is interactive — it leads to deeper evidence"
- "They show the product before asking for signup" becomes "Demonstrate value before requesting commitment"
- "The hero has one headline, one sentence, one button" becomes "Reduce the hero to a single assertion and a single action"

### Step 2: Identify your constraints

What's different about your context that changes how the principle should be implemented?

- **Audience:** Who are you talking to? What signals do they trust?
- **Maturity:** Do you have the assets (case studies, logos, usage numbers) to implement this pattern authentically?
- **Brand:** Does the implementation need to feel enterprise, developer, consumer, playful, premium?
- **Technical:** Can you actually build this? (A live demo in the hero is a different engineering commitment than a screenshot.)

### Step 3: Build your version

Implement the principle within your constraints. The result should:

- Achieve the same function as the original pattern
- Look and feel native to your product and audience
- Not require assets you don't have (don't fake social proof because you saw a site with good social proof)

---

## The "steal the structure, not the surface" principle

Structure is how something is organized and why. Surface is how it looks.

| Level | What to observe | What to steal |
|-------|----------------|---------------|
| **Surface** | Colors, fonts, specific images, exact copy | Nothing — this is their brand |
| **Layout** | Section order, spatial relationships, hierarchy | The logic of the arrangement |
| **Structure** | Why sections are in this order, what each section achieves | The narrative architecture |
| **Principle** | The human insight that makes the structure work | The underlying truth |

**Example:** A competitor's pricing page.

- Surface: Blue gradient, Inter font, 3-column card layout (don't steal)
- Layout: Features above pricing, comparison below, FAQ at bottom (steal the logic)
- Structure: Establish value before showing cost, make comparison easy, address objections (steal the narrative)
- Principle: People need to understand what they're buying before they'll evaluate the price (steal always)

---

## Feeding extracted patterns into other skills

Extracted patterns become inputs for:

### ai-design-polish

When you've identified a visual pattern worth adopting (e.g., "generous spacing communicates confidence"), add it as a reference point for the design polish process. The pattern becomes part of what "good" looks like for your product.

**How:** Note the specific values (spacing rhythm, type scale, color approach) and use them as reference standards when auditing and elevating your own designs.

### improve-product-feel

When you've identified an interaction pattern worth adopting (e.g., "undo instead of confirm for destructive actions"), it becomes vocabulary for describing touchpoint improvements.

**How:** Translate the pattern into a named convention for CONTEXT.md (e.g., "We use undo toasts for reversible actions, not confirmation modals — inspired by [source site]'s approach to low-friction flows").

### trust-building-patterns

When you've identified a trust pattern worth adopting (e.g., "place testimonials adjacent to CTAs, not in a separate section"), it becomes a specific recommendation for trust signal placement.

**How:** Reference the pattern in the trust audit — "competitor places trust signals at decision moments, we place them only in the hero. Gap: move proof to where decisions happen."

---

## When not to extract patterns

Not everything notable is worth extracting:

- **It only works at their scale.** "They have 50 case studies so they can match testimonials to every page" — if you have 3 testimonials, the pattern doesn't apply yet.
- **It only works for their audience.** A pattern that works for enterprise buyers may actively harm your startup-focused positioning.
- **It requires assets you can't authentically produce.** Don't fake social proof, manufacture case studies, or invent usage numbers because a competitor has real ones.
- **It's a surface choice, not a structural one.** Their specific blue accent color is not a transferable pattern. Their use of color as punctuation (accent only on CTAs) is.

The test: can you implement this pattern honestly and natively, without faking anything or fighting your actual context? If yes, extract and adapt. If no, note it as a benchmark to grow toward.
