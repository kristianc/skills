# CONTEXT-FORMAT.md

CONTEXT.md is where the product's voice and established patterns live. It is the document the polish skill reads first, and the document it updates lazily as decisions crystallize during grilling.

## What goes in

Three things, and only these three:

**Voice.** How the product sounds. Vocabulary it does and doesn't use. Tone (direct, warm, terse, formal). Attitude (does it apologize, joke, exclaim, hedge?). What it calls things ("Threads" not "Conversations," "Workspace" not "Account").

**Patterns.** Touchpoint treatments that have been used at least twice and named. "Destructive actions get undo toasts, not modals." "Empty states always offer the next action." "Errors quote the user's input back to them." A pattern entry says what the pattern is, when it applies, and when it doesn't.

**Anti-patterns.** Explicit decisions about what the product does not do. "We don't use exclamation marks." "We don't say 'oops'." "We don't show progress bars under 2 seconds." Equally load-bearing as patterns.

## What does not go in

- **Implementation details.** CONTEXT.md describes the product's posture, not its CSS.
- **Aspirations.** If something is not yet true twice, it's not a pattern.
- **Decisions with rationale.** That's what ADRs are for. CONTEXT.md states the rule; ADRs explain why.
- **Per-screen specifications.** CONTEXT.md is general. Screen-specific design lives with the screen.

## Format

Loose markdown. One section each for Voice, Patterns, Anti-patterns. Each entry is short — a name, a sentence or two, an example or counter-example.

### Example: Patterns section

```
## Patterns

### Undo for destructive actions
Destructive actions resolve optimistically and surface a 5-second undo toast.
Confirmation modals are reserved for irreversible actions (account deletion, payment).
Example: deleting a thread → thread vanishes, "Thread deleted. Undo." appears for 5s.

### Empty states offer the next action
Empty states never just describe absence. They offer the action that fills them.
"No threads yet." → no. "No threads yet — start one." with a button → yes.
Exception: filtered empty states ("No matches for 'foo'") describe the filter, not a creation action.
```

### Example: Voice section

```
## Voice

### No false cheer
We don't use exclamation marks, "Yay!", "Awesome!", or "Oops!".
We aim for the tone of a competent colleague, not a mascot.

### Plain over precise
We use the word a user would use, not the word that's technically more accurate.
"Sign in," not "Authenticate." "Folder," not "Directory."
```

### Example: Anti-patterns section

```
## Anti-patterns

### No browser confirms
We never use window.confirm() or window.alert(). They look unowned and break voice.

### No spinners under 1 second
Sub-second waits should not show a loading indicator.
The flicker reads as broken; the eye barely registers the wait.
```

## How to update during grilling

When a grilling conversation crystallizes a new pattern or sharpens existing voice, update CONTEXT.md inline — don't queue it for later. Same discipline as `/grill-with-docs`.

Add a new entry only when:

- The pattern has been used (or decided on) for at least two touchpoints.
- The voice rule is concrete enough to apply. ("Direct" alone is not concrete; "no exclamation marks" is.)
- You can write it in a sentence.

If you can't write it in a sentence, you don't understand it well enough yet. Keep grilling.

## How to remove from CONTEXT.md

If a pattern is being broken in a third place for what the user considers a good reason, the pattern may need amending or retiring. Don't quietly let drift accumulate — either:

- **Amend the pattern** to include the new case ("…except in onboarding flows, where confirmation is preferred to undo"),
- **Split the pattern** into two narrower patterns,
- Or **retire the pattern** and write an ADR explaining why it didn't hold up.

Retiring a pattern without recording why means the next polish pass will reinvent it.

## Relationship to ADRs

CONTEXT.md states the current rule. ADRs explain why. If they disagree, one is wrong — CONTEXT.md may have drifted, or the ADR may be superseded. Reconcile in the grilling conversation; don't let the disagreement persist.
