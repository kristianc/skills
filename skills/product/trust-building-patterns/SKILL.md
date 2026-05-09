---
name: trust-building-patterns
description: Audit and improve the trust signals in a product — social proof, pricing transparency, security indicators, honest copy, and risk reduction patterns. Use when the user wants to increase conversion, reduce signup friction, build credibility, audit for dark patterns, improve pricing page trust, or understand why prospects drop off before paying.
---

# Trust Building Patterns

Audit and improve the structural decisions that make users trust a product enough to sign up, pay, and stay. This is not visual polish (that's ai-design-polish) — it's the design patterns that communicate credibility, transparency, and honesty.

The core insight: trust is built by reducing the cost of being wrong. Every pattern should make the user feel like "even if this doesn't work out, I won't get burned."

Trust is not a single moment. It's a journey — visit, signup, first use, payment, renewal — and each moment has different trust needs. A logo bar on the homepage doesn't help at the cancellation screen. A money-back guarantee doesn't help if the user can't find the pricing.

## Glossary (see GLOSSARY.md)

Use these terms precisely. "Trust" is too broad to be useful — name the specific mechanism.

- **Trust signal** — a concrete element that reduces perceived risk
- **Trust moment** — a point where the user must decide to trust before continuing
- **Trust debt** — accumulated erosion from small dishonest choices
- **Dark pattern** — a design that tricks users into unintended actions
- **Risk reduction** — making it easy to reverse decisions if things go wrong

## Process

### 1. Map the trust journey

Identify every moment where the user must decide to trust the product. The five trust moments:

1. **Pre-signup** (visitor to consideration) — "Is this real? Is it credible? Will it do what it claims?"
2. **Signup** (consideration to commitment) — "What am I giving up? What do I get? Can I leave?"
3. **Post-signup** (first use to activation) — "Did it deliver? Is my data safe? Can I get help?"
4. **Payment** (activation to monetization) — "What does it really cost? What if I change my mind?"
5. **Ongoing** (retention to advocacy) — "Is this product alive? Does it respect me? Will it be here next year?"

For each moment, identify what the user needs to believe and what evidence is present.

### 2. Audit current signals

Walk each trust moment and score the signals present. Use the Agent tool with `subagent_type=Explore` to read the actual code — landing pages, signup flows, pricing pages, settings, cancellation flows, error pages, status pages, footer links.

Reference TRUST-SIGNALS.md for what to look for at each stage. For each signal found, note whether it's authentic, generic, or absent.

Score each trust moment on three axes (see SCORING-RUBRIC.md):
- **Signal strength** (1-5) — are trust signals present, relevant, and credible?
- **Honesty** (1-5) — is anything misleading, hidden, or manipulative?
- **Risk reduction** (1-5) — how easy is it for the user to reverse their decision?

### 3. Identify gaps and anti-patterns

Where trust is missing (no signals), where it's undermined (dark patterns, dishonest copy), where it's generic (stock testimonials, meaningless badges). Reference ANTI-PATTERNS.md for the full catalog of trust-destroying patterns and what to look for in code.

Flag any honesty score below 3 on any moment as a trust blocker — a single dishonest moment poisons all the authentic ones.

### 4. Present findings

Organize by trust moment. For each finding:

- **Where** — which page, flow, or component
- **What** — the specific trust signal, gap, or anti-pattern
- **Severity** — blocker (dishonesty, dark pattern), important (missing signal, generic signal), or minor (could be stronger)
- **Impact** — what the user experiences or concludes
- **Recommendation** — what to change, referencing the specific pattern from TRUST-SIGNALS.md

Present blockers first. Then ask the user: "Which of these should I address?"

### 5. Implement

Apply the patterns from TRUST-SIGNALS.md. Make real code changes — don't just describe what should change. For copy changes, write the actual copy. For structural changes (adding a status page link, restructuring a pricing table, adding a cancellation flow), implement them.

After changes, re-score the affected trust moments to verify improvement.

## Rules

- Always read the actual code. Audit the real signup flow, the real pricing page, the real cancellation flow. Don't guess from descriptions.
- Honesty blockers come first. No amount of social proof compensates for a dark pattern.
- Authentic beats impressive. "247 paying teams" is better than "thousands of customers." A real testimonial from a small company beats a fake one from a big one.
- Cross-reference error-copy-review for error honesty patterns. Error messages are trust moments.
- Don't add trust signals that aren't true. If the product doesn't have real testimonials, the answer is to earn them, not to fake them. Recommend what to build, not what to fabricate.
- Risk reduction is the highest-leverage pattern. Making it easy to leave is what makes people comfortable staying.
