---
name: onboarding-friction
description: Walk the first-run experience as a new user and map every moment where the product assumes knowledge the user doesn't have. Use when the user wants to improve onboarding, reduce time-to-value, audit the new user experience, find where first-time users get stuck, or lower activation friction.
---

# Onboarding Friction

Walk the product as if you've never seen it before. Map every moment where the product assumes you already know something — a concept, a workflow, a convention, a vocabulary term — and doesn't teach it.

This skill is not about building an onboarding wizard or adding tooltips. It's about finding the assumptions that make a first-run user feel stupid, and deciding which ones the product should resolve and which ones it should teach.

## The core question

At every step of first use, ask: **"What would I need to already know for this to make sense?"**

If the answer is "nothing — it's self-evident," the step is fine. If the answer is a concept, a term, a convention, or a prior action the user hasn't taken, that's a friction point. See ASSUMPTION-TYPES.md for the full taxonomy of hidden assumptions.

## Process

### 1. Define the activation path

Establish with the user:

- **Who is the new user?** — What do they know coming in? (Technical? Non-technical? Familiar with the category? Coming from a competitor?)
- **What is the activation moment?** — The first moment the user gets real value. Not "completed signup" — the moment they think "this is useful."
- **What's the shortest path from signup to activation?**

If the user can't articulate the activation moment, that's the first finding.

### 2. Walk the path

Use the Agent tool with `subagent_type=Explore` to walk the actual first-run experience step by step. Start from the very beginning — the signup page, the first screen after auth, the first empty state.

At each step, apply the core question and check every assumption type in ASSUMPTION-TYPES.md: vocabulary, workflow, mental model, prior-action, and technical. Record what the user sees, what the product assumes, what the user is likely thinking, and what's missing.

### 3. Map the friction

Present findings as an ordered friction map along the activation path. See FRICTION-MAP-FORMAT.md for the full format spec — each entry records the step, assumption, evidence, impact, and severity.

Order entries by where they occur on the path, not by severity. The friction map is a walk-through, not a priority list.

### 4. Recommend

For each friction point, recommend one of four remedies: **remove**, **teach inline**, **reorder**, or **accept**. See REMEDIES.md for when to use each, when not to, and the hierarchy: remove > teach > reorder > accept.

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
- Don't confuse "the user might not know X" with "the user doesn't know X." Consider the target user profile.
- Configuration that's required before value is a red flag. Ask: can the product start with a sensible default and let the user change it later?
- The signup form itself is part of onboarding. Every field is friction. Each one should earn its place.
