---
name: production-ready
description: Audit whether a feature or product is ready to survive real users at scale — error handling, observability, deployment safety, resilience, and operational readiness. Use before shipping a feature, before launch, after a major refactor, during periodic production readiness reviews, or when moving from beta to GA.
---

# Production Ready

Systematically audit a feature or product for production readiness — not "does it work" but "will it survive contact with real users at scale." The goal is to find the gaps between "it runs on my machine" and "it runs in production at 3 AM when I'm asleep."

This skill covers the operational dimensions that separate working code from production-grade code: error handling, observability, alerting, database safety, performance, deployment, resilience, and operational preparedness.

## Glossary

Use these terms precisely. Full definitions in GLOSSARY.md.

- **Production readiness** — the state where a system can be operated by someone who didn't write it, at scale, without the original author on call.
- **Observability** — the ability to understand what the system is doing from the outside, using logs, metrics, and traces.
- **Blast radius** — the scope of damage when something fails.
- **Graceful degradation** — the ability to continue providing partial service when a dependency fails.
- **Rollback** — reverting a deployment to the previous known-good state.

## Process

### 1. Scope

Establish what is being reviewed: a new feature, a new service, a database migration, or a full product launch. Ask the user if not obvious.

Define the blast radius: how many users does this affect? What downstream systems depend on it? What is the cost of an outage — inconvenience, data loss, revenue loss, safety risk?

### 2. Audit

Walk each readiness dimension against the checklist in READINESS-CHECKLIST.md. For each dimension — error handling, observability, alerting, database, performance, deployment, resilience, operational — check every applicable item against the codebase.

Use the Agent tool with `subagent_type=Explore` to walk the codebase. Search for error handling patterns, logging calls, health check endpoints, migration files, feature flag usage, timeout configuration, and retry logic.

Don't stop at the first finding per dimension. A single service can be missing structured logging AND have swallowed exceptions AND lack a health check endpoint.

### 3. Score

For each dimension, score on three axes: **coverage** (how many items are addressed), **quality** (how well they're implemented), and **risk** (what's the blast radius if this dimension fails). Full rubric in SCORING-RUBRIC.md.

A dimension scoring below 3 on risk is a launch blocker. Present blockers first.

### 4. Present findings

For each dimension, present:

- **Status** — blocking, risky, or acceptable
- **What's missing** — specific items from the checklist that aren't addressed
- **What's present but weak** — items that exist but don't meet the quality bar
- **What's solid** — items that are well-implemented (acknowledge good work)
- **Remediation** — what to do, with standard fix patterns from REMEDIATION-PATTERNS.md

Ask the user which findings to address.

### 5. Remediate

For each selected finding, implement the fix pattern from REMEDIATION-PATTERNS.md. After implementing:

- Verify the fix addresses the root cause, not just the symptom
- Check that the fix doesn't introduce new failure modes
- Confirm the fix is observable — if it fails, will someone know?

## Rules

- Every finding must include a remediation. A readiness report without fixes is a worry list, not an audit.
- Distinguish real gaps from theoretical concerns. Missing rate limiting on a public API is real. Missing rate limiting on an internal admin endpoint behind a VPN is theoretical.
- Don't gold-plate. The goal is production-ready, not production-perfect. A working health check endpoint is better than no health check endpoint, even if it doesn't check every dependency.
- Error messages surfaced to users should be reviewed with the error-copy-review skill. This skill focuses on the operational side — are errors handled, logged, and recoverable — not on whether the user sees a helpful message.
- Acknowledge what's already good. A readiness review that only lists problems demoralizes the team and misrepresents the state of the system.
