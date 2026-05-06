---
name: improve-product-feel
description: Find polish opportunities in a product, informed by the voice and patterns in CONTEXT.md and the decisions in docs/adr/. Use when the user wants to improve how a product feels, surface friction in existing flows, raise the craft level of touchpoints, or make a product feel more considered. Explicitly NOT for proposing new features.
---

# Improve Product Feel

Surface friction and propose polish opportunities — changes that turn perfunctory interactions into considered ones. The aim is craft, trust, and respect for the user's attention.

This skill is explicitly NOT for proposing new features. It is for raising the quality of what is already there. If a candidate's value comes from "we should also let users do X," it does not belong on the list.

## Glossary

Use these terms exactly in every suggestion. Consistent language is the point — don't drift into "UX," "experience," or vague words like "smooth," "clean," or "intuitive." Full definitions in LANGUAGE.md.

**Interaction** — a unit of user input and system response (click, hover, submit, load, error, success, return-after-absence).

**Touchpoint** — a specific moment within an interaction where craft is visible or absent. Empty state, loading state, error state, success state, transition, idle, recovery, first-run.

**Friction** — anything that costs the user time, attention, trust, or effort beyond what the task requires.

**Affordance** — what a control communicates about what it does and what state it is in.

**Feedback** — system response that tells the user what happened. Ideally immediate, specific, and in the product's voice.

**Default** — the unconsidered version of a touchpoint. The browser alert. The blank empty state. "An error occurred." The form that drops your input on validation failure.

**Crafted** — the considered version. Anticipates the user's situation, knows their likely next move, sounds like the product.

**Polish** — the gap between default and crafted.

## Key principles (see LANGUAGE.md for the full list)

- **Articulation test:** imagine pointing at the touchpoint and saying, in user terms, what it earns. "It loads" doesn't count. "Confirms within 100ms so the user doesn't tap twice" does. If you can't articulate it, the touchpoint is a default and a polish candidate.
- **Subtraction beats addition.** The best polish often removes — a confirmation, a wait, a redundant step — rather than adds animation, copy, or controls.
- **Voice is part of UX.** Button labels, error messages, empty-state lines and toasts are interactions. Cheap copy makes a product feel cheap, no matter how good the layout is.
- **Feel is cumulative.** Each touchpoint is small; the impression is the sum. Five defaults in a row reads as "this product doesn't care."
- **One bespoke treatment is a one-off. Two is a pattern** — worth naming in CONTEXT.md so the rest of the product can stay consistent with it.

This skill is informed by the product's voice and established patterns. CONTEXT.md gives names to the patterns and voice; ADRs record UX decisions the skill should not re-litigate.

## Process

### 1. Explore

Read CONTEXT.md and any ADRs covering the area you're touching first.

Then use the Agent tool with `subagent_type=Explore` to walk real flows in the product. Don't follow rigid heuristics — go through tasks the way a user would, and note where you feel friction:

- Where do touchpoints fall back to defaults — generic errors, blank empty states, browser confirms, "Loading…", "Something went wrong"?
- Where does the product abandon the user mid-flow (no undo, no confirmation that the action landed, no path back from an error)?
- Where does the copy slip out of the product's voice — developer-speak, passive aggression, forced cheerfulness, exclamation marks the brand doesn't earn?
- Where do affordances mislead — buttons that don't look pressable, disabled controls with no explanation, destructive actions that look identical to safe ones?
- Which interactions wait when they could be optimistic? Which confirm when they could undo? Which interrupt when they could surface quietly?
- Where is the same touchpoint solved differently in two places, with no reason for the inconsistency?

Apply the articulation test to anything that smells like a default: can you describe in user terms what it currently earns? If you can't, it is a polish candidate.

### 2. Present candidates

Present a numbered list of polish opportunities. For each candidate:

- **Where** — which screens, flows, or touchpoints are involved
- **Friction** — what the user experiences right now (be specific; "feels clunky" is not specific — "the form clears all fields when one is invalid, so the user retypes work they already did" is)
- **Change** — plain English description of what would be different
- **Why it matters** — what the user gains, in cumulative terms (trust, speed, recovery, confidence, voice)

Use CONTEXT.md vocabulary for the product, and LANGUAGE.md vocabulary for the craft. If CONTEXT.md says the product calls them "Threads" not "Conversations," use that. If the voice is "direct, no exclamation marks," apply that lens to copy suggestions.

**ADR conflicts:** if a candidate contradicts an existing ADR, only surface it when the friction is real enough to warrant reopening the decision. Mark it clearly (e.g. "contradicts ADR-0012 'no modal confirmations' — but worth reopening because…"). Don't list every theoretical change an ADR forbids.

Do NOT propose final copy, visuals, or animation specs yet. Ask the user: "Which of these would you like to explore?"

### 3. Grilling loop

Once the user picks a candidate, drop into a grilling conversation. Walk the touchpoint with them — what state the user is in at this exact moment, what they're trying to do, what the system already knows that it could use, what recovery looks like, what the voice should sound like here, what gets removed.

Side effects happen inline as decisions crystallize:

- Establishing a new pattern not in CONTEXT.md (e.g. "destructive actions get undo toasts, never modals" or "empty states always offer the next action, not just describe the absence")? Add it to CONTEXT.md — same discipline as `/grill-with-docs` (see CONTEXT-FORMAT.md). Create the file lazily if it doesn't exist.
- Sharpening voice during the conversation (e.g. landing on "we don't apologize in error copy, we explain")? Update CONTEXT.md right there.
- User rejects the candidate with a load-bearing reason? Offer an ADR, framed as: "Want me to record this so future polish passes don't re-suggest it?" Only offer when the reason would actually be needed by a future explorer to avoid re-suggesting the same thing — skip ephemeral reasons ("not a priority this quarter") and self-evident ones. See ADR-FORMAT.md.
- Want to explore alternative treatments for the touchpoint (modal vs inline vs toast, optimistic vs confirmed, prevent vs recover)? See INTERACTION-DESIGN.md.
