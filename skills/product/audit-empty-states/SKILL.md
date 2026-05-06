---
name: audit-empty-states
description: Find every empty state in a product and rewrite them from "nothing here" into actionable moments. Use when the user wants to audit empty states, improve blank screens, make zero-data states useful, or ensure every empty view tells the user what to do next.
---

# Audit Empty States

Find every empty state in the product and close the gap between "nothing here" and "here's what to do."

An empty state is the first thing a new user sees and the thing a returning user hits after clearing, filtering, or deleting. Both deserve more than a blank screen or a shrug emoji. The goal is not decoration — it's orientation. Every empty state should answer: what is this, why is it empty, and what's the next move.

## Types of empty state (see TAXONOMY.md)

Five types, each with different needs:

- **First-run** — user has never created anything here. This is a welcome, not a void.
- **Cleared** — user deleted or archived everything. This is confirmation, not a dead end.
- **Filtered** — search or filters returned nothing. This is feedback about the query, not the product.
- **Error** — something failed and there's nothing to show. This is a recovery prompt, not a shrug.
- **Permissions** — user can see the container but not the contents. This is an explanation, not a blank.

If you aren't sure which type you're looking at, TAXONOMY.md has the full decision criteria.

## Process

### 1. Inventory

Use the Agent tool with `subagent_type=Explore` to find every empty state in the codebase. Look for:

- Conditional renders checking zero-length arrays, null data, empty responses
- Components named `Empty`, `EmptyState`, `NoData`, `Placeholder`, `ZeroState`
- Strings like "No results", "Nothing here", "Get started", "No items found"
- Loading/error boundaries that render fallback UI

For each one, record: **where** (screen, component), **type** (from TAXONOMY.md), and **current treatment** (exact copy, layout, actions offered).

### 2. Score

Rate each empty state on three axes (see SCORING-RUBRIC.md for full criteria):

- **Orientation** (1-5) — does the user know what this area is for?
- **Next action** (1-5) — does it offer what to do?
- **Voice** (1-5) — does it sound like the product?

Anything scoring below 3 on any axis is a candidate.

### 3. Present candidates

For each candidate, present:

- **Where** — screen and component
- **Type** — which kind of empty state
- **Current** — exact current copy and treatment
- **Problem** — what the user experiences (dead end, confusion, tone mismatch)
- **Proposed** — what it should say and offer instead

Use the product's voice from CONTEXT.md if one exists. If not, match the tone of the product's best existing copy. See COPY-PATTERNS.md for how to write good empty state copy by type.

Do NOT write final implementation code yet. Present the list and ask: "Which of these should we finalize?"

### 4. Finalize

For each selected candidate, write:

- **Headline** — the main line the user reads (short, in voice)
- **Body** — one sentence of context if needed (often not needed)
- **Action** — the button or link label and where it goes
- **Illustration** — whether it needs a visual, and what it communicates (a brief, not a spec)

See COPY-PATTERNS.md for the anatomy of each component and common pitfalls. Then implement the changes in the codebase.

## Rules

- Every empty state must offer a next action unless the correct response is genuinely "wait" (e.g. a queue that fills asynchronously).
- Never use "Oops", "Uh oh", or exclamation marks unless the product voice explicitly calls for it.
- Filtered empty states must reference what the user searched or filtered for.
- First-run empty states must never make the user feel like something is broken.
- If the same empty state component is reused across multiple screens with different contexts, flag it — generic reuse is the number one source of "nothing here" copy.
