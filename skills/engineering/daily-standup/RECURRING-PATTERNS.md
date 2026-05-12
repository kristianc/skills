# RECURRING-PATTERNS.md

Patterns that the daily standup surfaces repeatedly across multiple sessions. When the same signals appear day after day, they indicate structural issues — not daily tasks, but systemic problems in the project's health or process.

Check these patterns when a standup feels like a repeat of yesterday.

---

## Same TODOs for weeks

**What it looks like:** The TODO/FIXME count does not decrease between standups. The same TODOs appear in the scan results every day.

**What it signals:** Tech debt is accumulating. New TODOs are being added faster than old ones are resolved — or old ones are never resolved at all. The codebase is collecting promises nobody intends to keep.

**Why it matters:** Load-bearing TODOs (those marking missing error handling, auth gaps, or validation) are latent bugs. Aspirational TODOs ("TODO: add dark mode") are clutter that trains the team to ignore all TODOs, including the dangerous ones.

**One-line fix:** Schedule a 4-hour "TODO bankruptcy" session. Delete aspirational TODOs (move them to issues if they matter). Fix structural TODOs. Establish a rule: no TODO without an issue number.

**Structural fix:** Add a TODO linting rule that requires an issue reference. Track TODO count as a codebase health metric. If the count exceeds a threshold, it blocks new feature work until it is reduced.

---

## Tests never appear for new code

**What it looks like:** The scan repeatedly finds recently-changed source files with no corresponding test files. New features ship without tests. The test-to-source ratio stays flat or decreases.

**What it signals:** Testing culture gap. Either tests are seen as optional, the test infrastructure is too painful to use, or there is no enforcement mechanism.

**Why it matters:** Untested code is unrefactorable code. The moment you need to change it, you cannot tell if you broke something. Technical debt accrues silently until a major change forces a rewrite.

**One-line fix:** Add the highest-risk untested module to today's plan as an AFK item: "Write tests for [module] covering existing behavior."

**Structural fix:** Add test coverage requirements to CI. Not 100% — that is counterproductive — but a minimum threshold (60-70%) for new code. Make the test infrastructure easy to use: good test utilities, fast test runner, clear examples. If writing a test takes longer than writing the feature, the infrastructure is the problem.

---

## Same files keep getting modified

**What it looks like:** The hot spots analysis shows the same 3-5 files appearing in every standup's commit scan. These files are touched in every PR and every sprint.

**What it signals:** These files are doing too much. They are god components, utility dumping grounds, or shared modules that every feature needs to change. They are the codebase's bottleneck — both for development speed and for merge conflicts.

**Why it matters:** Files that change constantly are the highest-risk files. Every change is a chance for regression, and because everyone touches them, merge conflicts are frequent. They are also the hardest to refactor because the blast radius is large.

**One-line fix:** If the file is over 300 lines, it is a refactoring candidate. Schedule a decomposition as a plan item.

**Structural fix:** Apply the Single Responsibility Principle. The file should do one thing. Extract concerns into separate modules. The goal is that a typical feature change touches 1-2 files, not the same 5 files every time.

---

## Issues go stale

**What it looks like:** The scan repeatedly finds issues that have been open for 30, 60, 90+ days. The count of stale issues does not decrease. New issues are opened but old ones are not closed.

**What it signals:** Triage process is broken. Issues are being used as a wish list, not a work queue. Nobody is accountable for deciding "we will not do this" and closing the issue.

**Why it matters:** A bloated issue tracker is worse than useless — it creates the illusion of a plan while actually being a graveyard of good intentions. Developers waste time reading issues that will never be done. Important issues get buried under noise.

**One-line fix:** Close all issues older than 90 days that have no activity, assignee, or milestone. If they matter, someone will reopen them.

**Structural fix:** Institute a monthly triage ritual: every open issue is either assigned, milestoned, or closed. Limit the total number of open issues (30-50 for a small team). If a new issue must be opened and the limit is reached, another issue must be closed first.

---

## Security warnings ignored

**What it looks like:** `npm audit` (or equivalent) reports the same vulnerabilities across multiple standups. Dependency update PRs sit unmerged. The cross-skill security scan finds the same patterns (hardcoded secrets, exposed env vars) day after day.

