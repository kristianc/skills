# SCORING-RUBRIC.md

How to score each readiness dimension from the audit. Each dimension gets rated 1-5 on three axes: **coverage**, **quality**, and **risk**. The scores drive what's blocking, what's risky, and what's acceptable for launch.

The point of scoring is decision-making, not precision. A pre-launch review needs to answer: "Can we ship this?" The scores tell you where the real gaps are.

## Axis 1: Coverage

How many items in the dimension are addressed?

**5 — Comprehensive.** Every item in the checklist for this dimension is addressed. The team has thought about each one and either implemented it or made a deliberate decision not to.

- Error handling: every code path has explicit error handling, user-facing errors are meaningful, retry logic exists, circuit breakers protect dependencies
- Observability: structured logging, tracing, RED metrics, dashboards all present
- Deployment: feature flags, canary rollout, tested rollback, zero-downtime verified, staging parity

**4 — Mostly covered.** Most items are addressed. The remaining gaps are known and have a rationale for deferral.

- Error handling: explicit error handling everywhere, meaningful user errors, retry logic — but no circuit breakers yet because there's only one external dependency
- Observability: structured logging and metrics — but tracing isn't set up because it's a single service

**3 — Partially covered.** About half the items are addressed. Some gaps are deliberate, others are oversights.

- Error handling: most paths have error handling, but some catch blocks are empty. Retry logic exists for the main API call but not others
- Observability: logging exists but isn't structured. Metrics exist but only at the global level, not per-endpoint

**2 — Minimal.** Only the most obvious items are addressed. Most of the dimension is unaddressed.

- Error handling: there's a global error handler that returns 500, but no per-path error handling. No retries, no circuit breakers
- Observability: console.log statements in some places. No metrics, no dashboards

**1 — Absent.** The dimension hasn't been addressed at all.

- Error handling: no explicit error handling. Exceptions propagate to the framework's default handler. Errors are swallowed silently in multiple places
- Observability: no logging beyond what the framework provides by default. No metrics, no tracing

---

## Axis 2: Quality

How well are the addressed items implemented?

**5 — Exemplary.** Implementations follow best practices, are well-tested, and demonstrate deep understanding of the problem.

- Error handling: typed error hierarchy, different handling for different error types, error middleware that's tested, retry logic with jitter and backoff
- Observability: structured logging with consistent fields, OpenTelemetry integration with custom spans, metrics with meaningful labels, dashboards that tell a story
- Database: expand-contract migrations, online DDL for large tables, backup restoration tested monthly

**4 — Solid.** Implementations are correct and maintainable. Minor improvements possible but nothing concerning.

- Error handling: consistent error handling pattern, meaningful messages, retry with backoff (but no jitter)
- Observability: structured logging with most important fields, basic tracing, standard RED metrics
- Database: reversible migrations, tested on production-sized data, automated backups with verified restoration

**3 — Functional.** Implementations work but have visible weaknesses — inconsistency, missing edge cases, or cargo-culted patterns.

- Error handling: error handling exists but the pattern varies between files. Some retries use fixed intervals instead of backoff. Some error messages are meaningful, others are generic
- Observability: logging is structured but fields are inconsistent across services. Metrics exist but the dashboard is hard to read
- Database: migrations have down methods but they haven't been tested. Backups exist but haven't been restored

**2 — Fragile.** Implementations technically exist but are unreliable, incomplete, or would fail under real conditions.

- Error handling: catch blocks exist but some swallow errors. Retry logic exists but has no maximum (can retry forever). Error messages include stack traces in production
- Observability: logging exists but is unstructured and noisy. A dashboard exists but shows the wrong metrics (averages instead of percentiles)
- Database: migrations exist but aren't reversible. Backups run but no one has verified they can be restored

**1 — Harmful.** Implementations exist but are worse than nothing — they give a false sense of security or actively cause problems.

- Error handling: catch blocks that swallow critical errors (data corruption goes undetected). Retry logic without backoff that hammers a struggling dependency. Error responses that leak internal details
- Observability: logging that records sensitive data (passwords, tokens). Metrics that are misleading (showing averages that mask p99 problems). Alerts that fire so often they're ignored
- Database: migrations that are irreversible and untested. "Backups" that are actually writing to the same disk as the database

---

## Axis 3: Risk

What is the blast radius if this dimension fails in production?

