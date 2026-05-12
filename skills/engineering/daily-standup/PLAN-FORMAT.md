# PLAN-FORMAT.md

The output format for the daily plan. This is what the user sees after scanning and triaging.

---

## Template

```
## Daily Plan — [YYYY-MM-DD]

### Summary
[1-2 sentences: what the codebase needs most today and why. Be specific —
"the billing module has no error handling and three open bugs" not
"there are some things to improve."]

### Scan Health
- Issues: [X open, Y bugs, Z unassigned]
- Recent activity: [X commits in last 3 days, Y files changed]
- TODOs: [X total, Y structural (error/auth/security)]
- Test coverage: [X source files, Y test files, ratio]
- Dependency health: [X vulnerabilities or "clean"]

### Items

#### 1. [Title] — [AFK/HITL] — [X hours raw -> Y hours compressed]
**What:** [One paragraph. Specific enough to act on. Names files, modules,
or subsystems involved. Does not prescribe implementation — says what needs
to change, not how to change it.]

**Why:** [What improves. Speaks to one or more of: user impact, risk
reduction, velocity improvement. Connects the work to an outcome, not
just a metric.]

**Done when:**
- [Specific, testable criterion]
- [Another specific, testable criterion]
- [Criteria an agent can verify: tests pass, grep returns 0, lint clean]

**Signals:** [What scan findings led to this item. Issue numbers, file
names, TODO counts, commit hashes — the evidence trail.]

**Priority score:** [Impact: X/15 | Effort: Y hours | Confidence: High/Medium/Low]

#### 2. ...

#### 3. ...

### Totals
- Raw hours: [sum] (target: 36-48)
- Compressed hours: [sum] (expected actual: 18-24)
- AFK items: [count] ([hours] compressed)
- HITL items: [count] ([hours] compressed)
- Balance: [X% product-forward / Y% internal quality] (target: 70/30)

### Carryover
[Items from yesterday that reappear, or "No carryover — clean slate."]
```

---

## Field guidance

### Title

Short, action-oriented. Starts with a verb. Specific enough to distinguish from other items.

Good:
- "Add error handling to billing API calls"
- "Fix the three open checkout bugs (#12, #15, #18)"
- "Extract shared validation logic from form components"

Bad:
- "Improve code quality" (too vague)
- "Work on issues" (not actionable)
- "Refactor" (refactor what?)

### What

One paragraph. Names the specific files, modules, or subsystems involved. Describes what needs to change without prescribing how. An experienced developer or a capable agent should be able to read this and know what to do.

Good: "The billing module (`src/billing/`) makes 8 API calls, none of which have error handling. When the payment provider returns an error, the UI shows a blank screen. Add try/catch to each API call, log the error with context, and show the user a specific message explaining what happened and what they can do."

Bad: "Fix error handling in the codebase." (Where? What kind of errors? What does the fix look like?)

### Why

Connects the work to an outcome the user or team cares about. Not "because the code is messy" but "because users see a blank screen when payment fails, and we have three support tickets about it this week."

Speak to the impact axes:
- **User-facing:** "Users currently see [bad experience] and will instead see [good experience]."
- **Risk reduction:** "This path has no error handling. When [thing] fails, [consequence]. This fix prevents [incident]."
- **Velocity:** "This file is touched in every PR and is 600 lines of mixed concerns. Splitting it means future changes are smaller and safer."

### Done when

Acceptance criteria that are specific enough for an agent to verify. Avoid subjective criteria for AFK items.

Good (AFK):
- "All API calls in `src/billing/` are wrapped in try/catch"
- "`grep -rn 'console.log' src/billing/` returns 0 results"
- "The test suite for `calculatePricing` covers all 6 documented cases and passes"
- "`npm run lint` produces no new warnings in the billing module"

Good (HITL):
- "The onboarding error messages have been reviewed and approved by the team"
- "The caching strategy is documented in an ADR with trade-offs explained"

Bad:
- "Code is cleaner" (not testable)
- "Error handling is better" (better how?)
- "Tests exist" (how many? for what?)

### Signals

The evidence trail. What scan findings led to this work item? Include specifics:
- Issue numbers: "#12, #15, #18 — all billing-related bugs"
- File names: "`src/billing/api.ts` — 0 try/catch blocks in 8 fetch calls"
- Commit hashes: "`a1b2c3d` — WIP commit message, incomplete refactor"
- TODO counts: "14 TODOs in `src/auth/`, 6 mention 'security'"
- Metrics: "47 `console.log` statements in production code"

This makes the plan auditable. The user can check whether the signals are real and whether the proposed work is the right response.

### Priority score

A quick summary of the prioritization math:
- **Impact:** combined score out of 15 (user-facing + risk reduction + velocity)
- **Effort:** compressed hours (what the user is actually committing to)
- **Confidence:** how reliable the estimate is

This lets the user quickly spot if a low-impact item is ranked too high or a high-effort item is ranked above an easy win.

---

## Handling carryover

Check `git log --since="1 day ago"` for work that was in progress yesterday.

- **If work was started and not finished:** include it in today's plan with updated estimates (subtract work already done). Note it as carryover.
- **If work was started and abandoned:** include it only if the signals still support it. Sometimes yesterday's priority is no longer relevant.
- **If yesterday's plan was fully completed:** note "no carryover" explicitly. This is a good signal.

Do not carry over items indefinitely. If an item has been in the plan for 3+ days without progress, it is either blocked (surface the blocker) or not actually a priority (drop it).

---

## Balancing the plan

### Product-forward vs. internal quality

Calculate the percentage:
- **Product-forward items:** bug fixes, feature work, UX improvements, performance improvements that users notice, copy improvements
- **Internal quality items:** refactoring, test additions for existing code, dependency updates, dead code removal, linting fixes

Target: 70% product-forward, 30% internal quality. Acceptable range: 60-80% product-forward.

If the plan is heavily tilted toward internal quality (under 60% product-forward), explicitly note this and explain why. Valid reasons: "The codebase has critical tech debt that is blocking feature work" or "A security audit surfaced 5 high-severity findings." Invalid reason: "There are lots of TODOs."

### Size distribution

The plan should not be all small items or all large items:
- Include at least one substantial item (8+ raw hours) — this is the day's main work
- Include at least one quick win (1-3 raw hours) — this builds momentum
- The remaining items fill the gap

A plan with five 8-hour items is unrealistic — context switching alone would kill the day. A plan with fifteen 2-hour items is scattered — nothing gets deep attention.

### AFK vs. HITL balance

Front-load AFK items so the agent can start immediately. But the plan should not be 100% AFK unless the codebase genuinely has no decisions to make.

A healthy plan has 2-3 AFK items and 1-2 HITL items. The AFK items keep the agent busy while the human reviews and decides on the HITL items.
