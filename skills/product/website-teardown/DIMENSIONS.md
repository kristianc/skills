# DIMENSIONS.md

The eight analysis dimensions for a website teardown. For each dimension: what to look for, how to evaluate it, and what separates intentional from accidental.

---

## 1. First impression (0-5 seconds)

What the visitor understands before they scroll or read carefully. This is the most important dimension because it determines whether anyone stays long enough to encounter the rest.

### What to look for

- **Hero clarity:** Can you state what the product does from the hero alone? One sentence, no jargon.
- **Value proposition placement:** Is it the largest text on the screen? Is it above the fold? Is it one sentence or a paragraph?
- **Visual hierarchy:** What does the eye hit first, second, third? Does the hierarchy serve the message or fight it?
- **Above-the-fold inventory:** What's competing for attention? Nav, hero, CTA, social proof, animation, images — how many elements demand the eye simultaneously?
- **Audience signal:** Can a stranger tell who this is for within 5 seconds? Developers, marketers, founders, enterprise, consumer?
- **Emotional register:** What does the design say before the copy does? Premium, playful, serious, technical, warm, corporate?

### How to evaluate

**Good (4-5):** A stranger can state what the product does and who it's for without scrolling. The hierarchy leads the eye from value prop to supporting evidence to CTA. One thing dominates; the rest supports.

**Acceptable (3):** The value prop is present but not dominant. Competing elements dilute attention. The visitor probably understands within 10-15 seconds of reading, not 5 seconds of scanning.

**Poor (1-2):** The hero is a tagline that could apply to any product. The value prop is buried below the fold or split across multiple sections. The visitor must work to understand what the product does. Or — the hero is so cluttered that nothing registers.

### Examples

- **Good:** "Automated invoice processing for accounting teams. Extract, match, and approve in seconds." (Specific product, specific audience, specific benefit.)
- **Acceptable:** "The modern platform for financial operations." (Category is clear, specifics are not.)
- **Poor:** "Empower your team to do more." (Could be literally anything.)

---

## 2. Information architecture

How content is organized — what the navigation and page structure tell you about priorities.

### What to look for

- **Navigation items:** What's in the top-level nav? What does the ordering communicate about priorities?
- **Navigation depth:** How many levels deep? Are important things buried under dropdowns?
- **Page structure:** What sections exist on the homepage, and in what order? What gets the most real estate?
- **Audience paths:** Is there routing for different audiences (by role, by use case, by company size)?
- **Missing pages:** What would you expect to find that isn't in the nav? (Pricing, docs, changelog, about, case studies, status)
- **Footer inventory:** What's in the footer? The footer reveals what the company thinks is important enough to persist on every page.

### How to evaluate

**Good (4-5):** The nav matches how the target audience thinks about the product (by use case, not by feature). Important pages are one click away. The structure tells a story — you can infer the company's priorities from the IA alone.

**Acceptable (3):** Standard SaaS navigation. Nothing is hard to find, but nothing is optimized for a specific audience. The structure is functional but not strategic.

**Poor (1-2):** Important information is buried or missing. The nav is organized by internal company structure (Products, Solutions, Resources, Company) rather than by what visitors need. Critical paths require 3+ clicks.

### Examples

- **Good:** A developer tool with top-level nav: Docs, Pricing, Changelog, Blog. Prioritizes what developers actually navigate to.
- **Acceptable:** Standard SaaS nav: Product, Solutions, Pricing, Resources, Company. Functional, forgettable.
- **Poor:** A B2B tool where Pricing requires navigating to "Solutions > Enterprise > Contact Us" to discover it's custom-only.

---

## 3. Copy & messaging

What the site claims, how it claims it, and who it's speaking to.

### What to look for

