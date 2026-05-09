# AUDIT-CHECKLIST.md

What to check when auditing an AI-generated design, and how to score it. Walk each dimension, read the actual code, and compare against the reference standard from REFERENCE-PATTERNS.md.

---

## Scoring

Rate the design on three axes. Each axis is scored 1-5. Anything below 3 on any axis signals work is needed.

### Axis 1: Craft (1-5)

Do the visual details show considered choices or AI defaults?

**Score 1 — Pure defaults.** The design uses AI-generated values unchanged. Heavy shadows, random colors, inconsistent radii, no hover states, default spacing. Multiple critical-severity items present.

**Score 2 — Some awareness.** One or two dimensions show consideration (e.g. decent typography), but most are defaults. The design reads as "generated with minor tweaks."

**Score 3 — Mixed.** Half the dimensions are considered, half are defaults. The design has good moments undermined by generic ones. Noticeable to design-conscious users.

**Score 4 — Mostly considered.** Most dimensions show deliberate choices. One or two minor defaults remain but don't dominate. Would pass casual review.

**Score 5 — Fully crafted.** Every dimension shows considered choices consistent with premium templates. No default tells. A design-conscious user would not suspect AI generation.

### Axis 2: Consistency (1-5)

Do the design decisions form a coherent system or a grab bag?

**Score 1 — No system.** Different components look like they came from different kits. Multiple shadow styles, border radii, spacing values, color choices with no relationship. Every section feels like a new design.

**Score 2 — Accidental consistency.** Some elements match, but only because AI happened to repeat a value, not because a system was established. Two cards might have the same shadow, but the third doesn't.

**Score 3 — Partial system.** Some dimensions are systematic (e.g. consistent typography), but others drift. The design has clusters of consistency with gaps between them.

**Score 4 — Strong system.** Most decisions are systematic. One radius for buttons, one for cards, one spacing scale, one color palette. Minor deviations exist but are exceptions, not the rule.

**Score 5 — Complete system.** Every value traces to a deliberate system. Spacing uses one scale, radii use one set, colors use one palette, shadows use one treatment. The design could be expressed as a design token set.

### Axis 3: Confidence (1-5)

Does the design commit to its choices or hedge with generic safe options?

**Score 1 — No commitment.** Everything is medium — medium type size, medium spacing, medium colors. The design avoids strong choices in any direction. It looks like it was designed to not offend.

**Score 2 — Tentative.** Some choices show direction (maybe a dark color scheme), but the execution hedges (small headings, tight spacing, safe button styles). The design has a concept but doesn't commit to it.

**Score 3 — Partially committed.** The hero might commit (big heading, bold color), but subsequent sections retreat to safe defaults. Confidence is uneven.

**Score 4 — Mostly committed.** Strong choices maintained through most of the design. Large headings, generous spacing, restrained palette, clear hierarchy. Minor hedging in secondary areas.

**Score 5 — Fully committed.** The design makes strong choices and maintains them throughout. Display headings are large. Whitespace is generous. The palette is restrained. Nothing looks like a fallback. The design has a point of view.

---

## Checklist by dimension

### Typography

