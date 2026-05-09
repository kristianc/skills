# SLICING.md

How to break a spec into vertical slices. This is the methodology for step 3 of the handoff process.

## The core principle

Each slice is a thin end-to-end cut through ALL layers of the system that delivers visible, demoable value. Not a layer. Not a component. Not a concern. A slice.

A vertical slice for "user can reset their password" is not:

- "Build the reset API endpoint" (horizontal — API layer only)
- "Design the reset form" (horizontal — UI layer only)
- "Add reset token to the database schema" (horizontal — data layer only)

It is:

- "User submits their email on the reset form, the system sends a reset link, user clicks the link and sets a new password" (end-to-end: UI + API + database + email + tests)

The slice is independently demoable: you can show it to someone. It's independently verifiable: you can write acceptance criteria for it. It's independently deployable: it doesn't break anything if the next slice isn't built yet.

## The slicing process

### Step 1: Start from the user story

What can the user do when this slice is complete that they couldn't do before? If the answer is "nothing visible," the slice is horizontal. Push until you hit something visible.

"The API returns data" is not visible to the user. "The user sees their data on the dashboard" is.

### Step 2: Trace through every layer

For the user story, trace the path through the system:

- **UI** — what does the user see and interact with?
- **API** — what endpoints are called, with what inputs and outputs?
- **Business logic** — what rules are applied, what decisions are made?
- **Data** — what's read, written, created, or modified?
- **External services** — what third-party systems are involved?
- **Tests** — what acceptance tests verify this works?

If you skip a layer, you'll discover it during implementation as a surprise dependency.

### Step 3: Cut the thinnest possible slice

Now narrow: what's the thinnest version of this story that delivers visible value?

- Can the happy path be sliced from error handling? (Usually yes. Ship the happy path first, add error handling as a follow-up slice.)
- Can the simple case be sliced from the edge cases? (A password reset with one email address before handling "user has multiple accounts.")
- Can the basic UI be sliced from the polished UI? (A functional form before animations, loading states, and empty states.)
- Can the manual step be sliced from the automated step? (An admin manually triggers a process before building the scheduler.)

Each narrowing is a potential slice boundary. The question at each boundary: "Is the thinner version independently valuable, or is it just unfinished?"

**Independently valuable:** a password reset that only handles the happy path. Users can reset passwords. Edge cases are handled in the next slice.

**Just unfinished:** a password reset form that submits to an API that doesn't exist yet. The user sees a form, submits it, and gets an error. Not a slice — it's a broken feature.

### Step 4: Identify dependencies

For each slice, ask: "What must exist before this slice can be built?"

Dependencies are structural, not preferential. "The reset endpoint must exist before the reset form can call it" is a dependency. "It would be nice to have the design system updated before building the form" is a preference.

Map dependencies as a directed graph. The publish step will create issues in topological order (dependencies first).

Common dependency patterns:

- **Tracer bullet first.** The simplest case, wired end-to-end, is almost always the first dependency. Everything else builds on the proof that the architecture works.
- **Schema before logic.** If a slice requires new data structures, the schema change is usually a dependency. But not always — if the slice can use an existing structure temporarily, defer the schema change.
- **Shared infrastructure before features.** Auth middleware, error handling patterns, API conventions — if multiple slices need them, they're a shared dependency. But don't over-extract; only extract what two or more slices actually need.

### Step 5: Check granularity

Too thick:

- The slice takes more than a few days to build
- The slice has internal dependencies (part A must be done before part B within the same issue)
- The acceptance criteria list has more than 7-8 items
- A reviewer would struggle to hold the whole change in their head

Too thin:

