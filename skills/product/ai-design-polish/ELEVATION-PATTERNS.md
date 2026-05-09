# ELEVATION-PATTERNS.md

The fixes, organized by the most common AI defaults. Each pattern includes the problem, the fix, the principle, and common mistakes when applying the fix.

These are not suggestions. These are the specific code changes that close the gap between "AI-generated" and "premium template." Apply them as written, then adjust to the specific design's intent.

---

## 1. Spacing too tight

### The problem

AI produces cramped designs. Section gaps of `1rem`-`2rem`, card padding of `0.75rem`, elements stacked with `0.25rem` gaps. Content touches its containers. Sections blur together. The design feels anxious — like it's worried about wasting space.

### The fix

```css
/* Before */
.section { padding: 2rem 1rem; }
.card { padding: 0.75rem; }
.section + .section { margin-top: 1rem; }

/* After */
.section { padding: 5rem 1.5rem; }
.card { padding: 1.5rem; }
.section + .section { margin-top: 0; } /* padding handles it */
```

For Tailwind:
```html
<!-- Before -->
<section class="py-8 px-4">
<div class="p-3">

<!-- After -->
<section class="py-20 px-6">
<div class="p-6">
```

### The principle

Whitespace is not wasted space. It is an active design element that communicates hierarchy, separates concerns, and gives content room to be read. Premium templates use space as generously as they use color or typography. The 8px grid (0.5rem increments) provides rhythm without randomness.

### Common mistakes

- Going too generous and making the design feel empty. Sections should breathe, not float in a void. `6rem` section gaps are generous. `12rem` is a desert.
- Increasing space uniformly. Space should reflect hierarchy: more between sections than between items within a section. More between groups than between items within a group.
- Forgetting mobile. Generous desktop spacing needs to scale down. Use responsive values: `py-20 md:py-24 lg:py-32`.

---

## 2. Too many colors

### The problem

AI reaches for color variety. A blue primary button, green success badge, orange warning, purple accent, teal link — all on one page. Each color was individually reasonable, but together they create visual noise. No color feels important because every color competes.

### The fix

1. Identify the one brand/primary color that should stand out.
2. Convert all other accent colors to the neutral palette or to the primary color at different opacities.
3. Keep semantic colors (red for destructive, green for success) but make them muted — not saturated.

```css
/* Before — 5+ hues */
.btn-primary { background: #3b82f6; }
.badge-success { background: #22c55e; }
.link { color: #06b6d4; }
.accent { color: #8b5cf6; }
.alert { background: #f97316; }

/* After — 1 accent, neutral everything else */
.btn-primary { background: #3b82f6; }
.badge-success { background: rgba(34, 197, 94, 0.15); color: #22c55e; }
.link { color: #3b82f6; } /* same as primary */
.accent { color: #3b82f6; } /* same as primary */
.alert { background: rgba(249, 115, 22, 0.1); color: #f97316; } /* muted bg */
```

### The principle

Restraint is sophistication. One accent color used consistently creates a visual thread through the design. When everything is colorful, nothing stands out. When one element has color, it commands attention. Premium templates treat color like punctuation — used sparingly and meaningfully.

### Common mistakes

- Making the design monochrome. The goal is 1-2 accents, not zero. The primary color should still be vibrant and confident.
- Forgetting semantic colors entirely. Red for errors and destructive actions, green for success — these serve function, not decoration. Keep them, but mute their backgrounds.
- Not updating hover/focus states to match the restrained palette. If the button is blue, its hover should be a darker blue, not a new color.

---

## 3. Heavy shadows

### The problem

AI produces `box-shadow: 0 4px 20px rgba(0,0,0,0.25)` or worse. These heavy shadows were popular in 2016-era design but now read as dated and heavy-handed. They draw attention to the container instead of the content. In dark mode, they look even worse — dark on dark with a heavy blur.

### The fix

```css
/* Before */
.card {
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.25);
}

/* After — light mode */
.card {
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);
  border: 1px solid rgba(0, 0, 0, 0.06);
}
.card:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

/* After — dark mode */
.card {
  box-shadow: none;
  border: 1px solid rgba(255, 255, 255, 0.08);
}
.card:hover {
  border-color: rgba(255, 255, 255, 0.15);
}
```

### The principle

Depth in modern design is communicated through subtle means — thin borders, minimal shadows, slight elevation changes on interaction. The content should float above the surface, not cast a dramatic shadow across it. In dark mode, borders replace shadows entirely because dark-on-dark shadows are invisible or muddy.

### Common mistakes

