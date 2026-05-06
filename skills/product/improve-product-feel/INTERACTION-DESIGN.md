# INTERACTION-DESIGN.md

Once a polish candidate has been picked and the grilling conversation is underway, this is the structured walk through the design tree for a single touchpoint.

The goal is not to enumerate every option. The goal is to make the trade-offs explicit so the user can make a real choice instead of accepting the first thing that comes to mind.

## The five questions

For any touchpoint, walk these in order. Each one narrows the design space.

### 1. What state is the user in at this moment?

Not "what page are they on" — what's in their head.

Are they:

- confirming work they just did,
- recovering from a mistake,
- exploring without commitment,
- returning after absence,
- blocked and waiting,
- about to do something irreversible?

The same UI element answers different needs in different states. An empty state on first visit is a welcome; an empty state after a filter clears everything is a dead end. An error during a fast flow needs to be unobtrusive; the same error during a high-stakes flow needs to stop the user.

If you can't name the state, you can't design for it. Push the user to articulate.

### 2. What does the system already know that it could use?

Most defaults exist because the touchpoint was designed without using context the system has.

- The empty inbox knows whether the user has ever had mail.
- The error knows what the user typed.
- The loading state knows how long the operation usually takes.
- The "are you sure" knows whether this user has ever undone an action.
- The form knows which field failed validation.

Crafted touchpoints use what's available. Defaults pretend the system knows nothing. Walk through what the system has — much of it isn't currently being used in the touchpoint.

### 3. Prevent, recover, or accept?

For any friction-causing event (error, mistake, slow operation), there are three postures:

- **Prevent it.** Validate before submit. Disable the impossible action. Make the wrong path unreachable.
- **Recover from it.** Undo. Retry. Restore. Let the failure happen but make it cheap.
- **Accept and explain.** The operation is genuinely slow, the constraint is real — tell the user honestly, and make the wait or the limit feel respected rather than ignored.

Defaults usually pick none — they let the failure happen and surface a generic message. Crafted touchpoints pick one deliberately.

Rough heuristic:

- Prevention is best when the cost of failure is high and prevention is cheap.
- Recovery is best when the action is common and the failure is rare.
- Acceptance is best when the underlying reality can't be hidden, and pretending otherwise erodes trust.

Picking the wrong one is a real mistake — preventing a low-cost action over-restricts; trying to recover from something irrecoverable misleads.

### 4. Optimistic, confirmed, or interrupting?

For any action with a system response:

- **Optimistic** — show the result immediately, reconcile in the background, undo on failure. Best when failure is rare and recovery is cheap.
- **Confirmed** — wait for the system, then show the outcome. Best when the user needs to know it actually happened (payment, send, publish).
- **Interrupting** — modal, blocking. Best when the user must engage before continuing. Rarer than designs assume.

Most defaults are confirmed where they could be optimistic. Most modals are interrupting where they could be inline. The grilling question: what would actually break if this were one step less interrupting?

### 5. What does this sound like?

Once the structure is decided, write the words.

Not last-minute placeholder copy — the words are part of the design. Read them aloud:

- Do they sound like the product? (Check CONTEXT.md voice.)
- Do they explain or do they apologize?
- Do they assume the user did something wrong, when often the system did?
- Do they tell the user what to do next, or leave them stranded?
- Do they use the product's vocabulary or the framework's?

A good touchpoint with bad copy still feels bad. Words are the last mile.

## Trade-off axes

When the user is choosing between two treatments, the trade-off is usually one of these. Name the axis when presenting alternatives.

- **Speed vs. certainty** — optimistic is faster but can be wrong; confirmed is slower but accurate.
- **Forgiveness vs. friction** — undo is forgiving but requires plumbing; confirmation is high-friction but cheap to build.
- **Density vs. clarity** — inline shows more at once; separate views explain better.
- **Anticipation vs. honesty** — guessing the user's intent saves taps but can be wrong; asking is slower but unambiguous.
- **Consistency vs. fit** — using the established pattern is predictable; a bespoke treatment may fit this case better but adds vocabulary.

"We can do A (faster, can be wrong) or B (slower, always right) — which fits this touchpoint?" lets the user choose. "We can do A or B" doesn't.

## Convergence

When the design tree has been walked, write the resulting treatment as a single short paragraph, covering: state, system context used, posture (prevent/recover/accept), response model (optimistic/confirmed/interrupting), voice.

If it can't fit in a paragraph, the design isn't done.

Then check it against CONTEXT.md:

- If this is the **first** use of a treatment, it's a one-off. Note it but don't promote it to a pattern yet.
- If this is the **second** use of a treatment that already exists once elsewhere, name it and add it to CONTEXT.md before moving on. This is the moment a pattern is born; if you don't capture it now, the third instance will drift.
- If this **breaks** an existing pattern in CONTEXT.md, stop. Either the pattern needs amending (add the exception), the pattern needs retiring (and an ADR explaining why), or this candidate should be reconsidered.

## When the grilling stalls

If the design tree walk isn't converging, usually one of these is true:

- **Question 1 wasn't answered.** The user state is fuzzy, so every decision downstream is fuzzy. Push back to state.
- **The candidate is two candidates.** It's actually two touchpoints, and they're being conflated. Split.
- **There's an unspoken constraint.** Engineering, brand, legal, business. Surface it; it might warrant an ADR.
- **The candidate isn't actually polish.** It's a feature in disguise. Decline; this skill doesn't do features.