- **Headline claim:** What's the primary assertion? Is it specific (measurable, provable) or generic (aspirational, unfalsifiable)?
- **Specificity:** Numbers, timeframes, concrete outcomes vs. vague promises. "Reduce deploy time by 80%" vs. "Deploy faster."
- **Audience signal in copy:** Who does the language assume you are? Technical jargon signals developers. Business outcomes signal executives. "Your team" signals managers.
- **Proof backing claims:** For each claim, is there evidence on the same page? Testimonials, case studies, screenshots, numbers?
- **Tone:** Enterprise (formal, cautious, comprehensive), startup (bold, casual, concise), developer (technical, direct, minimal), consumer (warm, simple, emotional).
- **AI copy tells:** Generic superlatives, empty modifiers ("powerful," "seamless," "intuitive"), sentences that sound impressive but say nothing specific. Cross-reference ai-design-polish COPY-UPGRADE.md.
- **CTA language:** What do the buttons say? "Start free trial" is honest. "Get started" when getting started requires a credit card is not. "See it in action" vs. "Book a demo" — which one reduces perceived commitment?

### How to evaluate

**Good (4-5):** Claims are specific and backed by evidence. The audience is obvious from the language. Copy does work — it advances understanding, not just fills space. Every section adds information the previous one didn't.

**Acceptable (3):** Copy is competent and clear but not differentiated. You could swap in a competitor's name and the copy would still work. Claims are reasonable but not specific enough to be provable.

**Poor (1-2):** Copy is generic, vague, or contradictory. Claims are aspirational without evidence. The tone is inconsistent (enterprise headline, casual body, technical feature descriptions). OR: the copy is clearly AI-generated — full of empty modifiers and sentences that don't advance understanding.

### Examples

- **Good:** "12,847 teams process 2.3M invoices/month through [Product]." (Specific, verifiable, social proof baked in.)
- **Acceptable:** "Thousands of teams trust [Product] for invoice processing." (Reasonable, but vague.)
- **Poor:** "The world's most powerful invoice solution." (Unfalsifiable, unverifiable, and signals insecurity.)

---

## 4. Visual design

What the design language communicates about the product and company.

### What to look for

- **Design register:** Premium/minimal, playful/bold, enterprise/safe, developer/technical, consumer/warm. What audience does the design target?
- **Typography:** Typeface family (serif, sans-serif, monospace), scale (how many sizes, how large is the hero), weight usage (bold for hierarchy or decoration?), line-height (tight = dense/technical, loose = readable/editorial).
- **Color palette:** How many colors? Is there a system (one accent, neutral foundation) or a grab bag? What do the colors communicate? Dark mode?
- **Spacing:** Generous (confident, premium) or tight (dense, utilitarian)? Consistent rhythm or random gaps?
- **Component style:** Rounded vs. sharp corners, heavy vs. light borders, cards vs. flat sections, contained vs. edge-to-edge.
- **Imagery:** Photography (real vs. stock), illustration (custom vs. generic), screenshots (real vs. mocked), abstract graphics.
- **Consistency:** Does the design system hold together? Or do different sections feel like different designers?

### How to evaluate

**Good (4-5):** The design communicates the right thing to the right audience. Choices form a coherent system. Typography, color, spacing, and components all tell the same story. The design earns the "premium" label through restraint and consistency, not through ornament.

**Acceptable (3):** Competent, clean, and functional. Standard SaaS template quality — nothing is wrong, but nothing is distinctive. The design doesn't actively communicate anything beyond "we exist."

**Poor (1-2):** Inconsistent system (mixed border radii, random spacing, competing colors). Heavy AI-generation tells (too many colors, heavy shadows, generic stock imagery, inconsistent component styles). Or: the design communicates the wrong thing — enterprise visual language for a product targeting indie developers.

### Examples

- **Good:** Linear's dark-mode-first interface with tight typography, monospace accents, and minimal color. Every choice says "developer tool built by people who care about design."
- **Acceptable:** Standard Tailwind template with Inter, blue accent, white background, card-based layout. Clean but forgettable.
- **Poor:** A homepage with 6 colors, 4 different border radii, stock photography mixed with custom illustrations, and sections that look like they came from different templates.

