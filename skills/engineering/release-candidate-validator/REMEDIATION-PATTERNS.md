# REMEDIATION-PATTERNS.md

Standard fix patterns for each validation dimension. For every finding, this is the how — the concrete steps, the common mistakes, and how to verify the fix is complete. Patterns focus on the release-level fixes, not the code-level fixes (see the production-ready skill for operational remediation).

---

## Scope integrity patterns

### Removing a stowaway commit

**The pattern:** Identify the stowaway commit and remove it from the release branch without disrupting the other changes. The approach depends on where the stowaway is in the commit history.

**If the stowaway is the most recent commit:**
```
git revert <stowaway-sha>
```

**If the stowaway is buried in the history:**
1. Create a new branch from the base (last release)
2. Cherry-pick only the intended commits from the current release branch
3. Verify the new branch has the correct scope
4. Replace the release branch with the new one

**Common mistakes:**
- Using `git rebase -i` to drop the commit on a shared release branch — this rewrites history and breaks anyone else working on the branch
- Reverting a stowaway that other commits depend on — the revert breaks those dependent commits. In this case, the dependent commits need to be reverted too, or the stowaway needs to be accepted and validated
- Not re-running tests after removing the stowaway — the remaining changes may depend on code the stowaway introduced

**How to verify:** `git log <base>...<head> --oneline` shows only intended commits. Tests pass on the updated branch. The diff matches the expected release scope.

### Adding a missing commit

**The pattern:** Cherry-pick the missing commit onto the release branch. Verify it's complete and doesn't bring unintended changes.

**Steps:**
1. Identify the commit SHA on the source branch
2. Check if the commit is self-contained or part of a series
3. If part of a series, cherry-pick all commits in order
4. `git cherry-pick <sha>` (or `git cherry-pick <sha1> <sha2>` for a series)
5. Run tests on the release branch

**Common mistakes:**
- Cherry-picking one commit from a multi-commit change — the cherry-pick applies cleanly but the behavior is wrong because it depends on context from the other commits
- Not verifying the cherry-pick against the original — use `git diff <original-sha> <cherry-pick-sha>` to confirm the content is identical (commit SHAs will differ)
- Cherry-picking a commit that was later amended or superseded on the source branch — you get the old version, not the final version

**How to verify:** The cherry-picked change behaves identically to the original on the source branch. Tests pass. The release scope now matches the plan.

---

## Breaking change patterns

### Writing a breaking change changelog entry

**The pattern:** A breaking change entry has four parts: what changed, what the old behavior was, what the new behavior is, and what the consumer needs to do.

**Template:**
```
### Breaking Changes

- **[Component/endpoint/feature name]:** [What changed].
  Previously, [old behavior]. Now, [new behavior].
  **Migration:** [Specific steps to update].
```

**Example:**
```
### Breaking Changes

- **`/api/users` response:** The `legacy_id` field has been removed.
  Previously, the response included `legacy_id` (integer) alongside `id` (UUID).
  Now, only `id` (UUID) is returned.
  **Migration:** Replace any usage of `legacy_id` with `id`. If your system
  stores `legacy_id` values, use the `/api/users/migrate-ids` endpoint
  (available since v2.3) to map legacy IDs to UUIDs before upgrading.
```

**Common mistakes:**
- Writing for developers instead of consumers: "Removed legacy_id from the UserSerializer" vs. "The `legacy_id` field has been removed from the `/api/users` response"
- Omitting the migration step: telling consumers what changed but not what to do about it
- Burying breaking changes under "Fixed" or "Changed" instead of a dedicated "Breaking Changes" section

**How to verify:** A consumer who reads only the changelog entry has enough information to update their code without reading the source or asking the team.

### Writing a migration guide

**The pattern:** For breaking changes that require more than a one-line fix, write a standalone migration guide. The guide walks the consumer through the full upgrade path.

**Structure:**
1. **Overview** — one paragraph: what changed and why
2. **Who is affected** — which consumers need to act (API users? SDK users? Self-hosted operators?)
3. **Step-by-step migration** — numbered steps with before-and-after code examples
4. **Configuration changes** — any config keys that changed, with old and new values
5. **Testing your migration** — how to verify the migration worked
6. **Timeline** — when the old behavior will be removed (if this is a deprecation) or when this version becomes required

**Common mistakes:**
- Assuming context: "Update your auth config" — which file? which key? what value?
- Missing the before-and-after: consumers need to see what the old code looks like and what the new code looks like, side by side
- Not covering edge cases: the guide covers the standard case but not the consumer who was using the feature in an unusual way

**How to verify:** Hand the migration guide to someone who hasn't seen the code changes. Can they follow it successfully? If they need to ask questions, the guide is incomplete.

---

## Dependency patterns

### Reviewing a major version bump

**The pattern:** When a dependency jumps a major version, systematically review the impact on the project.

