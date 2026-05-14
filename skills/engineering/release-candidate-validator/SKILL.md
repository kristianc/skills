---
name: release-candidate-validator
description: Validate whether a release candidate is safe to ship — breaking changes, dependency risks, migration safety, changelog completeness, scope integrity, and rollback viability. Use before cutting a release, before promoting RC to production, after a release branch is cut, or when reviewing a version bump.
---

# Release Candidate Validator

Systematically validate a release candidate before it ships. Not "does the code work" but "is this specific set of changes safe to release as a unit." The goal is to catch the problems that tests don't — undocumented breaking changes, stowaway commits, missing migrations, dependency risks, and rollback traps.

This skill covers the release-level dimensions that separate "all tests pass" from "this is safe to ship": scope integrity, breaking changes, dependency changes, migration safety, changelog completeness, version hygiene, and rollback viability.

## Glossary

Use these terms precisely. Full definitions in GLOSSARY.md.

- **Release candidate** — a specific build, tag, or branch cut intended for production. It has a defined scope of changes and a version number.
- **Breaking change** — any change that requires consumers to modify their code, configuration, data, or workflow to continue working.
- **Scope integrity** — the release contains exactly what was intended — no stowaway commits, no missing commits, no cherry-pick mistakes.
- **Rollback viability** — the release can be safely reverted to the previous version without data loss or state corruption.
- **Dependency risk** — the chance that a new, updated, or removed dependency introduces instability, vulnerabilities, or incompatibility.

## Process

### 1. Identify the RC

Establish what is being validated: a tagged commit, a release branch, a PR into main, or a set of commits since the last release. Ask the user if not obvious.

Determine:
- **Base:** the previous release (tag, commit, or branch) to diff against
- **Head:** the release candidate
- **Version:** the intended version number
- **Audience:** who consumes this release — internal teams, external customers, API consumers, package users

Use `git log <base>...<head>`, `git diff <base>...<head>`, and `git diff <base>...<head> --stat` to understand the full scope of changes. Do not rely on a single commit message — read the actual diff.

### 2. Validate

Walk each validation dimension against the checklist in VALIDATION-CHECKLIST.md. For each dimension — scope integrity, breaking changes, API surface, dependency changes, migration safety, changelog completeness, version hygiene, rollback viability — check every applicable item against the actual changes in the RC.

Use the Agent tool with `subagent_type=Explore` to walk the codebase. Search for API changes, migration files, dependency updates, changelog entries, version strings, and configuration changes.

Compare every finding against the changelog and release notes. A breaking change that's in the diff but not in the changelog is a documentation bug. A changelog entry with no corresponding diff is a scope bug.

### 3. Score

For each dimension, score on three axes: **coverage** (how many items are addressed), **quality** (how well they're handled), and **risk** (what's the blast radius if this dimension is wrong). Full rubric in SCORING-RUBRIC.md.

A dimension scoring below 3 on risk is a release blocker. Present blockers first.

### 4. Present findings

For each dimension, present:

- **Status** — blocking, risky, or clear
- **What's missing** — specific items from the checklist that aren't addressed
- **What's wrong** — items that exist but are incorrect or incomplete
- **What's solid** — items that are well-handled (acknowledge good work)
- **Remediation** — what to do, with standard fix patterns from REMEDIATION-PATTERNS.md

Ask the user which findings to address.

### 5. Remediate

For each selected finding, implement the fix pattern from REMEDIATION-PATTERNS.md. After implementing:

- Verify the fix doesn't change the intended scope of the release
- Confirm the changelog and release notes reflect the fix
- Re-check rollback viability — the fix itself must not introduce a rollback trap

## Rules

- Every finding must include a remediation. A validation report without fixes is a worry list, not a gate.
- Validate against the actual diff, not commit messages. Commit messages describe intent. The diff describes reality. When they disagree, trust the diff.
- Distinguish real problems from theoretical concerns. A missing changelog entry for a user-visible breaking change is real. A missing changelog entry for an internal refactor with no external effect is theoretical.
- Treat the changelog as part of the release. An undocumented breaking change is a release blocker even if the code is correct — the user who upgrades without knowing about it will break.
- Don't block on cosmetic issues. Wrong formatting in the changelog is a nit, not a blocker. A missing migration guide for a schema change is a blocker.
- Acknowledge what's already good. A validation that only lists problems misrepresents the state of the release.
- Check both directions: changes in the RC that shouldn't be there AND changes that should be there but aren't. Stowaway commits and missing cherry-picks are equally dangerous.