**5 — Contained.** If this dimension fails, the impact is minimal — a degraded experience for a subset of users, temporary inconvenience, no data loss.

- Error handling failure for a non-critical feature: users see a generic error on one screen, but the rest of the product works fine
- Missing monitoring on a background job: the job fails silently, but no user-facing impact — it can be rerun manually
- No rate limiting on an internal tool: the tool could be slow under abuse, but it's behind auth and only used by the team

**4 — Limited.** If this dimension fails, some users are affected noticeably, but the core product continues to work. Recovery is straightforward.

- Error handling failure causes a secondary feature to be unavailable: notifications don't send, but the main workflow works
- Observability gap means a slow-to-diagnose incident: the system recovers on its own but the team takes 30 minutes to understand why
- Missing timeout on a non-critical dependency: occasional slow requests, but the main flow isn't blocked

**3 — Significant.** If this dimension fails, many users are affected or the team's ability to operate is seriously degraded.

- Error handling failure causes the main user flow to return 500 errors under certain conditions
- Observability gap means the team can't diagnose a production issue and relies on users to report symptoms
- Database migration locks a table for 10 minutes during peak hours, causing timeouts for all queries on that table

**2 — Severe.** If this dimension fails, most or all users are affected. Recovery requires significant effort.

- Error handling failure causes cascading crashes across services
- Missing rollback capability means a bad deploy requires a manual fix (writing and deploying new code) rather than a revert
- Database corruption from an irreversible migration requires restoring from backup, causing data loss

**1 — Catastrophic.** If this dimension fails, the entire system is down with no clear recovery path, or data loss is permanent.

- No backups: data corruption or accidental deletion is unrecoverable
- Missing health checks and no monitoring: the system is down and no one knows until a user complains
- No timeout on database calls: a hung database connection exhausts the pool, bringing down every service that depends on it, with no automatic recovery

---

## Using the scores

### Launch decision matrix

The combination of scores determines the launch decision for each dimension:

| Decision | Condition |
|----------|-----------|
| **Blocking** | Risk score of 1 or 2 on any dimension. The system is too fragile to ship — a failure in this dimension would be catastrophic or severe and recovery is unclear. |
| **Risky** | Risk score of 3, OR coverage below 3, OR quality below 3 on a dimension with risk 4+. The system can ship, but the team is accepting known risk. Document the risk and set a deadline for remediation. |
| **Acceptable** | Risk score of 4-5 AND coverage of 3+ AND quality of 3+. The dimension is ready for production. There may be improvements to make, but nothing that should delay launch. |
| **Solid** | All three scores at 4+. This dimension is well-handled. Acknowledge it. |

### Distinguishing "not ready" from "ready with known risks" from "ready"

The most important distinction the scoring produces is between these three states:

**Not ready (blocking):** There are gaps that would cause serious harm if they fail. These must be fixed before shipping. Examples: no backups for a system with user data, no health checks for a load-balanced service, no timeout on external calls for a high-traffic service.

**Ready with known risks:** The system works and has reasonable safeguards, but there are gaps the team is aware of and accepting. The risk is documented, and there's a plan to address it. Examples: no canary deploy (deploying to 100% is risky but the team monitors actively), no circuit breakers (only one external dependency and it's highly reliable), observability covers the main flow but not edge cases.

**Ready:** Every dimension has adequate coverage and quality, and the risk is contained. The system can be operated by someone who didn't build it. There's always room for improvement, but nothing that should delay shipping.

### Presenting the assessment

Present the assessment as a summary table, then detail the blockers and risks:

```
| Dimension      | Coverage | Quality | Risk | Status     |
|----------------|----------|---------|------|------------|
| Error handling |    4     |    4    |   4  | Acceptable |
| Observability  |    3     |    3    |   3  | Risky      |
| Alerting       |    2     |    2    |   2  | Blocking   |
| Database       |    4     |    4    |   5  | Solid      |
| Performance    |    3     |    3    |   4  | Acceptable |
| Deployment     |    4     |    3    |   3  | Risky      |
| Resilience     |    3     |    3    |   3  | Risky      |
| Operational    |    2     |    2    |   2  | Blocking   |
```

Then for each blocking and risky dimension, detail the specific findings and remediations. For acceptable and solid dimensions, briefly acknowledge what's working.

The goal is a conversation with the team about what to fix before launch, what to accept as known risk, and what to prioritize after launch — not a pass/fail report card.
