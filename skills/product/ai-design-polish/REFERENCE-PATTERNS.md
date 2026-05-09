# REFERENCE-PATTERNS.md

The design language extracted from four premium SaaS templates (ZipPay, MetaFi, Scalar, Charter). This is the "what good looks like" document — the specific values, patterns, and principles that separate premium from generic.

These are not aspirational ideals. They are measured values from real designs that users perceive as high-quality. When auditing an AI-generated design, compare against these standards.

---

## Typography

### The reference standard

**Scale:** Three to four sizes used consistently. Not eight sizes with no logic.

- Display/hero: `clamp(1.5rem, 5vw, 3.5rem)` — large, confident, responsive
- Section heading: `clamp(1.25rem, 3vw, 2rem)` — clear hierarchy step down
- Body/UI: `1rem` (16px) — readable, not inflated
- Small/caption: `0.875rem` (14px) — metadata, labels, secondary info

**Weight:** Two weights maximum. Bold (600-700) for headings. Regular (400) for body. Medium (500) occasionally for UI labels. Never light (300) for body — it looks fragile at small sizes.

**Line height:**
- Headings: `1.1` to `1.2` — tight, creates visual mass
- Body: `1.5` to `1.6` — generous, readable
- Never `1.0` (cramped) or `2.0` (loose, academic)

**Letter spacing:**
- Headings: `-0.01em` to `-0.02em` — subtle tightening at large sizes
- Body: `0` (default) — don't touch it
- Uppercase labels: `0.05em` to `0.1em` — open it up for legibility

**Family:** Sans-serif. Inter, Geist, Satoshi, or system stack. Avoid decorative fonts for UI. One typeface for the entire design — mixing families is an advanced move that AI rarely pulls off well.

### What it communicates

Large display headings say "we have something worth saying." Restrained scale says "we made deliberate choices." Tight heading line-height creates visual density that reads as confident. Generous body line-height says "read comfortably."

### Why AI misses it

