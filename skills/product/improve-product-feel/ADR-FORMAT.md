# ADR-FORMAT.md

ADRs (decision records) capture UX decisions whose rationale needs to outlive the people who made them. They live in `docs/adr/` as numbered markdown files.

The point of an ADR is to prevent re-litigation. A polish review six months from now should not re-suggest a treatment that was deliberately rejected — and the only way to prevent that is to record why it was rejected, in a place future explorers will find.

## When to write one

Write an ADR when **all** of the following are true:

- A decision was made (a candidate was rejected, a pattern was chosen, a constraint was acknowledged).
- The reason wouldn't be independently rediscovered. A future polish pass would otherwise re-suggest the same thing.
- The reason is durable. It's not "we don't have time this quarter."

Common shapes:

- A polish candidate was rejected because the alternative had a non-obvious cost. Record what the cost was.
- A pattern was chosen over a plausible alternative. Record why the road not taken was not taken.
- A constraint exists that's not visible from the product alone. ("We can't use system-native confirmations because the product runs in an embedded iframe.") Not findable by exploration.

## When NOT to write one

- The reason is ephemeral. ("Not a priority this quarter." "We'll revisit when we have a designer.")
- The reason is self-evident from the product or CONTEXT.md.
- The decision is small enough that re-deciding it costs less than reading the ADR.
- The decision was about a specific bug or one-off, not a general posture.

The bar matters: a directory full of ADRs no one needed becomes noise that buries the ones that mattered.

## Format

A short markdown file. Filename: `NNNN-kebab-case-title.md`, numbered sequentially. Keep it short — most ADRs fit on one screen.

Sections:

- **Title** — what was decided, in one line.
- **Status** — `Accepted`, `Superseded by NNNN`, or `Deprecated`.
- **Context** — the situation. What polish opportunity surfaced this. What the alternative was.
- **Decision** — what was chosen, in one short paragraph.
- **Consequences** — what this means going forward. What it forecloses. What it makes easier.

## Example

```markdown
# 0007 — No modal confirmations for destructive actions

**Status:** Accepted

**Context:** The polish review surfaced inconsistency in how destructive actions
are confirmed — some used modals, some used native confirms, some had no
confirmation. Users reported feeling "nagged" by the modals during a usability
session. Standardizing on modals was proposed.

**Decision:** Destructive actions use undo toasts, not confirmation modals.
Modals are reserved for actions that are genuinely irreversible (account
deletion, payment submission, public publish).

**Consequences:**
- Every destructive action must support undo at the data layer. This is an
  engineering implication, not just a UI one.
- Polish reviews should not re-propose modals for delete / archive / remove
  flows.
- Truly irreversible actions need a separate, named pattern (see CONTEXT.md
  "Confirmation for irreversible actions").
- Onboarding-stage destructive actions are an open question — first-time users
  may not know undo exists.
```

## Tone

ADRs are written in past tense, neutral voice. They are not sales pitches for the decision.

Describe the alternative honestly. A future reader needs to be able to evaluate whether circumstances have changed — if the ADR makes the rejected option sound foolish, no one can revisit it without feeling foolish themselves.

A good test: would the person who proposed the rejected option recognize their argument in the Context section? If not, the ADR is biased.

## Superseding

When a later decision overrides an earlier ADR, do NOT delete the earlier one. Mark its status `Superseded by NNNN` and add a one-line note pointing to the new ADR.

Deleting superseded ADRs erases the trail of how the product's thinking evolved, which is exactly what future explorers need.

## Relationship to CONTEXT.md

CONTEXT.md states the current rule. ADRs explain why. The skill reads CONTEXT.md to know what the product does; it reads ADRs to know what the product deliberately doesn't do, and why a specific path was chosen.

If an ADR exists for a decision, the corresponding rule should be in CONTEXT.md too. If the rule is in CONTEXT.md but no ADR exists, that's fine — most rules don't need an ADR. ADRs are reserved for the load-bearing ones.