---

## 5. Conversion design

How the site moves visitors toward action — and what friction exists in that path.

### What to look for

- **CTA inventory:** How many CTAs are visible per screen? What actions do they offer? Is there a clear primary action?
- **CTA hierarchy:** Is there one dominant action (primary button) with supporting alternatives (secondary link)? Or do multiple buttons compete for attention?
- **Friction to value:** How many steps from landing to signup? From signup to first value? What does each step require?
- **CTA placement:** Are calls to action near the information that supports them? (Social proof near signup, features near "try it," pricing near "buy")
- **Trust at conversion points:** What trust signals appear adjacent to CTAs? Testimonials, guarantees, security badges, "no credit card required"?
- **Pricing visibility:** Is pricing on the site? Is it clear? Does it require contacting sales?
- **Competing actions:** Does the site ask the visitor to do too many things? (Subscribe, follow, download, sign up, book a demo, watch a video — all on one page)

### How to evaluate

**Good (4-5):** One clear primary action dominates. Trust signals appear at decision moments. The path from interest to signup is short and low-friction. Secondary actions exist but don't compete. The user always knows what to do next.

**Acceptable (3):** CTAs exist and are findable. The path to signup works but isn't optimized. Some friction could be reduced. Trust signals exist but aren't placed at decision moments.

**Poor (1-2):** Multiple competing CTAs with no clear hierarchy. No trust signals near conversion points. The path to value is unclear or requires excessive commitment upfront. Or: the only CTA is "Book a demo" with no self-serve option for a product that could support one.

### Examples

- **Good:** Hero CTA is "Start free — no credit card" with "See how it works" as a text link below. A testimonial about easy setup sits adjacent. One action dominates.
- **Acceptable:** Hero has "Get started" and "Watch demo" buttons at equal weight. Both are reasonable actions but the hierarchy is unclear.
- **Poor:** Hero has "Sign up," "Book a demo," "Watch video," and "Read docs" all as equally-weighted buttons. Below: "Subscribe to our newsletter" and "Follow us on Twitter." The visitor has decision paralysis.

---

## 6. Trust & credibility

What makes a visitor believe this product is real, credible, and safe to use.

### What to look for

- **Social proof type and quality:** Logo bars (real customers?), testimonials (specific, verifiable?), usage numbers (honest?), case studies (detailed?).
- **Social proof placement:** Near decision moments or only in the hero? Contextually relevant to the adjacent content?
- **Team visibility:** Can you find who's behind this? Are they credible for this domain?
- **Pricing transparency:** Is pricing visible? Clear? Honest about what things cost?
- **Security signals:** Compliance badges, data handling explanations, privacy-first defaults.
- **Risk reduction:** Free trial without credit card, money-back guarantee, easy cancellation, data export.
- **Liveness signals:** Changelog, blog activity, status page, last update date — is this product actively maintained?

Cross-reference trust-building-patterns/TRUST-SIGNALS.md for the full framework of what to evaluate at each trust moment.

### How to evaluate

**Good (4-5):** Multiple layers of authentic social proof. Team is visible and credible. Pricing is transparent. Risk reduction is explicit (free trial, easy cancellation). The product feels alive (recent updates, active changelog).

**Acceptable (3):** Some social proof exists but isn't comprehensive. Pricing is present but could be clearer. Risk reduction is implied but not made explicit. Standard trust signals without anything notable.

**Poor (1-2):** No social proof, anonymous team, hidden pricing, no risk reduction. Or: social proof that's obviously fake (stock photo testimonials, vague quotes, unverifiable claims). The product feels abandoned (no recent updates, dead blog, stale copyright year).

### Examples

- **Good:** Logo bar of recognizable customers linking to case studies. Named testimonials with specific outcomes. Pricing page with all costs visible. "Cancel anytime from your settings" stated explicitly.
- **Acceptable:** Logo bar exists, testimonials are present but generic ("Great tool!"), pricing requires some calculation to understand.
- **Poor:** No logos, no testimonials, "Contact sales for pricing," no about page, copyright 2022 with no visible activity since.

