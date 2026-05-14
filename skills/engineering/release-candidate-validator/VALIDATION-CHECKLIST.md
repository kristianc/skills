# VALIDATION-CHECKLIST.md

The deep reference for the validate step (Step 2). Each dimension lists the items to check, what each means concretely, how to verify it against the RC diff, what "clear" looks like vs. what blocks the release, and the severity if missed. Walk every applicable dimension for the release candidate being validated.

---

## 1. Scope integrity

The RC contains exactly the intended changes — nothing snuck in, nothing was left out.

### Every commit in the RC was intended for this release

**What it means:** Each commit between the base (last release) and the head (this RC) was deliberately included. No stowaway commits from unrelated feature branches, no accidental merges from main after the branch cut, no work-in-progress that got swept in.

**How to check:** Run `git log <base>...<head> --oneline` and review every commit. Cross-reference against the issue tracker, PR list, or release plan. For each commit, ask: "Was this supposed to ship in this version?" Flag any commit that doesn't map to a planned change.

**Clear:** Every commit traces to a planned change. The team can explain why each commit is in the release.

**Blocks if missing:** A stowaway commit that introduces untested behavior, incomplete features, or risky changes that bypass the release review process.

### No expected changes are missing

**What it means:** Every change that was planned for this release is actually present in the diff. No forgotten cherry-picks, no PRs that were approved but never merged, no changes that were reverted without the team realizing.

**How to check:** Compare the release plan (tickets marked for this version, milestone in the issue tracker) against the actual commits. For each planned change, verify it appears in the diff. Check for reverts: `git log <base>...<head> --grep="revert"` — a revert may have undone a planned change.

**Clear:** Every planned change is in the diff. Any planned change that was deferred is documented and removed from the release scope.

**Blocks if missing:** A missing change that users or stakeholders are expecting. Shipping a release that claims to include a fix but doesn't is worse than delaying the release.

### Cherry-picks are complete and safe

**What it means:** If changes were cherry-picked onto a release branch, each cherry-pick is complete — not a partial application of a multi-commit change — and doesn't depend on context that's only in the source branch.

**How to check:** For each cherry-pick, find the original commit(s) in the source branch. Check if the original was part of a series (e.g., a multi-commit PR). If it was, verify all commits in the series were cherry-picked. Run the tests against the release branch to confirm the cherry-picks don't break in isolation.

**Clear:** Every cherry-pick maps to a complete, self-contained change. Tests pass on the release branch.

**Blocks if missing:** A partial cherry-pick that compiles but behaves incorrectly — e.g., a bug fix cherry-picked without the helper function it calls, which was added in a different commit.

---

## 2. Breaking changes

Every change that requires consumers to modify their code, config, or workflow is identified, documented, and versioned correctly.

### All breaking changes are identified

**What it means:** The diff has been reviewed for changes that break backward compatibility. This includes removed API endpoints or fields, changed function signatures, renamed configuration keys, changed default values, stricter validation, dropped platform support, and changed error codes or formats.

**How to check:** Review the full diff (`git diff <base>...<head>`) for: removed exports, removed or renamed fields in API responses, changed function parameters, new required arguments, changed default values, removed feature flags, changed error response shapes. Search for deleted public functions, renamed config keys, and removed environment variables.

**Clear:** Every breaking change is listed. The team can articulate who is affected and what they need to change.

**Blocks if missing:** An unidentified breaking change that reaches consumers without warning. They upgrade, their integration breaks, and they file a bug or lose trust.

### Breaking changes are documented in the changelog

**What it means:** Each breaking change has a changelog entry under a "Breaking Changes" section that describes: what changed, what the old behavior was, what the new behavior is, and what the consumer needs to do.

**How to check:** Read the changelog for this version. For each identified breaking change, verify there's a corresponding entry. The entry should be actionable — not "Changed auth flow" but "The `/auth/token` endpoint now requires a `client_id` parameter. Requests without it return 400. Add `client_id` to your token requests."

**Clear:** Every breaking change has a specific, actionable changelog entry.

**Blocks if missing:** A breaking change without documentation is a silent breaking change — the worst kind.

### Migration guide exists for non-trivial breaking changes

**What it means:** For breaking changes that require more than a one-line fix, a migration guide explains the full upgrade path with before-and-after code examples, configuration changes, and data migration steps.

**How to check:** For each breaking change, assess complexity. A renamed field needs a changelog entry. A new authentication flow needs a migration guide with code examples, configuration changes, and a testing procedure. Check that the guide is linked from the changelog.

**Clear:** Non-trivial breaking changes have a migration guide that a consumer can follow independently.