AI tends to produce headings that are too small (doesn't commit to display scale), uses too many font sizes (every element gets a slightly different size), and sets line-height uniformly. The result is typographically flat — nothing commands the eye, and the hierarchy is muddy.

---

## Spacing

### The reference standard

**Base rhythm:** 8px (`0.5rem`). All spacing values are multiples: 8, 16, 24, 32, 48, 64, 96.

**Section gaps:** `4rem` to `6rem` (64-96px) between major sections. Content breathes. Sections have clear start and end — you can draw a line between them without looking at the content.

**Component internal padding:** `1.5rem` to `2rem` (24-32px) inside cards, panels, containers. Not so tight that content touches the edges. Not so loose that the container feels empty.

**Container max-width:** `1200px` for content, `1400px` for full layouts. Never truly full-width for text — `65ch` max for paragraph text. Hero sections can go wider.

**Element spacing (within components):**
- Between heading and body text: `0.5rem` to `1rem`
- Between body text and CTA: `1.5rem` to `2rem`
- Between items in a list or grid: `1rem` to `2rem`
- Between icon and label: `0.5rem` to `0.75rem`

### What it communicates

Generous whitespace communicates confidence — "we don't need to cram everything in." Clear section separation communicates structure — "we organized this for you." Consistent rhythm communicates care — "every measurement was deliberate."

### Why AI misses it

AI typically produces spacing that is too tight and inconsistent. Sections run together. Cards have cramped internal padding. Gaps between elements vary randomly. The result feels cluttered and anxious — like the design is worried about wasting space.

---

## Color

### The reference standard

**Palette construction:** 1-2 accent colors maximum. One primary accent (brand), one secondary accent (semantic — success, emphasis). Everything else is neutrals.

**Neutral foundations:**
- Dark mode base: `#0f0f0f` to `#1a1a1a` — rich black, not pure `#000000`
- Dark mode surface: `#1a1a1a` to `#262626` — one step lighter for cards/panels
- Dark mode borders: `rgba(255,255,255,0.08)` to `rgba(255,255,255,0.12)` — barely there
- Light mode base: `#ffffff` to `#fafafa`
- Light mode surface: `#f5f5f5` to `#f0f0f0`
- Light mode borders: `rgba(0,0,0,0.08)` to `rgba(0,0,0,0.12)`

**Text colors:**
- Primary text: `#ffffff` (dark) / `#0f0f0f` (light) — high contrast
- Secondary text: `rgba(255,255,255,0.6)` (dark) / `rgba(0,0,0,0.6)` (light) — clearly subordinate
- Tertiary text: `rgba(255,255,255,0.4)` (dark) / `rgba(0,0,0,0.4)` (light) — metadata, timestamps

**Accent usage:**
- CTAs and primary actions
- Active/selected states
- Key data points or metrics
- Never on large background areas — accent is for punctuation, not paint

**Social proof logos:** Desaturated or grayscale. `filter: grayscale(100%)` with `opacity: 0.5` to `0.7`. The logos should not compete with the product's own brand. On hover, they can restore to full color.

**Dark mode:** Support it. Use CSS custom properties or Tailwind's `dark:` prefix. Dark mode is not "invert everything" — it's a considered alternate palette where backgrounds darken, surfaces use subtle elevation differences, and text uses white at varying opacities.

### What it communicates

A restrained palette communicates sophistication — "we chose these colors deliberately." High contrast communicates confidence — "read this clearly." Desaturated social proof communicates "these logos serve us, we don't serve them." Dark mode support communicates "we thought about how you actually use this."

### Why AI misses it

AI reaches for too many colors, or colors that don't relate to each other (a blue button, a green badge, an orange alert, a purple tag — all on one page). It uses pure black for dark mode. It leaves social proof logos at full saturation and color, creating a visual NASCAR effect. It rarely implements dark mode, or implements it as a crude inversion.

---

## Layout

### The reference standard

**Hero pattern:** Full-width background (color, gradient, or subtle pattern), content centered at `max-width: 1200px`. Large heading, supporting paragraph, dual CTA. Social proof strip below.

**Section rhythm:** Alternation between full-width backgrounds and contained content blocks creates visual pacing. Pattern: full-width hero, contained features, full-width social proof, contained details, full-width CTA.

**Grid patterns:**
- Features: 3-column grid on desktop, single column on mobile. `grid-template-columns: repeat(3, 1fr)` with `gap: 2rem`
- Pricing: 3-column with center card elevated. Equal height columns.
- Testimonials: 2 or 3 column grid, or single-column carousel.

**Responsive breakpoints:**
- Mobile-first base styles
- `640px` (sm): minor adjustments
- `768px` (md): two-column layouts
- `1024px` (lg): three-column layouts, full navigation
- `1280px` (xl): max-width containers kick in

**Content width control:**
- Headings: up to `max-width: 800px`, centered
- Paragraphs: up to `max-width: 65ch` (roughly 600px), centered or left-aligned
- Full layout: `max-width: 1200px` with `margin: 0 auto`

### What it communicates

The alternation between full-width and contained creates breathing room and visual rhythm — "we have pacing." Constrained content width says "we want you to read this comfortably." A clear hero-to-content-to-CTA flow says "we organized this journey for you."

### Why AI misses it

AI tends to produce designs with no section rhythm — everything is the same width, the same background, running together without visual pause. Content often runs full-width with no max-width constraint, making lines too long to read comfortably. Responsive behavior is often missing or bolted on.

---

## Depth

### The reference standard

**Card shadows:**
- Resting: `0 1px 3px rgba(0,0,0,0.08)` or no shadow with a `1px` border
- Hover: `0 4px 12px rgba(0,0,0,0.1)` — subtle elevation change
- Dark mode: shadows are nearly invisible. Use `1px` borders at `rgba(255,255,255,0.08)` to `rgba(255,255,255,0.12)` instead.

**Border treatments:**
- Cards: `1px solid rgba(0,0,0,0.08)` (light) or `1px solid rgba(255,255,255,0.08)` (dark)
- Dividers: same values, used sparingly — whitespace separates better than lines
- Input fields: `1px solid rgba(0,0,0,0.15)`, slightly more visible than card borders

**Border radius:**
- Cards and containers: `8px` to `12px` — subtle rounding
- Buttons: `6px` to `8px` — not pills (`999px`), not sharp (`0`)
- Input fields: match button radius
- Avatars and icons: `50%` (circle) or `8px` (rounded square)
- Consistency: one radius value for buttons, one for cards, used everywhere

**Layering:**
- Background < surface < elevated surface. Each step is one shade lighter (dark mode) or uses a subtle shadow (light mode).
- Modals and overlays: `backdrop-filter: blur(8px)` with a semi-transparent background
- Sticky headers: subtle bottom border or shadow to separate from scrolling content

### What it communicates

Subtle depth says "we use elevation to communicate hierarchy, not to show off." Consistent border radius says "we have a system." Light borders say "structure without weight."

### Why AI misses it

AI reaches for heavy drop shadows: `0 4px 20px rgba(0,0,0,0.3)` or worse. It uses inconsistent border radii — pills on buttons, sharp on cards, rounded on inputs. The result is visually noisy: shadows compete with content, and the lack of a consistent radius system makes the design feel assembled from different kits.

---

## Motion

### The reference standard

**Hover states (buttons):**
```css
transition: background-color 150ms ease, box-shadow 150ms ease;
/* On hover: darken by 8-10%, or add subtle shadow */
```

**Hover states (cards):**
```css
transition: transform 200ms ease, box-shadow 200ms ease;
/* On hover: translateY(-2px), shadow increases one level */
```

**Theme toggle:**
```css
transition: background-color 300ms ease, color 300ms ease;
/* All color properties transition smoothly */
```

**What to animate:**
- `background-color` — buttons, interactive elements
- `box-shadow` — cards, panels on hover
- `transform` — subtle translateY on hover, scale on press
- `opacity` — fade in/out for appearing/disappearing elements
- `border-color` — input focus states

**What NOT to animate:**
- `width`, `height` — causes layout reflow, feels janky
- `margin`, `padding` — same problem
- Large `transform: scale()` — more than `1.02` feels cartoonish
- Everything simultaneously — if everything moves, nothing is emphasized

**Timing functions:**
- `ease` for most transitions — natural deceleration
- `ease-out` for elements entering — fast start, gentle stop
- `ease-in-out` for toggles and state changes
- Never `linear` for UI — it feels mechanical

**Duration:**
- Micro-interactions (hover, focus): `150ms` to `200ms`
- State changes (toggle, expand): `200ms` to `300ms`
- Page transitions: `300ms` to `500ms`
- Never above `500ms` for UI — it feels sluggish

### What it communicates

Subtle hover states say "this is interactive." Smooth transitions say "we care about the journey between states, not just the endpoints." Restrained animation says "motion serves function, not decoration."

### Why AI misses it

AI frequently omits hover states and transitions entirely — elements snap between states with no animation. When AI does add animation, it tends toward the flashy: bouncing, scaling, sliding in from offscreen. The result is either lifeless (no motion) or distracting (too much motion). Premium templates hit the middle: everything interactive has a hover state, and every state change transitions, but nothing calls attention to the animation itself.
