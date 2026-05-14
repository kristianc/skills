# SCORING-RUBRIC.md

How to score each validation dimension from the audit. Each dimension gets rated 1-5 on three axes: **coverage**, **quality**, and **risk**. The scores drive what blocks the release, what's risky but shippable, and what's clear.

The point of scoring is a ship/no-ship decision. A release validation needs to answer: "Can we ship this RC?" The scores tell you where the real problems are.

## Axis 1: Coverage

How many items in the dimension are addressed?

**5 — Comprehensive.** Every item in the checklist for this dimension is addressed. The team has verified each one.

- Scope integrity: every commit traced to a planned change, no stowaways, no missing items, cherry-picks verified
- Breaking changes: all identified, all documented, migration guides written for complex ones
- Dependencies: all changes enumerated, major bumps reviewed, new deps justified, audit clean

**4 — Mostly covered.** Most items are addressed. Remaining gaps are known and have a rationale.

- Scope integrity: all commits reviewed, one commit is unplanned but low-risk and accepted by the team
- Breaking changes: all identified and documented, migration guide deferred for one that only affects internal consumers
- Dependencies: direct changes reviewed, lockfile changes large but spot-checked

**3 — Partially covered.** About half the items are addressed. Some gaps are deliberate, others are oversights.

- Scope integrity: commits were reviewed but only by scanning titles, not verifying against the release plan
- Breaking changes: some are documented, others were discovered during validation and aren't in the changelog yet
- Dependencies: direct changes noted but major version bumps weren't reviewed against the dependency's changelog

**2 — Minimal.** Only the most obvious items are addressed.

- Scope integrity: "we cut the branch from main, so it should be fine" — no commit-level review
- Breaking changes: the obvious ones are in the changelog but no systematic review of the diff was done
- Dependencies: "nothing major changed" — lockfile changes weren't reviewed

**1 — Absent.** The dimension hasn't been addressed.

- Scope integrity: no review of what's in the RC — it's whatever was on the branch
- Breaking changes: no review for backward compatibility — "we'll see if anyone reports issues"
- Dependencies: dependency changes weren't considered as part of the release

---

## Axis 2: Quality

How well are the addressed items handled?

**5 — Exemplary.** Addressed items are thorough, accurate, and set up consumers for success.

- Changelog: entries are consumer-facing, categorized correctly, breaking changes include before-and-after examples, migration guides are step-by-step with code samples
- Version hygiene: semver is correct, version updated in every location, tag points to the right commit, release notes are published
- Rollback: rollback tested in staging, multi-service coordination documented, irreversible side effects identified with mitigation plans

**4 — Solid.** Addressed items are correct and useful. Minor improvements possible.

- Changelog: entries are clear and categorized, breaking changes documented, migration guide exists but could use more examples
- Version hygiene: semver correct, version consistent across files, tag exists
- Rollback: rollback procedure documented, migration rollbacks verified, one cross-service dependency noted

**3 — Functional.** Addressed items exist but have visible weaknesses.

- Changelog: entries exist but some are developer-facing ("refactored X"), categorization is mostly right, breaking changes listed but without migration guidance
- Version hygiene: version number bumped but inconsistent in one location (README badge still shows old version)
- Rollback: team can articulate the rollback procedure but hasn't tested it for this specific release

**2 — Fragile.** Items technically exist but are unreliable or misleading.

- Changelog: entries are auto-generated from commit messages, no categorization, breaking changes not called out
- Version hygiene: version bumped as patch but the release includes a breaking change
- Rollback: "we'll just deploy the old version" without considering migration state

**1 — Harmful.** Items exist but are worse than nothing — they give false confidence.

- Changelog: entries describe the wrong behavior or promise features that aren't in the RC
- Version hygiene: version not bumped at all, or bumped to a number that conflicts with a previous release
- Rollback: team believes rollback is safe but an irreversible migration makes it impossible

---

## Axis 3: Risk

What is the blast radius if this dimension is wrong?

**5 — Contained.** If this dimension has a problem, the impact is minimal — a minor inconvenience for a few users, no data loss, easy to fix.