**Steps:**
1. Read the dependency's changelog or release notes for the major version
2. List every breaking change in the dependency
3. For each breaking change, search the project for affected usage: `grep -r "affected_function" --include="*.ts"` (or equivalent)
4. For each affected usage, verify it's been updated to the new API
5. Run the test suite — but also manually verify any usage that isn't covered by tests

**Common mistakes:**
- Reading the dependency's changelog but not checking transitive effects — the dependency's dependency may have changed too
- Assuming tests cover all usage — tests may exercise the happy path but not edge cases that the new version handles differently
- Not checking for deprecated APIs that still work but will be removed in the next major version — update them now while the context is fresh

**How to verify:** Every breaking change in the dependency is accounted for in the project. Tests pass. No deprecated API warnings in the build output.

### Auditing a new dependency

**The pattern:** Before accepting a new dependency into a release, evaluate it on five dimensions: necessity, maintenance, security, license, and weight.

**Checklist:**
1. **Necessity:** What problem does this solve? Can an existing dependency or the standard library solve it instead? If the new dependency wraps a 10-line implementation, inline the logic instead.
2. **Maintenance:** When was the last commit? Are issues responded to? Is there more than one maintainer? Check the GitHub pulse or equivalent.
3. **Security:** Run `npm audit` / `pip audit` / `cargo audit`. Check the repository for a security policy. Does the package have a history of CVEs?
4. **License:** Is the license compatible with the project's license? MIT, Apache-2.0, and BSD are generally safe. GPL, AGPL, SSPL have copyleft implications. Check transitive dependencies' licenses too.
5. **Weight:** What's the install size? How many transitive dependencies does it add? For frontend packages, what's the bundle size impact?

**Common mistakes:**
- Evaluating only the direct dependency, not its transitive tree — a small utility package that pulls in 50 transitive dependencies is not small
- Checking the license of the direct dependency but not its transitive dependencies — a MIT package that depends on a GPL package may have copyleft implications
- Not checking for alternative implementations already in the project's dependency tree — many utility packages overlap (lodash vs. underscore vs. ramda, moment vs. date-fns vs. dayjs)

**How to verify:** The dependency is justified, maintained, secure, licensed appropriately, and doesn't add disproportionate weight. The evaluation is documented (even briefly) so the next developer who sees it in the lockfile understands why it's there.

---

## Migration safety patterns

### Making a migration backward-compatible

**The pattern:** Ensure the migration can run before the new code is deployed, and the old code continues to work with the new schema. This enables zero-downtime deploys and safe rollbacks.

**Safe operations (old code works with new schema):**
- Adding a new table
- Adding a nullable column
- Adding a column with a default value (PostgreSQL 11+, MySQL 8.0+)
- Adding an index (use `CONCURRENTLY` on PostgreSQL)
- Adding a new enum value (at the end, if the ORM supports it)

**Unsafe operations (old code breaks with new schema):**
- Renaming a column — old code queries the old name
- Removing a column — old code queries the removed column
- Changing a column type — old code may not handle the new type
- Adding a NOT NULL column without a default — existing rows violate the constraint
- Removing a table — old code queries the removed table

**Making unsafe operations safe (expand-contract):**
1. **Expand:** Add the new column/table alongside the old one
2. **Deploy code:** Update the application to write to both old and new
3. **Backfill:** Copy data from old to new
4. **Switch reads:** Update the application to read from the new
5. **Contract:** Remove the old column/table in a later release

Each step is independently deployable and reversible.

**Common mistakes:**
- Running the expand and contract in the same migration — this defeats the purpose; if the code deploy fails, the old column is already gone
- Backfilling in the migration itself for large tables — this locks the table. Use a background job or batch script instead
- Forgetting the dual-write step — new records don't have data in the new column, causing null errors when the read switch happens

**How to verify:** Deploy the migration to a staging environment running the old code. Verify the old code still works. Then deploy the new code. Verify the new code works. Then roll back the new code. Verify the old code still works with the migrated schema.

### Documenting new configuration

**The pattern:** For each new environment variable or configuration key, add documentation in three places: the deployment configuration template, the changelog, and inline where the configuration is read.

**Template for `.env.example` or equivalent:**
```
# Description of what this controls
# Required: yes/no
# Default: value (if any)
# Example: example_value
NEW_CONFIG_KEY=default_value
```

**Template for changelog:**
```
### Configuration

- **`NEW_CONFIG_KEY`** (optional, default: `default_value`): Description
  of what this controls and when you'd change it.
```

**Common mistakes:**
- Adding the configuration to the code but not to the deployment template — the deployment engineer has to read the code to find out what config is needed
- No default value for optional configuration — the code crashes if the key is missing, even though it's documented as optional
- Documenting the key but not the valid values or format — "Set `DATABASE_URL`" without explaining the expected format (`postgres://user:pass@host:port/db`)

**How to verify:** A deployment engineer who has never seen this release can set up the configuration correctly using only the documentation. No configuration-related crashes on deploy.

---

## Changelog patterns

