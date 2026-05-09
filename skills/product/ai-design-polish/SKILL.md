---
name: ai-design-polish
description: Elevate AI-generated designs from functional-but-generic to premium. Use when an AI has produced a design, page, or component that works but feels ho-hum, when you want to close the gap between "generated" and "crafted," or when a landing page, dashboard, or marketing site needs to feel like a premium SaaS template instead of like a default.
---

# AI Design Polish

Take an AI-generated design that works but feels generic and elevate it to the craft level of premium SaaS templates. The reference bar is the quality of designs like ZipPay, MetaFi, Scalar, and Charter — restrained palettes, generous spacing, confident typography, subtle depth, specific copy.

AI produces functional designs. The defaults it reaches for — heavy shadows, too many colors, generic copy, tight spacing, pill buttons, inconsistent hierarchy — are individually small but collectively unmistakable. A design-conscious user clocks "this was generated" within seconds. The tells are consistent and fixable.

This skill is NOT for redesigning from scratch. It's for taking what AI produced, identifying the specific defaults dragging the quality down, and systematically replacing them with the considered alternatives that premium templates use.

## Process

### 1. Audit

Use the Agent tool with `subagent_type=Explore` to read the actual code — HTML, CSS, Tailwind classes, component files, stylesheets. Don't guess at values from a screenshot or description. Read the real `font-size`, `padding`, `box-shadow`, `border-radius`, `color`, and `gap` values. Read the actual copy.

Walk the design and score it against the reference patterns in REFERENCE-PATTERNS.md across six dimensions: typography, spacing, color, layout, depth, and motion.

### 2. Diagnose

Identify the specific AI defaults dragging the design down. Reference AUDIT-CHECKLIST.md for the full list of what to check in each dimension. For each default found, note:

- **What it is now** — the specific CSS value, copy string, or pattern in the code
- **What the reference designs do instead** — the specific value from REFERENCE-PATTERNS.md
- **Severity** — critical (makes it look AI-generated), important (noticeable to design-conscious users), or minor (polish)

Score the design on three axes (see AUDIT-CHECKLIST.md):

- **Craft** (1-5) — do the visual details show considered choices or AI defaults?
- **Consistency** (1-5) — do the design decisions form a coherent system or a grab bag?
- **Confidence** (1-5) — does the design commit to its choices or hedge with generic safe options?

### 3. Present findings

Present the diagnosis to the user, organized from critical to minor. For each finding:

- **What** — the specific default (e.g. "box-shadow: 0 4px 20px rgba(0,0,0,0.3)")
- **Where** — which component or section
- **Why it matters** — what it communicates (e.g. "heavy shadows read as dated/unrefined")
- **Reference standard** — what premium templates use instead (e.g. "0 2px 8px rgba(0,0,0,0.08)")
- **Severity** — critical, important, or minor

Ask the user: "Which of these should I fix? All of them, or a subset?"

### 4. Elevate

Apply the fixes. Reference ELEVATION-PATTERNS.md for the standard upgrades — each pattern includes the specific code change with before/after values. For copy upgrades, reference COPY-UPGRADE.md.

Make the actual code changes. Don't just describe what should change — change it.

### 5. Verify

Re-score the design on the three axes (craft, consistency, confidence) after changes. The design should score higher on every axis. If any axis didn't improve, identify what was missed and whether additional changes are warranted.

Report the before/after scores and the specific changes made.

## Rules

- Always read the actual code. Never assess from descriptions alone.
- Preserve the design's intent and functionality. Elevation is about execution quality, not creative direction.
- Don't overcorrect. The goal is "premium template," not "award-winning art piece." Restraint is the point.
- Fix the highest-severity items first. Five critical fixes matter more than twenty minor tweaks.
- Copy changes require the most judgment. See COPY-UPGRADE.md, but always respect the product's domain and audience.
- When in doubt, subtract. Remove the extra color, the heavy shadow, the exclamation mark. Premium templates are defined more by what they leave out than what they add.