- Scope has a low-risk stowaway commit (a typo fix, a comment update) — no functional impact
- A minor changelog entry is miscategorized — consumers notice but aren't harmed
- A version is inconsistent in a README badge — cosmetic issue

**4 — Limited.** If this dimension has a problem, some consumers are affected but can recover easily.

- A non-breaking API addition is undocumented — consumers can discover it later
- A deprecated feature lacks a removal timeline — consumers aren't surprised by removal in this release, but will be eventually
- A minor dependency update introduces a subtle behavior change — affects edge cases

**3 — Significant.** If this dimension has a problem, many consumers are affected or trust is damaged.

- A breaking change is in the diff but not in the changelog — consumers upgrade and break
- A migration isn't backward-compatible — zero-downtime deploy fails and the team scrambles
- Scope includes an untested feature that causes errors for a subset of users

**2 — Severe.** If this dimension has a problem, most consumers are affected and recovery is expensive.

- A semver-violating release (breaking change shipped as patch) breaks consumers who auto-update
- An irreversible migration with no rollback path means the only option is a forward fix under pressure
- A stowaway commit introduces a security vulnerability that reaches all users

**1 — Catastrophic.** If this dimension has a problem, the release causes widespread harm with no clear recovery.

- Data loss from an irreversible migration that corrupts existing records
- A dependency with a critical CVE reaches production in a release that can't be rolled back
- A multi-service release with no rollback coordination leaves the system in an inconsistent state that requires manual intervention across services

---

## Using the scores

### Release decision matrix

The combination of scores determines the release decision for each dimension:

| Decision | Condition |
|----------|-----------|
| **Blocking** | Risk score of 1 or 2 on any dimension. The RC is too risky to ship — a problem in this dimension would be severe or catastrophic and recovery is unclear. |
| **Risky** | Risk score of 3, OR coverage below 3, OR quality below 3 on a dimension with risk 4+. The RC can ship, but the team is accepting known risk. Document the risk and set a remediation deadline. |
| **Clear** | Risk score of 4-5 AND coverage of 3+ AND quality of 3+. The dimension is validated. There may be improvements to make, but nothing that should delay the release. |
| **Solid** | All three scores at 4+. This dimension is well-handled. Acknowledge it. |

### Distinguishing "not ready" from "ready with known risks" from "ready"

**Not ready (blocking):** There are problems that would cause serious harm if the RC ships. These must be fixed before releasing. Examples: undocumented breaking change in a public API, irreversible migration with no rollback path, semver violation on a package with auto-updating consumers, critical CVE in a new dependency.

**Ready with known risks:** The RC is functional and validated, but there are gaps the team is aware of and accepting. The risk is documented and there's a plan. Examples: changelog entries are developer-facing (consumers can still read the diff), a minor version inconsistency in docs (confusing but not breaking), dependency audit shows a low-severity CVE that doesn't affect the project's usage.

**Ready:** Every dimension has adequate coverage and quality, and the risk is contained. The RC can ship with confidence. There's always room for improvement, but nothing that should delay the release.

### Presenting the assessment

Present the assessment as a summary table, then detail the blockers and risks:

```
| Dimension              | Coverage | Quality | Risk | Status   |
|------------------------|----------|---------|------|----------|
| Scope integrity        |    4     |    4    |   4  | Clear    |
| Breaking changes       |    3     |    2    |   2  | Blocking |
| API surface            |    4     |    4    |   5  | Solid    |
| Dependency changes     |    3     |    3    |   3  | Risky    |
| Migration safety       |    4     |    4    |   4  | Clear    |
| Changelog completeness |    3     |    3    |   3  | Risky    |
| Version hygiene        |    5     |    5    |   5  | Solid    |
| Rollback viability     |    4     |    4    |   4  | Clear    |
```

Then for each blocking and risky dimension, detail the specific findings and remediations. For clear and solid dimensions, briefly acknowledge what's working.

The goal is a ship/no-ship conversation — not a grade. The team should leave knowing exactly what must be fixed, what they're accepting, and what's already solid.