- The slice is a single function or a single file change
- The overhead of creating, reviewing, and deploying the slice exceeds the value of having it separate
- The slice isn't independently demoable or verifiable
- The slice is only meaningful in the context of another slice (it's a horizontal layer in disguise)

The sweet spot: a slice a developer can complete in 1-3 days, with 3-5 acceptance criteria, that produces a visible change a user or reviewer can verify.

## HITL vs. AFK classification

Every issue gets classified as requiring human judgment (HITL) or executable by an agent autonomously (AFK).

### AFK indicators

The issue is suitable for agent execution when:

- The acceptance criteria are specific and testable (no judgment calls in "done")
- The implementation approach is clear from the decisions in the spec
- The issue doesn't require choosing between aesthetics, tone, or user experience trade-offs
- The issue is primarily mechanical: wiring, plumbing, implementing a decided pattern, adding tests for a defined behavior
- The issue can be verified programmatically (tests pass, linter passes, type checks pass)

Examples of AFK issues:

- "Add rate limiting to the reset endpoint: max 5 requests per email per hour, return 429 with Retry-After header"
- "Add database migration for password_reset_tokens table with columns: token (UUID), user_id (FK), expires_at (timestamp), used_at (nullable timestamp)"
- "Wire the reset form to call POST /auth/reset with the email field, show a confirmation message on 200, show the error message from the response body on 4xx"

### HITL indicators

The issue requires human judgment when:

- The acceptance criteria include subjective quality ("the error message should feel helpful")
- The issue involves copy decisions that weren't settled in the conversation
- The issue requires visual design judgment (layout, spacing, color, animation)
- The issue involves a business logic ambiguity the spec acknowledges but doesn't resolve
- The issue requires talking to a stakeholder, customer, or domain expert
- The implementation requires choosing between trade-offs the spec left open

Examples of HITL issues:

- "Write the email copy for the password reset email — tone should match the product voice in CONTEXT.md, but the specific copy needs review"
- "Decide the token expiry duration: 15 minutes (more secure, users may not check email in time) vs. 1 hour (less secure, more forgiving) — the spec notes this as an open question"
- "Review the reset flow end-to-end for friction: are there unnecessary steps, confusing messages, or moments where the user doesn't know what to do next?"

### Default to AFK

When in doubt, classify as AFK. The reasoning:

- An AFK issue that turns out to need human input will surface that need during implementation. It becomes HITL reactively, with a clear question.
- A HITL issue that could have been AFK sits in a queue, waiting for a human who may not get to it for days. It's a bottleneck created by caution.

The cost of false AFK (agent attempts, fails, escalates) is lower than the cost of false HITL (issue waits in a queue when it could have been done).

## Examples

### Good slicing: password reset feature

From the spec: "Users can reset their password via email. Token expires after 1 hour. Rate limited to 5 requests per email per hour. The reset form uses the product's existing form components."

**Slice 1 (AFK, no dependencies):** Happy path reset flow. User enters email, receives reset link, clicks link, sets new password. Minimal error handling (just network errors). No rate limiting yet.

**Slice 2 (AFK, depends on 1):** Rate limiting. Add rate limiting to the reset endpoint. Return 429 with Retry-After. Show "too many attempts" message in the UI.

**Slice 3 (AFK, depends on 1):** Token expiry handling. If the user clicks an expired link, show an expiry message with a "request new link" action. If the token has already been used, show a "link already used" message.

**Slice 4 (HITL, depends on 1):** Error and edge case copy review. Review all error messages, the reset email copy, and the success confirmation for voice and clarity.

### Bad slicing: horizontal layers

Same feature, but sliced horizontally:

**Issue 1:** "Build the password_reset_tokens database table and migration"
**Issue 2:** "Build POST /auth/reset and POST /auth/reset/confirm endpoints"
**Issue 3:** "Build the reset request form and the new password form"
**Issue 4:** "Write tests for the reset flow"

Problems:

- Issue 1 is not demoable. You can't show a database table to anyone.
- Issue 2 requires issue 1, and issue 3 requires issue 2 — the entire breakdown is serialized.
- Issue 4 treats tests as a separate phase. Tests should be part of each slice.
- No issue is independently verifiable by a user. You need all four to see anything work.
- If the API team and UI team work in parallel (issues 2 and 3), they'll discover integration mismatches at the end.

### Bad slicing: too thick

**Issue 1:** "Implement the entire password reset feature including happy path, error handling, rate limiting, token expiry, email sending, and all copy."

This is not a slice. It's the entire feature in one issue. No parallelization, no incremental progress, no way to demo intermediate work. If it takes two weeks and goes sideways on day 10, you have nothing to show for the first 9 days.

### Bad slicing: hidden dependencies

**Slice 1:** "User can reset their password via email"
**Slice 2:** "User can reset their password via SMS"

These look independent, but they share: the token storage mechanism, the rate limiting logic, and the "reset initiated" UI state. If slice 2 starts before slice 1 establishes those patterns, they'll build incompatible implementations. The real structure is:

**Slice 1:** Reset via email (establishes the token pattern, rate limiting, and UI states)
**Slice 2 (depends on 1):** Reset via SMS (reuses the token pattern, extends rate limiting, adds SMS as a channel option in the UI)

Hidden dependencies are the most common slicing failure. The fix: trace through the layers for each slice and look for shared infrastructure. If two slices touch the same data, the same middleware, or the same UI component, one probably depends on the other.
