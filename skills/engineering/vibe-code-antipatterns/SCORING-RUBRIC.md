# SCORING-RUBRIC.md

How to score each smell found during the audit. Every finding gets rated 1-5 on three axes: **fragility**, **contagion**, and **fix cost**. The combination drives prioritization: what to fix first, what to leave, and what to defer.

The point of scoring is triage, not precision. The audit needs to answer: "Where should we spend our refactoring time?" The scores tell you where the real danger is.

---

## Axis 1: Fragility

How likely is this smell to cause a failure when someone modifies nearby code?

**5 — Imminent breakage.** Any change to this code or its neighbors will likely cause a bug. The code is held together by implicit assumptions that are not documented or tested.

- A 400-line component where modifying one `useEffect` breaks an unrelated piece of state because they share mutable variables
- An API call without error handling that returns `undefined` to 10 downstream consumers, any of which will crash on `.property` access
- A circular dependency where import order determines whether a function is defined at call time

**4 — Fragile under change.** The code works today but has no safety net. Changes require understanding the entire file to avoid regressions.

- A god component where state interactions are implicit — adding a new state variable may trigger unexpected re-renders
- Business logic mixed into the UI layer — changing the calculation requires understanding the rendering context
- Hardcoded config that works for the current environment but will silently break in a different one

**3 — Risky without tests.** The code could be changed safely with care, but there is no verification mechanism. Changes require manual testing of scenarios the developer may not think of.

- Duplicated logic — changing one copy does not update the others, but the divergence may not surface immediately
- Near-duplicate components that should behave the same — a fix to one is not applied to the others
- No validation at input boundaries — invalid data passes through silently until it causes a downstream error

**2 — Annoying but survivable.** The code can be changed, but the developer will waste time understanding it. Low risk of actual breakage.

- Inconsistent patterns requiring the developer to check "which approach does this part use?"
- Poor naming requiring reading the implementation to understand intent
- Import disorder making it slow to find dependencies

**1 — Cosmetic.** The code works, can be changed without risk, and the smell is purely aesthetic or preferential.

- Style mixing (Tailwind in some files, CSS modules in others) where they do not conflict
- Console.log statements that produce noise but no harm
- Dead code that is clearly unused and just adds noise

---

## Axis 2: Contagion

Does this smell force other code to be worse? Does it spread?

**5 — Architectural cancer.** This smell defines the architecture — every new feature must conform to it. Fixing it requires rethinking the system.

- A god component that is the parent of 20 child components, all of which receive weird props shaped by the god component's internal state
- No API client — every new feature must implement its own fetch/auth/error pattern from scratch
- No type system (or `any` everywhere) — every new interface inherits the lack of safety

**4 — Forces bad patterns.** The smell creates pressure on nearby code to adopt the same approach. Developers copy the existing bad pattern because it is the only example.

- Duplicated API patterns that new developers copy because "that is how it is done here"
- Mixed state management — new features do not know which pattern to follow and add a third
- No test infrastructure — adding the first test requires setting up the entire test framework, so no one does

**3 — Local infection.** The smell makes its immediate neighbors worse but does not spread beyond one feature or module.

- A god component that passes mangled props to its direct children but does not affect other features
- A circular dependency between two specific modules that does not force other modules into cycles
- Missing error handling in one service that requires its callers to add defensive checks

**2 — Contained annoyance.** The smell is ugly in one place but does not pressure other code to be ugly.

- Dead code in a file — other files do not care
- Console.logs in a module — they do not spread
- Poor naming in internal variables — external callers see a clean interface

**1 — Isolated.** The smell exists in one place and has zero impact on anything else.

- A commented-out code block in a leaf component
- An unused import
- A TODO in a file that no other code depends on

---

## Axis 3: Fix cost

How expensive is the refactoring to address this smell?

**5 — Architectural rewrite.** Fixing this requires changing the fundamental structure of the application. Weeks of work, high risk of introducing bugs, requires comprehensive testing.

- Separating concerns in a 5000-line monolithic component that is the core of the application
- Introducing a type system to a large untyped JavaScript codebase
- Replacing a deeply-embedded state management approach with a different one across 50 components

**4 — Multi-day refactor.** Fixing this requires coordinated changes across many files. Significant testing needed. Risk of breaking things.

- Extracting a shared API client from 30 files that each implement their own fetch pattern
- Breaking circular dependencies that span multiple layers of the architecture
- Adding error handling to every API call in the application (if there are many)

**3 — Focused effort.** Fixing this takes a day or less and touches a bounded set of files. Moderate testing needed.

- Extracting hooks from a god component (the component stays, but gets thinner)
- Consolidating two duplicate components into one parameterized version
- Centralizing scattered config into environment variables

**2 — Quick fix.** Fixing this takes an hour or less. Low risk. Minimal testing needed.

- Adding error handling to a specific API call
- Renaming variables for clarity in one file
- Adding a loading state to a specific component
- Removing dead code

**1 — Trivial.** Fixing this takes minutes. Zero risk.

- Removing a console.log
- Deleting an unused import
- Adding a TODO to a tracked issue and removing the code comment
- Fixing import ordering

---

## Priority matrix

The combination of scores determines fix priority:

| Priority | Condition | Action |
|----------|-----------|--------|
| **Fix now** | Fragility 4-5 AND Contagion 4-5 AND Fix cost 1-3 | High danger, spreads actively, cheap to fix. No reason to wait. |
| **Fix next** | Fragility 4-5 AND Fix cost 1-3 | High danger, cheap to fix. Even if it does not spread, the risk is real. |
| **Plan the fix** | Fragility 4-5 AND Contagion 4-5 AND Fix cost 4-5 | High danger, spreads, but expensive. Needs a plan, not a quick fix. Schedule it. |
| **Fix when touched** | Fragility 3 AND Fix cost 1-3 | Moderate risk, cheap. Fix it when you are already in the file for another reason. |
| **Defer** | Fragility 1-2 regardless of other scores | Low risk. Not worth the effort unless you are actively cleaning up. |
| **Leave it** | Fragility 1-2 AND Fix cost 4-5 | Low risk and expensive. The refactor costs more than the smell. Do not touch it. |

---

## The triage question

For every finding, ask: **"What happens if a junior developer needs to modify this code tomorrow?"**

- If the answer is "they will probably introduce a bug because they cannot understand the interactions" — that is fragility 4-5.
- If the answer is "they will copy this bad pattern into their new feature" — that is contagion 4-5.
- If the answer is "they will be confused for 10 minutes then figure it out" — that is fragility 2-3 at most.
- If the answer is "they will not even notice this smell" — it is a 1 and should not be in the report.

---

## Distinguishing severity levels

**Critical (fix now or plan immediately):**
- God components in the core flow that every feature touches
- No error handling on API calls that handle money or user data
- No timeouts on external calls in high-traffic paths
- TODO-as-architecture in authentication or authorization code

**High (fix in the current sprint):**
- Duplicate components that are actively diverging
- No types on shared interfaces used by multiple features
- Missing validation at public API boundaries
- No tests for business-critical logic

**Medium (fix when you touch it):**
- Inconsistent patterns that confuse but do not break
- Dead code that adds noise but not danger
- Near-duplicate logic that has not diverged yet
- Config scattered but currently consistent

**Low (acknowledge and move on):**
- Naming inconsistency in internal variables
- Style mixing that does not cause conflicts
- Import disorder
- Console.logs in non-sensitive code paths
