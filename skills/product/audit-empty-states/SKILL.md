---
name: audit-empty-states
description: Find every empty state in a product and rewrite them from "nothing here" into actionable moments. Use when the user wants to audit empty states, improve blank screens, make zero-data states useful, or ensure every empty view tells the user what to do next.
---

# Audit Empty States

Find every empty state in the product and close the gap between "nothing here" and "here's what to do."

An empty state is the first thing a new user sees and the thing a returning user hits after clearing, filtering, or deleting. Both deserve more than a blank screen or a shrug emoji. The goal is not decoration — it's orientation. Every empty state should answer: what is this, why is it empty, and what's the next move.

## What counts as an empty state

- **First-run empty** — the user has never created anything here. This is a welcome, not a void.
- **Cleared empty** — the user deleted or archived everything. This is confirmation the action worked, not a dead end.
- **Filtered empty** — the user applied filters or search that returned nothing. This is feedback about the query, not about the product.
- **Error empty** — something failed and there's nothing to show. This is a recovery prompt, not a shrug.
- **Permissions empty** — the user can see the container but not the contents. This is an explanation, not a blank.

Each type has different needs. A first-run empty should encourage; a filtered empty should help refine; a permissions empty should explain without making the user feel locked out.

## Process

### 1. Inventory

Use the Agent tool with `subagent_type=Explore` to find every empty state in the codebase. Look for:

- Conditional renders that check for zero-length arrays, null data, or empty responses
- Components with names like `Empty`, `EmptyState`, `NoData`, `Placeholder`, `ZeroState`
- Strings like "No results", "Nothing here", "Get started", "No items found"
- Loading/error boundaries that render fallback UI

For each one, record:
- **Where** — which screen, which component
- **Type** — first-run, cleared, filtered, error, or permissions
- **Current treatment** — what the user sees right now (exact copy, layout, actions offered)

### 2. Score

Rate each empty state on three axes:

- **Orientation** — does the user know what this area is for? (1 = no context, 5 = immediately clear)
- **Next action** — does it offer what to do? (1 = dead end, 5 = obvious next step with a button/link)
- **Voice** — does it sound like the product? (1 = generic/developer-speak, 5 = fully in voice)

Anything scoring below 3 on any axis is a candidate.

### 3. Present candidates

For each candidate, present:

- **Where** — screen and component
- **Type** — which kind of empty state
- **Current** — exact current copy and treatment
- **Problem** — what the user experiences (dead end, confusion, tone mismatch)
- **Proposed** — what it should say and offer instead

Use the product's voice from CONTEXT.md if one exists. If not, match the tone of the product's best existing copy.

Do NOT write final implementation code yet. Present the list and ask: "Which of these should we finalize?"

### 4. Finalize

For each selected candidate, write:

- **Headline** — the main line the user reads (short, in voice)
- **Body** — one sentence of context if needed (often not needed)
- **Action** — the button or link label and where it goes
- **Illustration** — whether the empty state needs a visual, and if so, what it communicates (not a spec — a brief for whoever makes it)

Then implement the changes in the codebase.

## Rules

- Every empty state must offer a next action unless the correct response is genuinely "wait" (e.g. a queue that will fill asynchronously).
- Never use "Oops", "Uh oh", or exclamation marks in empty states unless the product voice explicitly calls for it.
- Filtered empty states should reference what the user searched/filtered for, not just say "no results."
- First-run empty states should never make the user feel like something is broken.
- If the same empty state component is reused across multiple screens with different contexts, flag it — generic empty states are the number one source of "nothing here" copy.
