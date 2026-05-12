# PRIORITIZATION.md

How to evaluate and rank candidate work items from the scan phase. This framework turns a bag of signals into an ordered plan.

---

## Impact axes

Score each candidate work item on three axes, 1-5 each.

### User-facing (1-5)

Does fixing this improve what users see, experience, or trust?

| Score | Meaning |
|-------|---------|
| 5 | Users are actively hitting this problem (reported bug, broken flow, data loss) |
| 4 | Users will notice the improvement immediately (faster load, better error message, fixed empty state) |
| 3 | Users benefit indirectly (fewer bugs reach them, better error recovery) |
| 2 | Users benefit only in edge cases or over long timeframes |
| 1 | Purely internal — users never see this |

### Risk reduction (1-5)

Does this prevent a future incident, bug, or security issue?

| Score | Meaning |
|-------|---------|
| 5 | Active security vulnerability, data loss risk, or production incident waiting to happen |
| 4 | Missing error handling on a critical path, no rollback plan, untested payment/auth code |
| 3 | Missing tests on frequently-changed code, unhandled edge cases in core flows |
| 2 | Technical debt that increases the probability of future bugs |
| 1 | Cosmetic or stylistic issue with no risk implications |

### Velocity (1-5)

Does this make future work faster — reduce debt, improve DX, add tests, clarify architecture?

| Score | Meaning |
|-------|---------|
| 5 | Unblocks multiple other work items or eliminates a recurring time sink |
| 4 | Significantly reduces friction for a common task (refactoring a god component everyone touches) |
| 3 | Makes one area of the codebase meaningfully easier to work in |
| 2 | Minor DX improvement or cleanup |
| 1 | No velocity impact |

**Combined impact score:** User-facing + Risk reduction + Velocity. Maximum 15.

---

## Effort estimation

### Raw hours

How long would this take without AI assistance? This is the planning target — the plan should total 36-48 raw hours.

Guidelines for estimation:
- **1-2 hours raw:** Single-file changes, adding a test, fixing a lint warning, updating copy
- **3-4 hours raw:** Multi-file refactor with a clear pattern, adding error handling across a feature, dependency update with migration
- **6-8 hours raw:** Extracting a shared abstraction, adding a test suite for an untested module, fixing a systemic pattern
- **10-16 hours raw:** Architectural change, major refactor across multiple subsystems, implementing a new cross-cutting concern
- **20+ hours raw:** Large feature, full rewrite of a subsystem. Too large for a single work item — break it down.

### Compressed hours

Estimated time with AI agent assistance. Use these multipliers:

- **AFK work:** 40-50% of raw hours. Mechanical, well-defined tasks compress the most.
- **HITL work:** 60-70% of raw hours. Human decision-making is the bottleneck; the agent speeds up implementation between decisions.
- **Research/investigation work:** 70-80% of raw hours. The thinking is the work; execution speed matters less.

### Confidence

How reliably can you estimate this?

- **High:** You have done similar work before or the scope is clearly bounded. The estimate is likely within 25%.
- **Medium:** The scope is roughly understood but may have surprises. The estimate could be off by 50%.
- **Low:** The scope is unclear, depends on what you find, or involves unfamiliar systems. The estimate could be off by 2x.

Low-confidence items should be time-boxed: "Spend 2 hours investigating, then re-estimate." Do not put a low-confidence 16-hour item in the plan without a checkpoint.

---

## AFK vs. HITL classification

### AFK indicators

The work item is suitable for autonomous agent execution when:

- The scope is well-defined: specific files, specific changes, specific acceptance criteria
- The changes are mechanical: applying an established pattern, adding tests for existing behavior, fixing lint warnings, removing dead code
- The result is verifiable by tests, types, or linting — no subjective judgment needed
- There are no design decisions: the "what" is decided, only the "how" remains
- The changes are low-risk: a mistake can be caught in review, not in production

**Examples:**
- "Add error handling to all API calls in the billing module — wrap in try/catch, log the error, show the user a specific recovery message"
- "Remove all 47 `console.log` statements from production code"
- "Add unit tests for the `calculatePricing` function covering the 6 cases documented in the spec"
- "Update all instances of the deprecated `findOne` method to use `findUnique`"

### HITL indicators

The work item requires human judgment when:

- The acceptance criteria involve subjective quality ("the copy should feel helpful")
- The work requires choosing between trade-offs the team hasn't decided on
- The changes affect user-facing copy, design, or interaction patterns
- The work involves business logic that isn't documented or is ambiguous
- The changes have high blast radius: auth, payments, data migrations
- The work requires talking to someone outside the codebase (stakeholder, customer, designer)

**Examples:**
- "Rewrite the onboarding flow's error messages to match the product voice"
- "Decide whether to consolidate the three notification systems or keep them separate"
- "Review the pricing page copy for clarity and conversion"
- "Choose a caching strategy for the dashboard: client-side, server-side, or edge"

### When in doubt, classify as AFK

Same reasoning as the handoff skill: the cost of false AFK (agent attempts, gets stuck, escalates) is lower than false HITL (work sits in a queue waiting for a human who is busy). An AFK item that needs human input will surface that need quickly. A HITL item that could have been AFK waits unnecessarily.

---

## Prioritization rules

### Ordering algorithm

1. **Safety overrides:** Anything with risk-reduction score of 5 goes first, regardless of other scores. Security vulnerabilities, data loss risks, and production incident risks do not wait.

2. **AFK-first within equal priority:** When two items have similar combined impact scores, the AFK item goes first. This lets the agent start immediately while the human reviews HITL items.

3. **Impact-per-hour ranking:** For remaining items, rank by combined impact score divided by compressed hours. This surfaces high-impact, low-effort items.

4. **Confidence tiebreaker:** When impact-per-hour is similar, prefer the higher-confidence estimate. Known work over unknown work.

### The 70/30 rule

At least 70% of the plan's raw hours should move the product forward — work that users notice:
- Bug fixes
- Feature completion
- Performance improvements
- UX improvements (error messages, empty states, copy)

At most 30% can be internal quality work that users do not see:
- Refactoring
- Adding tests to existing code
- Dependency updates
- Code cleanup and dead code removal

Why: teams that spend entire days on refactoring feel productive but ship nothing. Teams that only ship features accumulate debt that slows them down. 70/30 is the balance.

If the scan produces only refactoring candidates, that is a signal: either the product is in great shape (unlikely) or the scan missed user-facing opportunities. Recheck issues and recent commits.

### Handling conflicts

- **Two items touch the same files:** order them sequentially. The larger refactor goes first; the smaller change goes second and adapts to the refactored code.
- **A HITL item blocks an AFK item:** put the HITL item first with a note: "blocks item N, decision needed before agent can proceed."
- **The plan exceeds 48 hours:** cut the lowest-priority item. Do not compress estimates to make items fit.
- **The plan is under 36 hours:** add a lower-priority item or expand the scope of an existing item. Having slack in the plan is fine; having too little work means the agent runs out of tasks.

### Anti-patterns to avoid

- **All refactoring, no progress:** the 70/30 rule prevents this. If the plan is 100% refactoring, something is wrong.
- **All easy wins, no substance:** a plan with five 2-hour items feels productive but may skip the hard, important work. Include at least one meaty item (8+ hours).
- **Over-indexing on issue count:** five stale issues does not mean five work items. Group by subsystem or theme.
- **Ignoring carryover:** if yesterday's plan had unfinished items, they should appear today — either to finish or to explicitly deprioritize.