**Blocks if missing:** A complex breaking change without a migration guide will generate support requests, slow adoption, and erode trust. The effort to write the guide is always less than the effort to support users who don't have one.

---

## 3. API surface

Public interfaces — REST endpoints, GraphQL schemas, SDK methods, CLI commands, event payloads — are reviewed for unintended changes.

### No accidental API changes

**What it means:** Every change to the public API surface was intentional. Fields weren't accidentally removed by a refactor. Response shapes didn't change as a side effect of an internal restructuring. New required parameters weren't introduced without being flagged as breaking.

**How to check:** Diff API-related files: route definitions, controller/handler functions, GraphQL schema files, OpenAPI/Swagger specs, SDK public exports, CLI command definitions. For each change, ask: "Does this change what an external consumer sees?" If the project has an OpenAPI spec, diff it: `git diff <base>...<head> -- **/openapi* **/swagger*`.

**Clear:** Every API change is intentional, documented, and versioned appropriately.

**Blocks if missing:** An accidental API change that breaks consumers. Especially dangerous for field removals and type changes, which cause runtime errors in consumers' code.

### New endpoints and fields are documented

**What it means:** Any new API surface added in this release is documented — in the API docs, the OpenAPI spec, or the changelog. Consumers can discover and use the new functionality without reading source code.

**How to check:** For each new endpoint, field, or parameter in the diff, verify it appears in the documentation or changelog. Check that examples are provided for non-obvious usage.

**Clear:** New API surface is documented and discoverable.

**Risky if missing:** Undocumented new API surface isn't a blocker (it doesn't break anything), but it means the feature was built and shipped without being usable. If no one knows it exists, it's wasted work.

### Deprecated features have a timeline

**What it means:** Features marked for deprecation in this release include: a deprecation notice (in code, docs, and changelog), the version when the feature will be removed, and what to use instead.

**How to check:** Search the diff for deprecation markers: `@deprecated`, `@Deprecated`, deprecation warnings in code, deprecation notices in docs. For each, check that the replacement is specified and the removal timeline is documented.

**Clear:** Deprecated features have a replacement, a timeline, and are communicated in the changelog.

**Risky if missing:** Deprecation without a timeline is indefinite — consumers have no urgency to migrate, and the team can never remove the feature without it being a surprise breaking change.

---

## 4. Dependency changes

New, updated, and removed dependencies are reviewed for risk — security, compatibility, license, and stability.

### All dependency changes are identified

**What it means:** Every change in the project's dependency declarations (package.json, requirements.txt, Cargo.toml, go.mod, etc.) and lockfile is enumerated. This includes direct dependency changes and transitive dependency changes visible in the lockfile.

**How to check:** Diff the dependency files: `git diff <base>...<head> -- **/package.json **/requirements*.txt **/Cargo.toml **/go.mod **/Gemfile **/pom.xml`. Diff the lockfile: `git diff <base>...<head> --stat -- **/package-lock.json **/yarn.lock **/Cargo.lock **/go.sum **/Gemfile.lock`. Categorize changes: new dependency, removed dependency, major update, minor update, patch update.

**Clear:** Every dependency change is listed with its change type and reason.

**Blocks if missing:** An unreviewed dependency change — especially a major version bump or a new dependency — can introduce breaking behavior, vulnerabilities, or license incompatibilities.

### Major version updates are reviewed for breaking changes

**What it means:** When a dependency jumps a major version (e.g., 3.x to 4.x), the dependency's own changelog has been reviewed for breaking changes that affect this project. The project's usage of the dependency has been verified against the new version's API.

**How to check:** For each major version bump, read the dependency's changelog or migration guide. Search the project for usage of APIs that the dependency deprecated or removed. Run the test suite — but don't rely solely on tests, since they may not cover all usage patterns.

**Clear:** Major version bumps are accompanied by a note explaining what changed in the dependency and confirming the project's usage is compatible.

**Blocks if missing:** A major dependency update that breaks a code path not covered by tests. The RC passes CI but fails in production on an edge case.

### New dependencies are justified

**What it means:** Each new dependency added in this release has a reason: what problem it solves, why an existing dependency or standard library can't solve it, and whether the dependency is actively maintained.

**How to check:** For each new dependency, check: Is the repository active (recent commits, responsive to issues)? What's the download count / adoption? What license does it use? How large is it (install size, transitive dependencies)? Does it overlap with an existing dependency?

**Clear:** New dependencies are justified, actively maintained, appropriately licensed, and don't duplicate existing functionality.

**Risky if missing:** An unjustified dependency adds supply-chain risk, install size, and maintenance burden. Not a blocker, but worth questioning.