**What it signals:** Dependency hygiene problem. Security work is seen as maintenance, not as real work. There is no process for triaging and fixing dependency vulnerabilities.

**Why it matters:** Known vulnerabilities with known exploits are the easiest attack vector. "We knew about it but didn't fix it" is the worst possible incident postmortem.

**One-line fix:** Schedule a dedicated "dependency sweep" as an AFK item: update all dependencies, run the audit, fix or suppress what remains.

**Structural fix:** Automate dependency updates (Dependabot, Renovate). Make security audit part of CI — the build fails on critical/high vulnerabilities. Schedule a monthly dependency review for medium/low findings.

---

## Error messages stay generic

**What it looks like:** The cross-skill error copy scan finds the same "Something went wrong" and "An error occurred" strings every time. The count does not decrease.

**What it signals:** Product polish gap. Developers are handling errors technically (catching them, logging them) but not experientially (telling the user what happened and what to do). Errors are treated as implementation details, not as user interactions.

**Why it matters:** Generic error messages are trust-destroying. The user hit a problem, and the product's response is "I don't know what happened either." Every generic error is a support ticket waiting to happen and a user considering whether to trust the product.

**One-line fix:** Run the error-copy-review skill on the highest-traffic error paths. Rewrite the 5 most common error messages.

**Structural fix:** Establish an error message style guide as part of the product's voice documentation. Require error messages in code review — "what does the user see when this fails?" should be a standard review question.

---

## New patterns not propagated

**What it looks like:** A commit introduces a new pattern (a utility function, a shared component, a service abstraction) but only uses it in one place. The rest of the codebase continues using the old pattern.

**What it signals:** Consistency problem. The team (or the AI) solves a problem once but does not apply the solution everywhere. This creates two patterns for the same thing, which confuses future developers and AI agents about which approach is "correct."

**Why it matters:** Inconsistency is a multiplier of confusion. Every inconsistency forces every future developer to ask "which way should I do this?" and then either pick randomly or add a third pattern. Codebases with inconsistent patterns grow complexity faster than codebases with consistent ones.

**One-line fix:** When the scan finds a new pattern introduced recently, schedule an AFK item to apply it to all existing instances.

**Structural fix:** When introducing a new pattern, apply it everywhere in the same PR. If that is too large, create a follow-up issue immediately and assign it. Add lint rules where possible to enforce the new pattern and flag the old one.

---

## Carryover items keep reappearing

**What it looks like:** The same work item appears as carryover three or more days in a row. It is "in progress" but never completes.

**What it signals:** The work is either harder than estimated, blocked by something unstated, or not actually a priority despite appearing in the plan. It may also be the wrong granularity — too large to finish in a day, too vague to make progress on.

**Why it matters:** Persistent carryover demoralizes. It turns the daily plan from a fresh assessment into a stale reminder of unfinished work. It also indicates estimation or scoping problems.

**One-line fix:** Stop carrying it over. Either break it into a smaller first step that can be completed today, identify and resolve the blocker, or explicitly drop it from the plan with a note on why.

**Structural fix:** Time-box carryover items: if an item has been in the plan for 3 days, it must be either completed, broken down, or removed on day 4. Track the reasons for persistent carryover — they reveal systematic estimation problems or hidden blockers.

---

## The codebase scan produces no findings

**What it looks like:** The scan comes back clean. No stale issues, no TODOs, no missing tests, no lint warnings, no security alerts.

**What it signals:** Either the codebase is genuinely healthy (rare and worth celebrating) or the scan is not looking in the right places. Check whether the project has issues in a different tracker, uses a different test framework than expected, or has lint rules configured to suppress rather than report.

**Why it matters:** An empty scan is suspicious. Most codebases have something to improve. If the scan finds nothing, the plan should say so explicitly and suggest deeper investigation.

**One-line fix:** Run the full audits from production-ready, security-review, and vibe-code-antipatterns. They go deeper than the daily standup's quick checks.

**Structural fix:** If the codebase is genuinely healthy, shift the plan toward feature work and proactive improvements rather than reactive fixes. This is the best possible outcome.