### Writing a consumer-facing changelog

**The pattern:** Transform developer-centric descriptions into consumer-centric descriptions. The consumer cares about what changed for them, not how it was implemented.

**Transformation examples:**

| Developer-centric (avoid) | Consumer-centric (use) |
|---|---|
| Refactored auth middleware to use strategy pattern | Login now supports SAML SSO in addition to OAuth |
| Fixed race condition in order processing | Orders placed during high traffic no longer occasionally show incorrect totals |
| Updated React from 17 to 18 | Dashboard loads ~30% faster on initial page load |
| Migrated user table to use UUIDs | User IDs are now UUIDs instead of integers. See migration guide. |
| Removed deprecated endpoint | The `/v1/legacy-search` endpoint has been removed. Use `/v2/search` instead. |

**Structure for a version entry:**
```
## [version] - YYYY-MM-DD

### Breaking Changes
(Changes that require consumer action — always first, always prominent)

### Added
(New features and capabilities)

### Changed
(Changes to existing behavior)

### Fixed
(Bug fixes)

### Deprecated
(Features marked for future removal, with timeline and replacement)

### Security
(Vulnerability fixes — even if also listed elsewhere, call them out here)
```

**Common mistakes:**
- Auto-generating from commit messages — commit messages are for code reviewers, changelog entries are for users
- Omitting the date — consumers need to know when the version was released, not just what changed
- Mixing categories — a bug fix listed under "Added" or a breaking change under "Fixed" misleads consumers about risk

**How to verify:** Read the changelog aloud as if you're telling a customer what changed. Does it make sense without context? Can they decide whether to upgrade based on reading it?

---

## Version hygiene patterns

### Correcting a semver violation

**The pattern:** If the version number doesn't match the changes (e.g., a breaking change shipped as a minor bump), correct the version before release.

**Steps:**
1. Determine the correct version: does the RC contain breaking changes? (Major bump.) New features only? (Minor bump.) Bug fixes only? (Patch bump.)
2. Update the version in all locations (see checklist item "version updated in all required locations")
3. Update the git tag
4. Update the changelog header to reflect the correct version
5. Communicate the version change to anyone who's been testing or referencing the old version number

**If the incorrect version was already published:**
- If it was published to a package registry: publish a corrected version at the right semver, and yank or deprecate the incorrect version if possible
- If consumers have already upgraded: the damage of the semver violation is done — issue a patch release with the breaking change properly versioned

**Common mistakes:**
- Changing the version number without updating the tag — the tag still points to the old version
- Updating the version in package.json but not in the lockfile — run the package manager's install command to regenerate the lockfile
- Not communicating the version change — QA, staging, and release documentation may reference the old version

**How to verify:** `grep -r "old_version"` returns no results. The tag matches the version. The lockfile is regenerated. The changelog header is correct.

---

## Rollback patterns

### Testing rollback before release

**The pattern:** Before shipping the RC, verify that rolling back to the previous version works. This is especially critical when the RC includes database migrations or stateful changes.

**Steps:**
1. Deploy the RC to a staging environment
2. Run any migrations included in the RC
3. Exercise the application (create data, process transactions, use new features)
4. Roll back the application code to the previous version
5. Roll back the migrations (if applicable)
6. Verify the application works: health checks pass, key flows work, no data corruption
7. Document the rollback time (how long from "decide to roll back" to "system is healthy")

**Common mistakes:**
- Testing rollback on an empty database — the rollback may work fine with no data but fail when there are records that reference the new schema
- Rolling back code but not migrations — the old code runs against the new schema, which may or may not work depending on the migration
- Not testing the full rollback sequence — testing code rollback and migration rollback separately doesn't catch the interaction between them

**How to verify:** The staging environment is healthy after the full rollback sequence. No data was lost. No errors in logs. The rollback time is documented and acceptable.

### Documenting cross-service rollback order

**The pattern:** When a release spans multiple services, document the order in which services must be rolled back to avoid inconsistent state.

**Template:**
```
## Rollback Order

If a rollback is needed after deploying this release:

1. Roll back [service-A] first — this removes the dependency on
   [service-B]'s new endpoint
2. Roll back [service-B] — now safe because [service-A] no longer
   calls the new endpoint
3. Verify: [specific health checks or smoke tests to confirm
   the system is consistent]

**Do NOT roll back [service-B] before [service-A]** — [service-A]
will call an endpoint that no longer exists, causing [specific failure].
```

**Common mistakes:**
- Assuming services can be rolled back in any order — if service A calls service B's new endpoint, rolling back B first breaks A
- Not documenting the rollback order — during an incident at 3 AM, the on-call engineer shouldn't have to reverse-engineer the dependency graph
- Forgetting to include verification steps — rolling back without verifying leaves the team unsure if the rollback worked

**How to verify:** The rollback order is documented in the release notes or runbook. The team has reviewed the order and confirmed it's correct. Ideally, the rollback has been tested in staging with the documented order.