- Removing shadows entirely and relying only on borders. Resting state can use a `1px` border, but hover state benefits from a subtle shadow increase to signal interactivity.
- Using the same shadow value on every element. Navigation bars, cards, modals, and dropdowns should have different shadow levels reflecting their z-position.
- Forgetting to differentiate shadow treatment between light and dark mode. Shadows that work in light mode are usually invisible in dark mode — switch to border-based depth.

---

## 4. Generic copy

### The problem

AI produces copy that could appear on any product's website: "Welcome to our platform!", "Get started today!", "We're here to help you succeed." This copy says nothing about the product, makes no specific claims, and uses the excited-but-empty tone that marks everything as generated. See COPY-UPGRADE.md for the full treatment.

### The fix

Replace every piece of generic copy with something specific to the product. The specificity test: could this sentence appear on a competitor's site unchanged? If yes, it's generic.

```
Before: "Welcome to our platform! Get started today."
After:  "Reconcile invoices in half the time. No imports, no formatting — paste and go."

Before: "Trusted by thousands of happy customers"
After:  "25,000+ finance teams process $2.3B monthly"

Before: "Our powerful features help you succeed"
After:  "Sub-200ms queries across 10M+ row datasets"
```

### The principle

Specific copy is confident copy. A product that says "average savings of 5% on processing fees" is making a claim specific enough to be wrong — and that specificity signals that it's based on real data. Generic copy ("save money!") signals that the writer had nothing real to say.

### Common mistakes

- Making up specific numbers. If the product doesn't have real metrics, use specific descriptions instead of fabricated stats. "Paste a CSV and see results in seconds" is specific without requiring data.
- Overcorrecting into jargon. Specific doesn't mean technical. "Sub-200ms P95 query latency" is specific but alienating for most audiences. Match the audience's vocabulary.
- Rewriting copy that was intentionally simple. CTAs like "Sign up" or "Log in" don't need to be clever. Not every piece of text needs personality — functional labels should be functional.

---

## 5. Inconsistent typography

### The problem

AI assigns font sizes per-element rather than establishing a scale. The hero heading might be `2.25rem`, section headings range from `1.5rem` to `1.875rem` to `1.25rem`, and body text varies between `0.875rem` and `1rem`. The result is a hierarchy that the eye can't parse — nothing is clearly more important than anything else.

### The fix

Establish a 3-4 level type scale and enforce it:

```css
/* Before — ad hoc sizes */
.hero h1 { font-size: 2.25rem; }
.section-a h2 { font-size: 1.875rem; }
.section-b h2 { font-size: 1.5rem; }
.section-c h2 { font-size: 1.25rem; }
.body-text { font-size: 0.9375rem; }
.small-body { font-size: 0.875rem; }

/* After — systematic scale */
.heading-display { font-size: clamp(1.5rem, 5vw, 3.5rem); line-height: 1.1; letter-spacing: -0.02em; font-weight: 700; }
.heading-section { font-size: clamp(1.25rem, 3vw, 2rem); line-height: 1.2; letter-spacing: -0.01em; font-weight: 600; }
.body { font-size: 1rem; line-height: 1.6; font-weight: 400; }
.small { font-size: 0.875rem; line-height: 1.5; font-weight: 400; }
```

### The principle

A type scale is a constraint that creates clarity. When there are only three heading levels, the reader's eye learns the hierarchy instantly. When there are eight slightly different sizes, the eye gives up and reads linearly. Premium templates commit to a small scale and use it relentlessly — the same `h2` style on every section heading, no exceptions.

### Common mistakes

- Setting all headings to the same size. The fix for "too many sizes" is not "one size" — it's a deliberate scale with clear steps between levels.
- Forgetting to set `line-height` with each size. Large headings need tight line-height (`1.1`-`1.2`). Body text needs generous line-height (`1.5`-`1.6`). Inheriting the same line-height across all sizes is a common AI default.
- Not making display headings large enough. `clamp(1.5rem, 5vw, 3.5rem)` means the heading might be `3.5rem` (56px) on desktop. That feels too big when you first see it in code, but it's exactly what gives premium templates their confident presence.

---

## 6. Missing hover states and transitions

### The problem

AI produces interactive elements — buttons, cards, links — with no visual change on hover. The cursor changes, but the element itself gives no feedback. The design feels static and unresponsive, like a PDF rather than an interface.

### The fix

Add hover states and transitions to every interactive element:

```css
/* Buttons */
.btn-primary {
  background: #1a1a1a;
  transition: background-color 150ms ease, box-shadow 150ms ease;
}
.btn-primary:hover {
  background: #333333;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
}

/* Secondary/outline buttons */
.btn-secondary {
  border: 1px solid rgba(0, 0, 0, 0.2);
  transition: border-color 150ms ease, background-color 150ms ease;
}
.btn-secondary:hover {
  border-color: rgba(0, 0, 0, 0.4);
  background: rgba(0, 0, 0, 0.04);
}

/* Cards */
.card {
  transition: transform 200ms ease, box-shadow 200ms ease;
}
.card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

/* Links */
a {
  transition: color 150ms ease;
}
a:hover {
  color: /* slightly darker or lighter variant */;
}
```

### The principle

Every interactive element should acknowledge interaction. Hover states are the visual equivalent of feedback — they tell the user "yes, this is clickable, and something will happen." Transitions between states should be fast enough to feel responsive (`150ms`-`200ms`) and use `ease` timing to feel natural, not mechanical.

### Common mistakes

- Adding `transition: all 0.3s ease` as a blanket rule. Transition specific properties — `all` can cause unexpected animations on layout properties and hurts performance.
- Making hover states too dramatic. `scale(1.1)` on a card hover is distracting. `translateY(-2px)` with a shadow increase is enough.
- Forgetting active/pressed states. Buttons should have a pressed appearance: slightly darker, or `translateY(1px)` to simulate being pushed. This completes the interaction arc: resting, hovering, pressed, released.
- Omitting focus-visible states. Keyboard users need focus indicators. Use `outline` or `box-shadow` on `:focus-visible`.

---

## 7. No section rhythm

### The problem

AI produces designs where every section has the same background, the same width, and the same structure. Content runs together without visual pacing. The page reads as one long scroll with no landmarks — the user can't tell where one idea ends and another begins.

### The fix

Alternate between full-width and contained treatments:

```css
/* Full-width section (hero, social proof, final CTA) */
.section-full {
  width: 100%;
  padding: 5rem 1.5rem;
  background: #0f0f0f; /* or a subtle gradient, or a slightly different shade */
}
.section-full .container {
  max-width: 1200px;
  margin: 0 auto;
}

/* Contained section (features, details, pricing) */
.section-contained {
  max-width: 1200px;
  margin: 0 auto;
  padding: 5rem 1.5rem;
}
```

Alternate background colors between sections:

```css
.section:nth-child(odd) { background: #0f0f0f; }
.section:nth-child(even) { background: #141414; }
/* Or use explicit classes for intentional alternation */
```

### The principle

Visual rhythm works like paragraphs in writing — each section is a unit of thought with clear boundaries. The alternation between full-width backgrounds and contained content creates "breathing points" that let the user process what they just read before moving on. This pacing is what gives premium landing pages their "guided tour" quality.

### Common mistakes

- Making every section full-width. The alternation is the point. If every section has a colored background, nothing stands out and the design feels heavy.
- Using drastically different background colors. The alternation should be subtle — `#0f0f0f` vs. `#141414`, not `#000000` vs. `#333333`. The user should feel the rhythm without being jarred by contrast.
- Forgetting to maintain consistent internal structure. Each section should use the same content container width even when the background width changes. `max-width: 1200px` inside every section, whether the section is full-width or not.

---

## 8. Pill buttons

### The problem

AI loves `border-radius: 9999px` on buttons. This creates pill-shaped buttons that look playful but not professional. Combined with heavy shadows or bright colors, pill buttons are one of the strongest AI tells. They also create visual inconsistency when used alongside cards with `8px` radius.

### The fix

```css
/* Before */
.btn { border-radius: 9999px; }

/* After */
.btn { border-radius: 6px; } /* or 8px — match your card radius system */
```

Also fix the CTA pattern — AI often produces a single, loud primary button. Premium templates use dual-CTA:

```html
<!-- Before -->
<button class="btn-primary btn-large">Get Started Now!</button>

<!-- After -->
<div class="cta-group" style="display: flex; gap: 1rem;">
  <button class="btn-primary">Start free trial</button>
  <button class="btn-secondary">See how it works</button>
</div>
```

Button sizing:

```css
/* Before — oversized or inconsistent */
.btn { padding: 16px 48px; font-size: 1.125rem; }

/* After — refined */
.btn { padding: 12px 24px; font-size: 0.9375rem; }
.btn-lg { padding: 14px 28px; font-size: 1rem; }
```

### The principle

Button radius should match the design system's radius scale. If cards use `8px`, buttons use `6px`-`8px`. If cards use `12px`, buttons use `8px`-`10px`. The dual-CTA pattern gives users a primary path (action) and a secondary path (learn more) — it reduces decision pressure and increases conversion by offering a lower-commitment alternative.

### Common mistakes

