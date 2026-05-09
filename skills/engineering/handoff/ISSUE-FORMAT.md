# ISSUE-FORMAT.md

The template for each issue created during the publish step. Every issue is a vertical slice of the spec — independently grabbable by a human or an agent.

## Template

```markdown
## [Imperative title]

**Type:** HITL | AFK
**Parent:** [link to the spec issue or document]
**Blocked by:** [comma-separated list of issue IDs, or "None"]

### What to build

[One paragraph. What this issue delivers, stated in terms of decisions from the spec. No file paths, no code snippets, no function names — those go stale the moment someone commits. Describe the outcome, not the implementation.]

### Acceptance criteria

- [ ] [Criterion 1 — independently testable]
- [ ] [Criterion 2 — independently testable]
- [ ] [Criterion 3 — independently testable]
```

## Field guidance

### Title

The title is imperative and specific. It says what changes when this issue is done.

**Good titles:**

- "Add password reset flow via email with token expiry"
- "Rate-limit the reset endpoint to 5 requests per email per hour"
- "Show expiry and already-used states for reset tokens"
- "Review reset flow copy for voice and recovery paths"

**Bad titles:**

- "Password reset" (too vague — which part?)
- "Backend work for reset feature" (horizontal layer, not a slice)
- "Implement POST /auth/reset endpoint" (file-path-level detail in the title — goes stale)
- "Reset — part 2" (meaningless without reading part 1)
- "Nice error messages" (vague and subjective)

The title should make sense in a list of 20 issues, without reading the description. If two issues have titles that could be confused with each other, one or both are too vague.

### Type: HITL vs. AFK

Label every issue with exactly one type. See SLICING.md for the classification methodology.

- **AFK** — the issue can be completed by an agent without human decisions. The acceptance criteria are the full contract.
- **HITL** — the issue requires human judgment at some point. State what judgment is needed in the "what to build" paragraph.

Don't hedge with "AFK but might need review." Every issue gets reviewed. AFK means the implementation doesn't require human decisions, not that it doesn't require human review.

### Parent

Link to the spec. Every issue is a slice of a spec. The spec is the source of truth for why this issue exists and what decisions informed it.

If the spec is a GitHub issue, link to it by number. If it's a document in the repo, link to the file. Either way, the implementer should be able to read the spec for full context.

### Blocked by

List the issue IDs of dependencies — issues that must be completed before this one can start.

**Blocked by means structurally blocked.** Not "it would be nice to have this first" but "this issue literally cannot be built without the output of that issue."

Use real issue IDs, not placeholders. This is why the publish step creates issues in dependency order: the blocker is created first, gets a real ID, and the downstream issue references that real ID.

If the issue has no dependencies, write "None."

### What to build

One paragraph that describes what this slice delivers. The paragraph should:

- Reference decisions from the spec, not re-argue them
- State the outcome in user or system terms ("the user sees X," "the system enforces Y")
- Omit file paths, function names, class names, or line numbers — these change constantly and the implementer will find the right place in the code
- Include enough context that the implementer doesn't need to read every other issue to understand this one

**Good:**

"Add rate limiting to the password reset endpoint. The spec decided on a limit of 5 requests per email address per hour, returning a 429 status with a Retry-After header. The UI should show a clear message when the limit is hit, telling the user when they can try again. This follows the existing rate-limiting pattern used on the login endpoint."

**Bad:**

"In `src/api/auth/reset.ts`, add a rate limiter using the `RateLimiter` class from `src/lib/rate-limit.ts`. Call `rateLimiter.check(email)` before processing the request. If it returns false, return `res.status(429).json({ error: 'Too many requests', retryAfter: rateLimiter.retryAfter(email) })`. In `src/components/ResetForm.tsx`, handle the 429 response by showing the `RateLimitBanner` component."

The bad version is longer, more specific, and worse. It will be wrong the moment someone renames a file, moves a function, or refactors the rate limiter. The good version describes what to build; the bad version prescribes how to build it using today's code layout.

### Acceptance criteria

Each criterion is a single statement that can be independently verified. The criteria are the contract: when all are checked, the issue is done.

**Rules for acceptance criteria:**

1. **Each criterion is independently testable.** Someone can verify it without verifying the others. "The form submits" and "the form shows an error on invalid input" are independently testable. "The form works correctly" is not.

2. **No implementation details.** "The system validates the email format before sending the request" is an acceptance criterion. "The `validateEmail()` function is called in the `onSubmit` handler" is an implementation detail.

3. **No subjective quality.** "The error message explains what went wrong and offers a next step" is verifiable. "The error message is helpful" is subjective. If a criterion requires judgment to verify, the issue should be HITL.

4. **Specific numbers where applicable.** "The rate limit is 5 requests per email per hour" is specific. "The endpoint is rate limited" is not — limited to what?

5. **Cover the edges.** Don't just test the happy path. If the slice includes error handling, the acceptance criteria should verify the error states. If it includes a limit, verify the behavior at and beyond the limit.

6. **3-5 criteria per issue.** Fewer than 3 suggests the issue is too thin or under-specified. More than 7-8 suggests the issue is too thick and should be split. 3-5 is the sweet spot.

**Good acceptance criteria:**

```
- [ ] User can submit their email address on the reset form
- [ ] System sends a reset email with a unique link within 30 seconds
- [ ] Clicking the link opens a form to set a new password
- [ ] After setting a new password, the user can log in with it
- [ ] If the email address is not associated with an account, the same confirmation message is shown (no enumeration)
```

**Bad acceptance criteria:**

```
- [ ] Reset works
- [ ] Errors are handled
- [ ] Tests pass
- [ ] Code is clean
```

(Not testable, not specific, not independently verifiable.)

## Publishing order

Issues are created in dependency order: if issue B is blocked by issue A, then issue A is created first.

This matters because:

1. Issue A gets a real ID (e.g., #42) when created
2. Issue B's "Blocked by" field can reference #42 — a real, clickable link
3. The issue tracker reflects the true dependency graph

Never use placeholder IDs ("blocked by [TBD]" or "blocked by the schema issue"). If you don't have the real ID, you haven't published the dependency yet. Fix the order.

The publishing algorithm:

1. Build the dependency graph from the slice definitions
2. Topologically sort the graph (issues with no dependencies first)
3. Create issues in that order, capturing real IDs as they're created
4. For each issue with dependencies, substitute real IDs into the "blocked by" field before creating

If the dependency graph has a cycle, the slicing is wrong. Two issues cannot each depend on the other. Re-slice until the cycle is broken.

## Labels

Apply these labels during publishing:

- `HITL` or `AFK` — matches the type field
- `handoff` — marks issues created by this skill, so they can be filtered
- Any additional labels the user requests (e.g., `security`, `polish`, `launch-blocker`)

## After publishing

Report back to the user with:

- The spec issue/document link
- A numbered list of all created issues with their IDs, titles, types, and dependency relationships
- A count of AFK vs. HITL issues
- Any issues that have no blockers and can be started immediately
