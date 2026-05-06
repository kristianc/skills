---
name: onboarding-friction
description: Walk the first-run experience as a new user and map every moment where the product assumes knowledge the user doesn't have. Use when the user wants to improve onboarding, reduce time-to-value, audit the new user experience, find where first-time users get stuck, or lower activation friction.
---

# Onboarding Friction

Walk the product as if you've never seen it before. Map every moment where the product assumes you already know something — a concept, a workflow, a convention, a vocabulary term — and doesn't teach it.

This skill is not about building an onboarding wizard or adding tooltips. It's about finding the assumptions that make a first-run user feel stupid, and deciding which ones the product should resolve and which ones it should teach.

## The core question

At every step of first use, ask: **"What would I need to already know for this to make sense?"**

If the answer is "nothing — it's self-evident," the step is fine. If the answer is a concept, a term, a convention, or a prior action the user hasn't taken, that's a friction point.

## Process

### 1. Define the activation path

Before walking flows, establish with the user:

- **Who is the new user?** — What do they know coming in? (Technical? Non-technical? Familiar with the category? Coming from a competitor?)
- **What is the activation moment?** — The first moment the user gets real value. Not "completed signup" — the moment they think "this is useful." (Sent their first message, saw their first report, connected their first integration.)
- **What's the shortest path from signup to activation?**

If the user can't articulate the activation moment, that's the first finding.

### 2. Walk the path

Use the Agent tool with `subagent_type=Explore` to walk the actual first-run experience step by step. Start from the very beginning — the signup page, the first screen after auth, the first empty state.

At each step, record:

- **What the user sees** — screen, copy, controls, empty states
- **What the product assumes** — knowledge, vocabulary, mental model, prior actions
- **What the user is likely thinking** — "what do I do?", "what is this?", "where did my thing go?", "did that work?"
- **What's missing** — context, explanation, guidance, feedback, a path forward

Pay attention to:

- **Vocabulary the product uses without defining.** If the nav says "Workspaces" and the user doesn't know what a workspace is in this product, that's friction.
- **Empty states that don't teach.** The first time a user sees a list with no items, it should explain what goes here and how to add one.
- **Settings and configuration before value.** Anything that asks the user to configure before they've seen what the product does is asking for trust they haven't earned.
- **Implicit conventions.** Drag-to-reorder, right-click menus, keyboard shortcuts — anything the product relies on but doesn't surface.
- **Missing feedback.** Did the action work? Is something loading? Did it save? First-time users interpret silence as failure.
- **Jargon leaks.** Technical terms, internal naming, framework vocabulary that made it into the UI.

### 3. Map the friction

Present findings as an ordered list along the activation path. For each friction point:

- **Step** — where in the path this occurs (e.g. "first screen after signup", "creating first project", "inviting a teammate")
- **Assumption** — what the product assumes the user knows
- **Evidence** — what specifically in the UI reveals this assumption (copy, layout, missing guidance)
- **Impact** — what happens if the user doesn't have this knowledge (stuck, confused, makes wrong choice, abandons)
- **Severity** — blocking (can't proceed), confusing (proceeds but uncertain), or cosmetic (notices but unaffected)

### 4. Recommend

For each friction point, recommend one of:

- **Remove** — eliminate the step entirely. The best onboarding is no onboarding. If the product can infer, default, or defer, do that instead of asking.
- **Teach inline** — add context where the user is, when they need it. Not a tooltip library — a specific line of copy, an empty state that explains, a label that clarifies.
- **Reorder** — move this step to after the user has the context to understand it. Configuration after value, not before.
- **Accept** — this assumption is reasonable for the target user. Document why so future reviews don't re-flag it.

Do NOT recommend a guided tour, tooltip sequence, or onboarding modal unless the user specifically asks for one. These are almost always band-aids over a product that doesn't explain itself.

### 5. Implement

For selected recommendations, make the changes in the codebase. Prefer:

- Better copy over added UI
- Smarter defaults over configuration screens
- Inline context over separate help pages
- Progressive disclosure over upfront complexity

## Rules

- Walk the path yourself before making recommendations. Don't audit from the component tree — use the product.
- Every "teach inline" recommendation must include the actual copy, not "add some explanatory text."
- If the activation path takes more than 5 minutes for the target user, that's a finding in itself.
- Don't confuse "the user might not know X" with "the user doesn't know X." Consider the target user profile. A developer tool can assume the user knows what an API key is.
- Configuration that's required before value is a red flag. Ask: can the product start with a sensible default and let the user change it later?
- The signup form itself is part of onboarding. Every field is friction. Each one should earn its place.