### No known vulnerabilities introduced

**What it means:** The dependency changes don't introduce packages with known CVEs. Both direct and transitive dependencies are checked.

**How to check:** Run the package manager's audit command: `npm audit`, `pip audit`, `cargo audit`, `go vuln check`. Compare the audit results against the base version — new vulnerabilities introduced by this RC are the concern, not pre-existing ones (though those should be tracked separately).

**Clear:** No new vulnerabilities introduced, or new vulnerabilities are low-severity and documented as accepted risk.

**Blocks if missing:** A new high or critical severity CVE introduced by a dependency change in this RC.

---

## 5. Migration safety

Database migrations, configuration changes, and environment changes are safe to apply and safe to reverse.

### Migrations are reversible

**What it means:** Every database migration in the RC has a corresponding rollback. Destructive operations (dropping columns, changing types) use the expand-contract pattern or have a tested rollback strategy.

**How to check:** For each migration file in the diff, check for a `down` or `rollback` method. Check if any migration drops a column, drops a table, changes a column type, or deletes data. For destructive operations, verify the expand-contract pattern is being followed or a backup step is included.

**Clear:** Every migration has a tested rollback path. Destructive migrations use expand-contract.

**Blocks if missing:** An irreversible migration means the release cannot be rolled back without data loss or manual intervention. This is the single most common source of "we can't go back" situations.

### Migrations are backward-compatible with the running application

**What it means:** The migration can run while the current (pre-release) version of the application is still serving traffic. The old code works with the new schema, and the new code works with the old schema. This allows zero-downtime deploys where the migration runs before the code is updated.

**How to check:** For each migration, ask: "If the old application code runs against this new schema, does it break?" Adding a nullable column is safe. Adding a NOT NULL column without a default is not. Renaming a column breaks the old code that references the old name.

**Clear:** Migrations can run independently of the code deploy. The old code works with the new schema.

**Blocks if missing:** A migration that requires simultaneous code deployment means the deploy window has zero tolerance — if the code deploy fails after the migration runs, the old code doesn't work with the new schema and the system is down.

### New environment variables and configuration are documented

**What it means:** Any new environment variables, configuration keys, or infrastructure requirements introduced by this RC are documented with: the key name, what it controls, the default value (if any), whether it's required, and example values.

**How to check:** Search the diff for new `process.env`, `os.environ`, `os.Getenv`, configuration file reads, or environment variable references. Cross-reference against the deployment documentation or `.env.example` file.

**Clear:** New configuration is documented, has sensible defaults where possible, and the deployment runbook is updated.

**Blocks if missing:** A new required environment variable that isn't in the deployment configuration means the release fails on deploy. The deployment engineer sees a crash, not a helpful error.

---

## 6. Changelog completeness

The changelog accurately reflects every user-visible change in the RC.

### Every user-visible change has a changelog entry

**What it means:** Changes that affect what users see, do, or experience are reflected in the changelog. New features, bug fixes, changed behavior, performance improvements, and UI changes all warrant entries. Internal refactors with no user-visible effect do not.

**How to check:** Walk the diff. For each change, ask: "Would a user notice this?" If yes, check for a corresponding changelog entry. The entry should describe the change from the user's perspective, not the developer's.

**Clear:** Every user-visible change has a changelog entry written for the user, not the developer.

**Risky if missing:** Users who read the changelog to decide whether to upgrade miss a change that affects them. Not a blocker if the change is minor, but a blocker if it's a behavior change or breaking change.

### Changelog entries are categorized correctly

**What it means:** Entries are grouped under the right heading: Added (new features), Changed (behavior changes), Fixed (bug fixes), Removed (removed features), Deprecated (features marked for future removal), Security (vulnerability fixes). A bug fix listed under "Added" or a breaking change buried under "Fixed" misleads consumers.

**How to check:** Read each entry and verify it's under the correct category. Breaking changes should be under a "Breaking Changes" or "Changed" section, not hidden in "Fixed." Security fixes should be under "Security," not buried in "Fixed."

**Clear:** Entries are categorized correctly. Breaking changes and security fixes are prominently placed.

**Risky if missing:** Miscategorized entries cause consumers to misjudge the risk of upgrading. A breaking change hidden in "Fixed" will surprise someone.

### Changelog language is consumer-facing

**What it means:** Changelog entries are written for the people who use the software, not the people who built it. "Refactored the middleware pipeline" means nothing to a user. "Page load times improved by ~200ms on dashboard views" does.

**How to check:** Read each entry and ask: "Would a non-developer user or an API consumer understand this and know if it affects them?" Entries should describe the outcome, not the implementation.

