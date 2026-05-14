---
name: ux-consistency
description: Audit a product for consistency — finding where the same concept is named, styled, or handled differently across screens, components, and flows. Use when the product feels disjointed, when multiple developers have contributed without coordination, after rapid prototyping, when onboarding a designer, or when users report confusion about how things work.
---

# UX Consistency

Find the places where the same concept is handled differently — different names, different treatments, different patterns — and standardize them. Users don't notice consistency; they notice inconsistency. One inconsistent touchpoint makes them wonder if they're in the right place. Five makes them think the product is unreliable.

This is not about enforcing a design system (that's tooling). It's about finding the inconsistencies that exist despite a design system — or especially when there isn't one. The places where the same action has different labels, the same state has different treatments, the same pattern is solved three different ways.

## Process

### 1. Inventory

Use the Agent tool with `subagent_type=Explore` to walk the codebase. Build a map of recurring concepts:

- **Nouns** — the entities the user interacts with. What are they called in the nav, in page titles, in breadcrumbs, in button labels, in error messages, in docs, in code?
- **Verbs** — the actions the user takes. Create, delete, edit, save, submit, cancel, remove, archive. What labels are used for each action, and where?
- **States** — loading, empty, error, success, disabled, pending. How is each state handled across screens and components?

For each concept, record every name, label, and treatment found. Note the file and component where each instance lives. The goal is a complete picture before any judgment — don't fix anything yet.

See CONSISTENCY-DIMENSIONS.md for the seven dimensions to check and specific grep patterns for each.

### 2. Diff

For each concept in the inventory, compare every instance. Where does the name differ? Where does the treatment differ? Where does the behavior differ?

Focus on the gaps that would confuse a user who encounters both versions in the same session. A different label in the settings page vs. the nav bar is high-impact — the user sees both. A different variable name in the code vs. the UI label is lower-impact but still worth cataloging for developer onboarding.

Reference CONSISTENCY-DIMENSIONS.md for what to compare in each dimension.

### 3. Classify

For each inconsistency, determine its type. Not all inconsistency is accidental — some is intentional and justified. The distinction matters because the resolution is different.

See CLASSIFICATION.md for the four types: intentional variation, accidental variation, evolutionary drift, and precedent conflict. Each has a different resolution path.

### 4. Present

Group findings by severity. For each inconsistency:

- **What** — the concept and dimension (e.g. "the 'delete' action has three different confirmation patterns")
- **Where** — every location involved, with file paths
- **Type** — intentional, accidental, drift, or conflict (from CLASSIFICATION.md)
- **Canonical recommendation** — which variant should be the standard, and why
- **Fix effort** — trivial (copy change), moderate (component refactor), significant (flow redesign)

Use the deliverable format in AUDIT-FORMAT.md. Ask the user which inconsistencies to standardize before implementing anything.

### 5. Standardize

Implement the agreed-upon fixes. For each:

- Apply the canonical version everywhere
- If the pattern isn't documented in CONTEXT.md (see improve-product-feel/CONTEXT-FORMAT.md), add it — named patterns prevent recurrence
- If the inconsistency was intentional, add a note to CONTEXT.md explaining when the variation applies

See PREVENTION.md for how to keep consistency once achieved.
