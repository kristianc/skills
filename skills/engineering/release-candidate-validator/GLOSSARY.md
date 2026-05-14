# GLOSSARY.md

The vocabulary the skill uses. Precise terms prevent ambiguity in release discussions. "There might be breaking changes" is vague. "The `/users` endpoint removed the `legacy_id` field, which is a breaking change for API consumers on v2" is actionable.

## Terms

### Release candidate (RC)

A specific build — identified by a git tag, branch, or commit SHA — that is proposed for production. An RC has a defined scope (the set of changes since the last release), a version number, and an intended audience. The RC is the unit of validation: everything in it ships together, so everything in it must be validated together.

**Common misuse:** Treating "the main branch" as the RC. The RC is a snapshot — a specific set of changes that have been cut and frozen. If main keeps moving after the cut, those new changes are not part of this RC.

### Breaking change

Any change that requires consumers — users, API clients, downstream services, or operators — to modify their code, configuration, data, or workflow to continue working after the upgrade. Breaking changes include: removed API endpoints or fields, changed default values, renamed configuration keys, schema changes that require migration, dropped support for a runtime or platform, and changed behavior of existing functions.

**The key test:** Can a consumer upgrade to this version without changing anything on their side? If no, it's a breaking change.

**Common misuse:** Only counting removed features as breaking. A changed default is breaking if consumers relied on the old default. A new required field is breaking. A stricter validation rule is breaking. The test is always: does existing code/config/data still work unchanged?

### Scope integrity

The release contains exactly the changes intended — nothing more, nothing less. No stowaway commits (changes that were merged but shouldn't be in this release), no missing commits (changes that were intended for this release but aren't included), and no cherry-pick mistakes (partial application of a multi-commit change).

**Example:** A release branch was cut from main. After the cut, a developer merged a risky refactor into main. If main was then merged back into the release branch (instead of cherry-picking only the intended fixes), the risky refactor is now a stowaway in the RC.

**Common misuse:** Assuming that because the release branch was cut correctly, scope integrity is guaranteed. Merges, cherry-picks, and rebases after the cut can all introduce scope problems.

### Rollback viability

The ability to revert the release to the previous version and return the system to a working state. A release with high rollback viability can be reverted with a single command and no data loss. A release with low rollback viability requires manual intervention, data fixups, or coordination across services.

**Factors that reduce rollback viability:**
- Irreversible database migrations (dropped columns, changed types without expand-contract)
- Stateful changes (new data formats written to storage that the old version can't read)
- External integrations (new webhook registrations, third-party state changes)
- Multi-service coordination (service A depends on service B's new API, so rolling back A requires rolling back B too)

**Common misuse:** Assuming all releases are rollback-safe because "we can just deploy the old version." Deploying old code against a new database schema, new data formats, or new external state is not a rollback — it's a new failure mode.

### Dependency risk

The risk introduced by adding, updating, or removing a dependency. New dependencies add attack surface and supply-chain risk. Major version updates may include breaking changes in the dependency itself. Removed dependencies may break functionality that relied on them transitively.

**Risk factors:**
- **Major version bump** — likely includes breaking changes in the dependency's API
- **New dependency** — adds supply-chain risk, increases install size, new license to evaluate
- **Unmaintained dependency** — no recent commits, open CVEs, archived repository
- **Transitive change** — a dependency of a dependency changed, potentially unnoticed

**Common misuse:** Only reviewing direct dependency changes. Transitive dependency updates (via lockfile changes) can introduce vulnerabilities or breaking behavior without any explicit change in the project's dependency declarations.

### Semver (Semantic Versioning)

A versioning convention where version numbers carry meaning: `MAJOR.MINOR.PATCH`. Major increments signal breaking changes. Minor increments signal new functionality that's backward-compatible. Patch increments signal bug fixes with no API changes.

**The contract:** A consumer should be able to update within a minor or patch version without breaking. A major version update requires reading the migration guide.

**Common misuse:** Incrementing patch for a breaking change because the change is "small." Semver communicates risk to consumers. A one-line change that removes a public API field is a major version bump, not a patch — the size of the diff is irrelevant, the impact on consumers is what matters.

### Changelog

A human-readable record of what changed in each version, written for the people who consume the release — not the people who built it. A good changelog tells a consumer: what's new, what's fixed, what's changed, what's removed, and what they need to do differently.

**Common misuse:** Auto-generating the changelog from commit messages. Commit messages are written for developers reviewing code. Changelog entries are written for users deciding whether to upgrade. "Refactor auth middleware to use strategy pattern" is a commit message. "Fixed: Login no longer fails when using SSO with certain identity providers" is a changelog entry.

### Migration guide

Documentation that tells a consumer exactly what to change when upgrading across a breaking change. A migration guide includes: what changed, why it changed, what the old behavior was, what the new behavior is, and step-by-step instructions for updating code, configuration, or data.

**The completeness test:** Can a consumer follow the migration guide and successfully upgrade without reading the source code or asking the team for help? If not, the guide is incomplete.

**Common misuse:** A migration guide that says "the API has changed, see the docs." That's a pointer, not a guide. A guide includes the specific changes, the before-and-after, and the steps.

### Stowaway commit

A commit that is present in the release candidate but was not intended to be part of this release. Stowaways enter through: merging main into a release branch instead of cherry-picking, rebasing onto a branch that moved, or accidentally including a feature branch that wasn't ready.

**Why it matters:** Stowaways bypass the release process. They haven't been reviewed in the context of this release, may not be tested with the other changes in the release, and may introduce risk that the team didn't account for.

### Cherry-pick safety

Whether a commit can be cleanly applied to a release branch without dragging in unrelated changes or breaking functionality that depends on commits not in the branch. A safe cherry-pick is self-contained. An unsafe cherry-pick depends on other commits that aren't in the release branch, creating a partial application that may compile but behave incorrectly.

**Common misuse:** Assuming a cherry-pick that applies cleanly (no merge conflicts) is safe. A clean application only means the text doesn't conflict. The behavior may still be wrong if the cherry-picked commit depends on context (other functions, configuration, schema) that's only in the source branch.