**Clear:** A consumer can read the changelog and understand what changed, whether it affects them, and what (if anything) they need to do.

**Risky if missing:** Developer-facing changelog entries reduce the changelog's value as a communication tool. Consumers stop reading it, which means they stop noticing breaking changes too.

---

## 7. Version hygiene

The version number is correct and consistent, and the versioning convention communicates the right signal to consumers.

### Version number follows semver correctly

**What it means:** If the project uses semantic versioning, the version increment matches the changes: major for breaking changes, minor for new features, patch for bug fixes. A release with breaking changes must bump the major version. A release with only bug fixes should not bump the minor version.

**How to check:** Identify the version number in the RC. Review the changes: are there breaking changes? (Major bump required.) Are there new features? (Minor bump minimum.) Are there only bug fixes? (Patch bump.) Compare the required bump against the actual bump.

**Clear:** The version number accurately reflects the nature of the changes.

**Blocks if missing:** A breaking change shipped as a minor or patch version violates the semver contract. Consumers who auto-update within minor/patch ranges (e.g., `^1.2.0`) get broken without warning.

### Version is updated in all required locations

**What it means:** The version number is consistent across all files that declare it: package.json, setup.py, Cargo.toml, version constants in code, Docker image tags, Helm chart versions, and any documentation that references the current version.

**How to check:** Search for the old version number across the codebase: `grep -r "old_version"`. Check each location where it appears and verify it's been updated to the new version. Common missed locations: README badges, documentation references, Docker compose files, and CI/CD pipeline configurations.

**Clear:** The version number is consistent everywhere.

**Risky if missing:** Inconsistent versions cause confusion — "which version am I running?" — and can cause build or deployment failures if a version check expects a specific value.

### Git tag matches the version

**What it means:** The git tag for this RC matches the version declared in the project files. The tag points to the correct commit (the head of the RC), not an earlier commit.

**How to check:** Check if a tag exists: `git tag -l "v*"` or `git tag -l "*version*"`. Verify the tag points to the RC head: `git rev-parse <tag>` should match `git rev-parse HEAD` (or the RC branch head). Verify the tag name matches the version in project files.

**Clear:** The tag exists, matches the version, and points to the correct commit.

**Blocks if missing:** A missing or incorrect tag means the release is not reproducible. Consumers, CI pipelines, and package registries rely on tags to identify versions.

---

## 8. Rollback viability

The release can be safely reverted if something goes wrong after shipping.

### The previous version can run against the new state

**What it means:** If the release is rolled back (old code deployed), the old code works with whatever state changes the new release introduced — new database schema, new data formats, new cache entries, new queue messages.

**How to check:** For each stateful change in the RC (migrations, new data formats, new cache keys, new queue message shapes), ask: "If I deploy the old code, does it handle this new state?" A new nullable column is fine — the old code ignores it. A renamed column is not — the old code queries the old name and gets an error.

**Clear:** The old code is compatible with the new state. Rollback is a one-command operation.

**Blocks if missing:** A release that can't be rolled back is a one-way door. If it breaks, the only path forward is a hotfix under pressure — the most dangerous condition for writing code.

### Multi-service rollback is coordinated

**What it means:** If the release spans multiple services (e.g., a new API endpoint in the backend consumed by a new feature in the frontend), rolling back one service doesn't break the other. Services can be rolled back independently.

**How to check:** For each cross-service change, ask: "If I roll back service A but not service B, does B still work?" If the frontend calls a new endpoint that only exists in the new backend version, rolling back the backend breaks the frontend. The safe pattern: deploy the backend first (new endpoint exists but isn't called), then deploy the frontend. Rollback order: frontend first, then backend.

**Clear:** Services can be rolled back in any order without breaking each other, or the rollback order is documented.

**Risky if missing:** A multi-service release without coordinated rollback means rolling back one service can cascade into rolling back all services — increasing blast radius and recovery time.

### No irreversible side effects

**What it means:** The release doesn't trigger actions that can't be undone on rollback: sending emails, processing payments, registering webhooks with third parties, publishing to external feeds, or deleting external resources.

**How to check:** Review the diff for code that interacts with external systems: API calls to third parties, email/SMS sends, payment processing, webhook registration, DNS changes, CDN purges. Ask: "If we roll back after this runs, is the external state inconsistent?"

**Clear:** External interactions are either idempotent (safe to re-execute), gated behind feature flags (can be disabled without rollback), or documented as irreversible with a mitigation plan.

**Risky if missing:** An external side effect that can't be undone doesn't necessarily block the release, but the team should know about it. "If we roll back after sending these emails, users will have received a notification about a feature that no longer exists" is worth knowing before you ship.