- Overcorrecting to `border-radius: 0`. Sharp corners can look brutalist but also aggressive. A subtle `6px`-`8px` is the target.
- Making both CTAs look the same. Primary gets filled background. Secondary gets an outline or ghost treatment. The visual weight difference must be obvious.
- Using "Get Started" as the primary CTA. Replace with a specific action: "Start free trial," "Create your first report," "See pricing." See COPY-UPGRADE.md.

---

## 9. Fake social proof

### The problem

AI fabricates social proof — "John D., Happy Customer" with a generic testimonial, or random company names that clearly don't use the product. Stock photos for testimonial avatars. Claims like "Trusted by thousands" with no specifics. Users recognize fake social proof instantly, and it undermines trust more than no social proof at all.

### The fix

If real social proof exists, use it with full attribution:

```html
<!-- Before -->
<blockquote>
  "Great product! Really helped our team."
  <cite>— John D., Happy Customer</cite>
</blockquote>

<!-- After -->
<blockquote>
  "Cut our reconciliation time from 3 hours to 20 minutes. 
   We process 2,000+ invoices monthly and haven't had a mismatch since switching."
  <cite>
    <strong>Sarah Chen</strong>
    <span>VP Finance, Meridian Health</span>
  </cite>
</blockquote>
```

If real social proof does not exist, remove the section entirely or replace with a factual claim:

```html
<!-- Instead of fake testimonials -->
<section class="social-proof">
  <p>Processing $2.3B in monthly transactions</p>
  <!-- or simply omit the section -->
</section>
```

For company logos, desaturate and reduce opacity:

```css
.logo-strip img {
  filter: grayscale(100%);
  opacity: 0.5;
  transition: opacity 200ms ease, filter 200ms ease;
}
.logo-strip img:hover {
  opacity: 1;
  filter: grayscale(0%);
}
```

### The principle

Social proof works because it transfers trust from known entities to unknown ones. Fake social proof does the opposite — it signals that the product has no real users willing to vouch for it. A specific quote with a real name, role, and company is worth more than ten generic testimonials. If there are no real testimonials, an honest metric ("500 invoices processed this month") is more trustworthy than a fabricated review.

### Common mistakes

- Leaving placeholder testimonials "to be filled in later." They never get filled in, and placeholder content on a live site is worse than nothing.
- Using only first names or initializations. "Sarah C." reads as partially anonymized, which reads as fabricated. Full names with roles and companies signal real people.
- Over-desaturating logos so they become unrecognizable. `grayscale(100%) opacity(0.5)` should still allow users to identify the company. If the logo is too faint to recognize, it's not serving as social proof.

---

## 10. Stock icons and illustrations

### The problem

AI generates designs with inconsistent icon styles — mixing filled and outlined icons, varying stroke weights, different sizes, or using overly decorative illustrations that don't match the product's tone. The result looks assembled from multiple free icon packs rather than designed as a system.

### The fix

Pick one icon style and enforce it:

```css
/* Consistent icon system */
.icon {
  width: 24px;    /* consistent size for inline icons */
  height: 24px;
  stroke-width: 1.5; /* consistent stroke weight */
  color: currentColor; /* inherits text color */
}

.icon-lg {
  width: 48px;    /* feature section icons */
  height: 48px;
}
```

For feature section icons, use a consistent container:

```html
<!-- Before — random icon treatments -->
<div style="font-size: 48px;">&#x2728;</div>
<img src="random-illustration.svg" width="60">
<i class="fas fa-rocket fa-3x text-blue-500"></i>

<!-- After — consistent icon system -->
<div class="icon-container">
  <svg class="icon-lg"><!-- consistent stroke icon --></svg>
</div>
```

```css
.icon-container {
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--color-primary);
  /* Optional: subtle background */
  background: rgba(var(--color-primary-rgb), 0.1);
  border-radius: 12px;
}
```

### The principle

Icons are part of the design system. Like typography and color, they need a rule set: one style (outline or filled, not both), one stroke weight, one size scale, one color approach (monochromatic, matching text color). Premium templates use icons as punctuation — consistent, quiet, functional. AI-generated designs use them as decoration — inconsistent, colorful, distracting.

### Common mistakes

- Switching from a mixed icon set to a different mixed icon set. The fix is consistency, not different inconsistency. Use Lucide, Phosphor, or Heroicons — pick one and stick to it.
- Making feature icons too large. A `48px` icon for feature sections is sufficient. `96px` icons dominate the content and make the section feel like an icon gallery.
- Adding decorative illustrations because "the section feels empty." If the section feels empty after the icons are consistent, the problem is spacing or content, not missing decoration.