---

## 7. Technical signals

Performance, responsiveness, accessibility, and SEO basics — things that affect user experience and discoverability even if most visitors don't consciously notice them.

### What to look for

- **Load performance:** How quickly does the page become usable? Observable from fetch time and page weight. Heavy images, render-blocking scripts, excessive JavaScript.
- **Mobile responsiveness:** Does the viewport meta tag exist? Is there evidence of responsive design (media queries, responsive utilities, mobile navigation)?
- **Accessibility basics:** Alt text on images, ARIA labels on interactive elements, heading structure (h1 > h2 > h3 in order), sufficient color contrast, keyboard navigability.
- **SEO fundamentals:** Title tag, meta description, OG tags (title, description, image), canonical URL, heading hierarchy, structured data.
- **Code quality signals:** Is the HTML semantic? Is CSS organized (utility classes, BEM, CSS modules) or inline chaos? Are scripts deferred/async?

### How to evaluate

**Good (4-5):** Fast load, mobile-responsive, accessible, well-structured for SEO. The technical foundation doesn't get in the way of the content. Someone cared about the invisible layer.

**Acceptable (3):** Functional on mobile, loads reasonably, basic SEO is present. Nothing is broken, but optimization opportunities exist.

**Poor (1-2):** Slow to load, broken on mobile, no accessibility consideration, missing basic SEO tags. The technical layer actively harms the user experience or discoverability.

### Examples

- **Good:** Sub-2-second load, responsive breakpoints, semantic HTML, complete OG tags, images with alt text, deferred scripts.
- **Acceptable:** 3-4 second load, mobile-functional but not mobile-optimized, title and description present but OG tags missing.
- **Poor:** 8+ second load, content unreadable on mobile, no meta description, images without alt text, JavaScript errors in console.

---

## 8. What's missing

What a visitor would expect to find that isn't there. Gaps in the site's narrative — questions the visitor has that the site never answers.

### What to look for

- **Expected pages:** Pricing (for a SaaS product), About/Team (for credibility), Documentation (for technical products), Case studies (for enterprise products), Changelog (for active products), Status page (for infrastructure products).
- **Expected sections:** How it works (for complex products), Comparison to alternatives (for competitive markets), Integration list (for platform products), Security/compliance (for enterprise buyers).
- **Narrative gaps:** Questions the target audience would have that the site never addresses. "How is this different from X?" "What does it cost?" "Can I try it first?" "How long does setup take?" "What happens to my data?"
- **Functional gaps:** Expected functionality that doesn't exist. Search on a documentation-heavy site. Filtering on a comparison page. Dark mode on a developer tool. RSS on a blog.
- **Stage-specific gaps:** What's missing for different buying stages? Awareness stage (what is this?), consideration stage (how does it compare?), decision stage (what does it cost, can I try it?), post-purchase (how do I get help?).

### How to evaluate

**Good (4-5):** The site anticipates and answers the key questions for its audience. Expected pages exist and are findable. The narrative is complete — you don't finish the site with unanswered questions about what the product does, costs, or how to start.

**Acceptable (3):** Most expected content exists but some gaps remain. A question or two goes unanswered. The visitor can probably figure things out but has to work for it.

**Poor (1-2):** Critical information is missing. No pricing for a self-serve product. No documentation for a technical product. No way to try it before buying. The visitor leaves with more questions than answers.

### Examples

- **Good:** SaaS product with Pricing, Docs, Changelog, Status, About, Case Studies, Blog — all accessible from the nav or footer. Every buying-stage question is addressed.
- **Acceptable:** Product has most pages but no case studies and pricing requires "Contact us" for the mid-tier plan.
- **Poor:** Developer tool with no documentation, no pricing page, no changelog, and "Coming soon" on half the feature pages.
