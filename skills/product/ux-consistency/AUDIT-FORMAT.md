# AUDIT-FORMAT.md

The deliverable format for a consistency audit. Four sections, each serving a different audience need: inventory for completeness, map for analysis, recommendations for action, and pattern proposals for prevention.

## 1. Concept inventory

A table of every recurring concept in the product, its canonical name, and every variant found.

| Concept | Canonical name | Variants found | Locations |
|---------|---------------|----------------|-----------|
| The user's group of people | Workspace | "Team" (settings), "Space" (onboarding), "Org" (API errors) | `Nav.tsx:12`, `Settings.tsx:45`, `Onboarding.tsx:8`, `api/errors.ts:23` |
| Removing an item | Delete | "Remove" (contacts), "Trash" (files), "Archive" (messages) | `ContactList.tsx:89`, `FileManager.tsx:34`, `Inbox.tsx:112` |

### How to build it

- One row per concept, not per instance. The concept is the abstract thing; the variants are how it appears.
- The canonical name is your recommendation for what the standard should be. Pick the one that's most common, most accurate, or most in-voice — in that priority order, unless the user overrides.
- List every variant with its location. Include file path and line number or component name — enough to find it.
- Include non-obvious concepts. Entity names are obvious; state treatments, action labels, and confirmation patterns are less obvious but equally important.

### When to skip

If the product is small enough that the inventory fits in memory from the exploration pass, you can present the diff map directly. For products with more than ten screens, the inventory pays for itself by catching concepts you'd otherwise miss during diffing.

---

## 2. Inconsistency map

Grouped by dimension (see CONSISTENCY-DIMENSIONS.md), each entry describing one inconsistency.

### Entry format

```
### [Dimension]: [Brief description]

**What:** [The concept and what's inconsistent about it]
**Where:**
- [Location 1]: [Treatment/name/behavior in this location]
- [Location 2]: [Treatment/name/behavior in this location]
- [Location 3]: [Treatment/name/behavior in this location]
**Type:** [Intentional | Accidental | Drift | Conflict] (see CLASSIFICATION.md)
**Canonical recommendation:** [Which variant to standardize on, and why]
**Fix effort:** [Trivial | Moderate | Significant]
```

### Example entry

```
### Interaction: Delete confirmation pattern

**What:** Destructive delete action uses three different confirmation patterns for entities of similar weight.
**Where:**
- `ContactList.tsx:89` — undo-toast, 5-second window
- `TaskList.tsx:134` — confirmation modal ("Are you sure?")
- `NotesList.tsx:67` — no confirmation, immediate deletion
**Type:** Accidental
**Canonical recommendation:** Undo-toast for all three. These are lightweight, recoverable entities. Modal confirmation adds friction without matching the risk level. No confirmation is dangerous.
**Fix effort:** Moderate — NotesList needs undo infrastructure; TaskList needs modal removal and toast addition.
```

### Grouping

Group entries under dimension headings: Naming, Interaction, Visual, Copy, State, Flow, Terminology. Within each dimension, order by severity (highest first). If a dimension has no findings, omit it — don't include an empty section.

---

## 3. Priority recommendations

The 5-10 highest-impact standardizations, ranked. This is the "if you only fix ten things" list.

For each recommendation:

- **What to standardize** — one sentence
- **Why it matters** — the user impact, not the aesthetic impact
- **Instances affected** — count and locations
- **Fix effort** — trivial, moderate, or significant
- **Dependencies** — does this need to be done before or after another recommendation?

### How to prioritize

Impact and effort, weighted toward impact. A naming inconsistency that touches every screen is higher priority than a visual inconsistency in a rarely-visited settings page, even if the visual fix is harder.

Rough priority order:
1. **Naming inconsistencies** visible across the product — high impact, usually trivial effort
2. **Interaction inconsistencies** for destructive actions — high impact, user safety involved
3. **Copy inconsistencies** in high-traffic areas — medium-high impact, trivial effort
4. **State inconsistencies** that affect perceived reliability — high impact, moderate effort
5. **Flow inconsistencies** between analogous tasks — medium impact, significant effort
6. **Visual inconsistencies** — medium impact, variable effort
7. **Terminology inconsistencies** — medium impact for users, high impact for developer experience

This ordering is a starting point. The user's context may shift priorities — if they're about to ship docs, terminology rises. If they just had a user report confusion about a destructive action, interaction rises.

---

## 4. Pattern proposals

New patterns to add to CONTEXT.md to prevent recurrence. Each proposal follows the format from improve-product-feel/CONTEXT-FORMAT.md.

### Proposal format

```
### [Pattern name]

**Pattern:** [What the standard is]
**Applies to:** [Which contexts]
**Exceptions:** [When variation is justified]
**Rationale:** [One sentence on why this is the canonical choice]
```

### Example proposal

```
### Destructive action confirmation

**Pattern:** Destructive actions on recoverable entities use a 5-second undo-toast. Destructive actions on irrecoverable entities (account deletion, payment actions) use a confirmation modal with the entity name repeated.
**Applies to:** All delete, remove, archive, disconnect, and revoke actions.
**Exceptions:** Bulk actions on more than 10 items use a confirmation modal with a count, even if individual items would use undo-toast. The volume makes undo impractical.
**Rationale:** Undo-toast keeps the common case fast; modal reserves interruption for genuine risk.
```

### How to write useful proposals

- Be specific enough to apply. "Be consistent with buttons" is not a pattern. "Primary actions use 'Save' for persisting changes and 'Create' for making new entities" is.
- Include exceptions. A pattern with no exceptions will be broken the first time an edge case appears, and then the drift starts again.
- Keep each proposal to one concept. Don't bundle "button labels and confirmation patterns and loading states" into one mega-pattern.
- Reference the inconsistency that prompted the proposal. This creates a paper trail — the team can see why the pattern was established.
- Write in the same format as existing CONTEXT.md entries (see improve-product-feel/CONTEXT-FORMAT.md) so they can be copied directly into the file.