| What to check | Where in code | AI default (problem) | Reference standard (target) | Severity |
|---|---|---|---|---|
| Hero heading size | `font-size` on `h1` or hero heading component | `1.5rem`-`2rem` (too small, doesn't command attention) | `clamp(1.5rem, 5vw, 3.5rem)` (responsive, confident) | Critical |
| Number of distinct font sizes | All `font-size` declarations | 6-8+ different sizes (no clear hierarchy) | 3-4 sizes used consistently (display, heading, body, small) | Critical |
| Heading line height | `line-height` on headings | `1.5` or inherited body value (too loose, headings float) | `1.1` to `1.2` (tight, creates visual mass) | Important |
| Body line height | `line-height` on body text | `1.3` or `1.8` (too tight or too academic) | `1.5` to `1.6` (comfortable reading) | Important |
| Font weight usage | `font-weight` declarations | 3+ weights, or everything is one weight | 2 weights: regular (400) for body, bold (600-700) for headings | Minor |
| Heading letter spacing | `letter-spacing` on large headings | `0` (default, doesn't tighten at size) | `-0.01em` to `-0.02em` (subtle tightening) | Minor |
| Font family | `font-family` declarations | Multiple fonts mixed, or decorative fonts | Single sans-serif family (Inter, Geist, system stack) | Important |

### Spacing

| What to check | Where in code | AI default (problem) | Reference standard (target) | Severity |
|---|---|---|---|---|
| Section gaps | `gap`, `margin`, `padding` between major sections | `1rem`-`2rem` (cramped, sections run together) | `4rem`-`6rem` (sections breathe, clear separation) | Critical |
| Card internal padding | `padding` on card/panel components | `0.75rem`-`1rem` (content touches edges) | `1.5rem`-`2rem` (content breathes inside container) | Critical |
| Container max-width | `max-width` on content containers | None (full-width text) or `1600px`+ (too wide) | `1200px` for layout, `65ch` for paragraph text | Important |
| Spacing consistency | All spacing values in the design | Random values (`13px`, `22px`, `37px`) | 8px base rhythm: `8, 16, 24, 32, 48, 64, 96` | Important |
| Element spacing | `gap` or `margin` between items within a component | `0.25rem`-`0.5rem` (cramped within groups) | `0.5rem`-`1rem` between related items, `1.5rem`-`2rem` between groups | Minor |

### Color

| What to check | Where in code | AI default (problem) | Reference standard (target) | Severity |
|---|---|---|---|---|
| Number of accent colors | All color declarations excluding neutrals | 3-5+ distinct hues (visual noise) | 1-2 accent colors max (one primary, one semantic) | Critical |
| Neutral foundation | Background and surface colors | `#000000` pure black, or mismatched grays | `#0f0f0f`-`#1a1a1a` dark, `#fafafa`-`#ffffff` light | Important |
| Text contrast | Text color values | Low contrast or single opacity for all text | 3 levels: primary (full), secondary (`0.6` opacity), tertiary (`0.4`) | Important |
| Social proof logo treatment | `filter`, `opacity` on partner/client logos | Full color, full saturation (visual NASCAR) | `grayscale(100%)` with `opacity: 0.5`-`0.7` | Important |
| Dark mode support | CSS custom properties, `prefers-color-scheme`, `dark:` classes | No dark mode at all | Full dark mode with considered alternate palette | Minor |
| Color relationships | Overall palette coherence | Colors that don't relate (random hue picks) | Colors derived from one or two hue families | Important |

### Layout

| What to check | Where in code | AI default (problem) | Reference standard (target) | Severity |
|---|---|---|---|---|
| Section rhythm | Background alternation between sections | All sections same background, no visual pacing | Alternating full-width bg / contained content | Critical |
| Hero structure | Hero section layout and content | Small heading, short section, weak CTA | Full-width bg, large heading, dual CTA, social proof below | Important |
| Grid consistency | Grid column/row patterns | Varying grid structures between similar sections | Consistent grid: 3-col features, 3-col pricing, responsive to 1-col | Important |
| Content width constraint | `max-width` on headings and paragraphs | No constraint, text runs edge to edge | Headings: `max-width: 800px`, paragraphs: `max-width: 65ch` | Important |
| Responsive behavior | Media queries, responsive utilities | No responsive behavior or bolted-on breakpoints | Mobile-first with considered breakpoints at 640/768/1024/1280 | Important |

### Depth

| What to check | Where in code | AI default (problem) | Reference standard (target) | Severity |
|---|---|---|---|---|
| Shadow weight | `box-shadow` values | `0 4px 20px rgba(0,0,0,0.3)` (heavy, dated) | `0 1px 3px rgba(0,0,0,0.08)` resting, `0 4px 12px rgba(0,0,0,0.1)` hover | Critical |
| Border radius consistency | `border-radius` values across components | Mixed: `999px` on buttons, `4px` on cards, `12px` on inputs | Consistent: `6px`-`8px` buttons, `8px`-`12px` cards | Critical |
| Border treatment | `border` on cards and containers | No borders (relies on shadow alone) or `2px` borders (too heavy) | `1px solid rgba(0,0,0,0.08)` (barely there structure) | Important |
| Button radius | `border-radius` on buttons | `999px` / `9999px` (pill shape) | `6px` to `8px` (subtle rounding) | Important |

### Motion

| What to check | Where in code | AI default (problem) | Reference standard (target) | Severity |
|---|---|---|---|---|
| Hover states present | `:hover` styles on interactive elements | No hover states at all — elements look static | Every interactive element has a hover state | Critical |
| Transition properties | `transition` declarations | None (snap changes) or `all 0.3s` (lazy blanket) | Specific properties: `background-color 150ms ease`, `transform 200ms ease` | Important |
| Animation restraint | Keyframe animations, scale transforms | Bounce effects, large scale transforms, slide-ins | `translateY(-2px)` on hover, subtle opacity changes | Important |
| Focus states | `:focus`, `:focus-visible` styles | No custom focus states (default browser outline only) | Styled focus ring that matches the design system | Minor |

---

## How to audit

1. **Read, don't guess.** Use the Agent tool with `subagent_type=Explore` to find and read the actual CSS/Tailwind values. Open the component files. Look at the stylesheets. Check computed values if needed.

2. **Work dimension by dimension.** Walk each row in the checklist above and record the actual value vs. the reference standard. Don't skip dimensions — AI defaults tend to cluster, and the cumulative effect matters more than any single item.

3. **Note the critical items first.** Critical-severity items are the tells that make a design look AI-generated. These are the first priority for elevation.

4. **Score each axis.** After completing the checklist, score craft, consistency, and confidence. The scores should reflect the overall picture, not just a count of items — a design with two critical defaults scores lower than one with five minor issues.

5. **Identify the pattern.** Most AI-generated designs have a specific cluster of defaults. Common clusters:
   - **The "safe" design:** everything medium, no strong choices, consistent but bland
   - **The "enthusiastic" design:** too many colors, heavy shadows, exclamation marks, pill buttons
   - **The "functional" design:** decent structure but no depth, no motion, no personality
   - **The "kitchen sink" design:** every section uses a different treatment, no system

   Naming the cluster helps focus the elevation work.
