# READINESS-CHECKLIST.md

The deep reference for the audit step (Step 2). Each dimension lists the items to check, what each means concretely, how to verify it in code, what "good enough" looks like vs. gold standard, and the severity if missing. Walk every applicable dimension for the feature or service being reviewed.

---

## 1. Error handling

Every code path has an explicit plan for failure. No exception is swallowed, no error is hidden, no failure mode is left to chance.

### Every code path has explicit error handling

**What it means:** Every `try/catch`, every `.catch()`, every error callback does something intentional — logs the error, returns a meaningful response, retries, or propagates. No empty catch blocks. No `catch (e) {}`.

**How to check:** Search for empty catch blocks: `catch (e) {}`, `catch (e) { }`, `except: pass`, `rescue => nil`. Search for catch blocks that only log but don't handle: the error is logged but the function continues as if nothing happened, returning stale or default data.

**Good enough:** Every catch block either re-throws, returns an error response, or logs and takes a recovery action. No silent swallowing.

**Gold standard:** Typed error handling — different error types get different treatment. Network errors retry. Validation errors return 400. System errors return 500 with a request ID for debugging. Error types are defined, not ad-hoc.

**If missing:** Risky. Swallowed errors are invisible failures — the system looks healthy while data is silently wrong.

### User-facing errors are meaningful

**What it means:** When an error reaches the user, it explains what happened and what to do next. No "Something went wrong." No raw error codes. No stack traces.

**How to check:** Search for generic error messages: "Something went wrong", "An error occurred", "Please try again later", "Error", "Unknown error". Search for error boundaries or global error handlers — what do they show the user? Search for places where `error.message` is displayed directly to the user (may leak internal details).

**Good enough:** Every user-facing error names the problem and offers a next step. Generic fallbacks exist but are only reached for truly unknown errors, and even then they include a request ID or support contact.

**Gold standard:** Error copy reviewed with the error-copy-review skill. Each error is classified by type and has tailored copy with recovery actions.

**If missing:** Risky for user trust. Users who see "Something went wrong" repeatedly lose confidence in the product. See the error-copy-review skill for the full discipline.

### Retry logic with backoff for transient failures

**What it means:** When a network call or external dependency fails with a transient error (timeout, 503, connection reset), the system retries with exponential backoff and jitter rather than immediately failing or retrying in a tight loop.

**How to check:** Search for HTTP client calls and check if they have retry configuration. Search for retry libraries: `axios-retry`, `retry`, `tenacity`, `backoff`, `polly`. Check that retries use exponential backoff (not fixed intervals) and have a maximum retry count.

**Good enough:** External calls retry 2-3 times with exponential backoff. Maximum retry count prevents infinite loops. Transient errors (timeouts, 503s) are retried; permanent errors (400s, 404s) are not.

**Gold standard:** Retry policies are centralized (not configured per-call), use jitter to prevent thundering herds, and are observable — retries are logged and counted as metrics so you can see when a dependency is flaky.

**If missing:** Risky. Without retries, every network blip becomes a user-visible error. Without backoff, retries can overwhelm a struggling dependency.

### Circuit breakers for external dependencies

**What it means:** When an external dependency fails repeatedly, the system stops calling it and returns a fallback response instead of piling on requests to a service that's already down.

**How to check:** Search for circuit breaker libraries: `opossum`, `cockatiel`, `resilience4j`, `polly`, `pybreaker`. For each external dependency, check if there's a failure threshold, a fallback, and a recovery probe.

**Good enough:** Critical dependencies have circuit breakers. When the breaker trips, a reasonable fallback is served (cached data, default values, or an honest "this feature is temporarily unavailable" message).

**Gold standard:** Every external call goes through a circuit breaker. Breaker state is exposed as a metric. Fallbacks are tested — the team has verified that the product still works when each dependency is down.

**If missing:** Risky for high-traffic services. A dependency outage cascades: every request waits for a timeout, threads/connections are exhausted, and the entire service goes down because one dependency is slow.

---

## 2. Observability

The team can understand what the system is doing without reading the code. Logs, metrics, and traces provide the visibility needed to debug problems and answer "is it working?"

### Structured logging on every significant operation

**What it means:** Every significant operation — request received, request completed, external call made, business logic executed, error encountered — produces a structured log entry with consistent fields: timestamp, request ID, user ID, operation name, duration, result.

**How to check:** Search for logging calls: `logger.info`, `console.log`, `logging.info`. Are they structured (JSON with named fields) or unstructured (free-form strings)? Are request IDs propagated across log entries so a single request's journey can be traced? Are log levels used correctly — info for normal operations, warn for recoverable problems, error for failures?

**Good enough:** Structured JSON logging with request ID, operation name, and duration on key operations. Log levels used consistently. No sensitive data in logs (passwords, tokens, PII).

**Gold standard:** Every log entry includes a correlation ID that spans the full request lifecycle. Log fields are standardized across services. Sensitive data is automatically redacted. Log volume is managed — verbose debug logging can be enabled dynamically for specific requests or users without redeployment.

**If missing:** Blocking for new services. Without structured logging, debugging production issues means grepping through free-text strings — slow, error-prone, and impossible to automate. For existing services, this is risky — you can operate but you can't diagnose efficiently.

### Request tracing across services

**What it means:** A single user action that touches multiple services can be tracked end-to-end. The trace shows which services were called, in what order, how long each took, and where failures occurred.

**How to check:** Search for tracing libraries: `opentelemetry`, `@opentelemetry/sdk-trace-node`, `dd-trace`, `jaeger-client`, `zipkin`. Check if trace context is propagated in HTTP headers (`traceparent`, `x-request-id`, `x-trace-id`). For single-service applications, check if request IDs are generated and included in all log entries.

**Good enough:** Request IDs are generated at the edge and propagated to all downstream calls and log entries. For multi-service architectures, a tracing library is integrated and traces are viewable in a tracing UI.

**Gold standard:** OpenTelemetry fully integrated. Traces include custom spans for business-critical operations (not just HTTP calls). Trace sampling is configured to balance cost and visibility. Traces link to related logs and metrics.

**If missing:** Risky for multi-service architectures. Without tracing, debugging a slow request across services means correlating timestamps across multiple log streams — which is impractical for anything but the most obvious problems. For single-service applications, this is nice-to-have if request IDs are present in logs.

### Metrics on latency, throughput, and error rates

**What it means:** The system emits metrics that answer three questions at a glance: How fast is it? (latency) How much work is it doing? (throughput) How often is it failing? (error rate). These are the RED metrics — Rate, Errors, Duration.

**How to check:** Search for metrics libraries: `prom-client`, `statsd`, `datadog-metrics`, `micrometer`, `prometheus_client`. Check if the application emits request duration histograms, request counters (by status code), and error counters. Check if these are exposed via a `/metrics` endpoint or sent to a metrics backend.

**Good enough:** Request latency (p50, p95, p99), request rate, and error rate are collected and visible in a dashboard. Key business operations (checkout, signup, search) have their own metrics beyond the global HTTP metrics.

**Gold standard:** RED metrics for every endpoint. USE metrics (Utilization, Saturation, Errors) for infrastructure resources (CPU, memory, connections, queue depth). Custom business metrics (signups per hour, payments processed, items searched). All metrics have labels/tags for slicing by endpoint, user segment, or region.

**If missing:** Blocking. Without metrics, the team cannot answer "is it working?" without looking at individual requests. Metrics are the first thing an on-call engineer checks when an alert fires.

### Dashboards that answer "is it working?"

**What it means:** There is a dashboard — not just raw metrics — that an on-call engineer can open and immediately understand the health of the system. It shows the key signals, highlights anomalies, and requires no specialized knowledge to interpret.

**How to check:** Does a dashboard exist? (Check Grafana, Datadog, CloudWatch, or equivalent.) Does it show latency, error rate, and throughput? Does it have a time range selector? Can someone unfamiliar with the system look at it and determine if the system is healthy?

**Good enough:** A single dashboard showing latency percentiles, error rate, and throughput over time. Obvious spikes are visible. The dashboard URL is documented in the runbook or README.

**Gold standard:** Multiple dashboards at different levels: a system-level overview, per-service dashboards, and per-endpoint dashboards. Dashboards include annotations for deployments (so you can correlate a deploy with a metrics change). Business metrics are alongside technical metrics.

**If missing:** Risky. Metrics without dashboards are data without interpretation. The on-call engineer has to build ad-hoc queries to answer basic questions, which is slow and error-prone during an incident.

---

## 3. Alerting

The team is notified of problems before users report them. Alerts are actionable, not noisy.

### Alerts on SLO violations, not just errors

**What it means:** Alerts fire when the service is failing to meet its service level objective — not when any single error occurs. A single 500 error is noise. A 2% error rate sustained for 5 minutes when the SLO target is 0.1% is a real problem.

**How to check:** Review alert definitions in the monitoring platform. Are alerts based on thresholds over time windows (error rate > 1% for 5 minutes) or on individual events (any 500 triggers an alert)? Are SLOs defined and tracked?

**Good enough:** Alerts on error rate and latency percentiles over time windows. No alerts on individual errors. At least one SLO defined for the most critical user-facing operation.

**Gold standard:** SLOs defined for every critical operation. Error budget tracking drives development priorities. Alerts fire at different severity levels as the error budget burns — a slow burn is a warning, a fast burn is a page. Multi-window burn rate alerting.

**If missing:** Risky. Without SLO-based alerting, the team either gets paged for every error (alert fatigue) or only learns about outages from users (too late). SLO-based alerting balances sensitivity and specificity.

### Alert fatigue prevention

**What it means:** Alerts are tuned so that every alert that fires is worth waking someone up for (if it's a page) or worth investigating soon (if it's a warning). No flapping alerts. No alerts that fire and self-resolve before anyone can investigate. No duplicate alerts for the same underlying problem.

**How to check:** Review alert history. How many alerts fired in the last 30 days? How many required action? How many were acknowledged and immediately closed as noise? Is there a "mute" or "snooze" culture where alerts are routinely silenced?

**Good enough:** Every alert has fired at least once in a drill or real incident. No alerts fire more than once a week without requiring action. Alerts that are routinely ignored are either fixed or removed.

**Gold standard:** Alert quality is reviewed quarterly. New alerts require a runbook before they're activated. Alert response rate is tracked — if less than 80% of pages result in action, the alert set needs pruning.

**If missing:** Risky. Alert fatigue means the real alerts get ignored along with the noise. The team that gets paged 10 times a week starts treating pages like email.

### Escalation paths

**What it means:** When an on-call engineer can't resolve an alert within a defined timeframe, it escalates to the next person. Escalation paths are defined, documented, and configured in the on-call tool.

**How to check:** Is there an on-call rotation in PagerDuty, OpsGenie, or equivalent? Are escalation policies configured? Does each alert have a defined escalation path, or is there a single person who gets all pages?

**Good enough:** An on-call rotation exists. Escalation to a secondary is configured with a timeout (e.g., 15 minutes). The on-call engineer knows who to escalate to for issues outside their area.

**Gold standard:** Escalation paths are per-service, with the team that built the service as the escalation target. Escalation includes automatic notification to management for incidents above a severity threshold. Post-escalation follow-up ensures the primary learns from the escalation.

**If missing:** Risky. Without escalation, a missed page means no one responds. If the on-call engineer is asleep, sick, or overwhelmed, the problem festers until a user reports it.

---

## 4. Database

Schema changes don't cause downtime. Data is backed up and recoverable. Queries perform at production scale.

### Migrations are reversible

**What it means:** Every database migration has a corresponding rollback migration. If the deploy fails, the migration can be reversed without manual intervention or data loss.

**How to check:** For each migration file, check if there's a `down` or `rollback` method. Check if destructive operations (dropping columns, dropping tables, changing column types) have a rollback strategy. Check if data-destructive migrations (deleting rows, truncating tables) have a backup step.

**Good enough:** Every migration has a `down` method. Destructive migrations are flagged for manual review. The team has tested rolling back the most recent migration.

**Gold standard:** Migrations follow the expand-contract pattern: new columns/tables are added (expand), code is updated to use the new schema, old columns/tables are removed in a later migration (contract). This means the migration and the code deployment are decoupled — either can be rolled back independently.

**If missing:** Blocking for migrations that alter existing columns or tables. Adding a new table or column is low-risk even without a rollback — you can drop it later. But altering or removing existing schema without a rollback plan risks an outage if the deploy fails.

### No locking migrations on large tables

**What it means:** Schema changes on tables with millions of rows don't hold locks that block reads or writes for extended periods. On MySQL, many `ALTER TABLE` operations lock the table. On PostgreSQL, adding a column with a default value locks the table in versions before 11.

**How to check:** Identify the largest tables (by row count). Check if any migration runs `ALTER TABLE` on those tables. Check if the migration tool supports online schema changes (e.g., `gh-ost` for MySQL, `pg_repack` for PostgreSQL). For PostgreSQL 11+, adding a column with a default is safe — but adding an index without `CONCURRENTLY` is not.

**Good enough:** The team knows which tables are large and has tested the migration against a production-sized dataset. Locking migrations are identified and scheduled for a maintenance window if unavoidable.

**Gold standard:** All schema changes on large tables use online DDL tools. Index creation uses `CONCURRENTLY` (PostgreSQL) or equivalent. The team has measured the expected lock time for every migration.

**If missing:** Blocking for large tables. A locking migration on a table with 50 million rows can take minutes to hours, during which every query on that table blocks. This is the most common cause of deployment-related outages.

### Data backups verified

**What it means:** Backups exist, are automated, are stored off-site, and — critically — have been tested by restoring them. An untested backup is not a backup; it's a hope.

**How to check:** Is automated backup configured (RDS snapshots, `pg_dump` cron, mongodump)? How frequently? How long are backups retained? When was the last time a backup was restored to verify it works? Is point-in-time recovery (PITR) configured?

**Good enough:** Automated daily backups with 30-day retention. The team has restored a backup at least once (even to a test environment). Backup completion is monitored — a failed backup triggers an alert.

**Gold standard:** Automated backups with PITR. Backup restoration is tested monthly as part of a disaster recovery drill. Backup and restore time are documented (RTO — Recovery Time Objective). The team knows how much data they'd lose in a worst-case scenario (RPO — Recovery Point Objective).

**If missing:** Blocking. Without verified backups, any data corruption or accidental deletion is permanent. This is the gap that turns a bad deploy into a catastrophe.

### Connection pooling configured

**What it means:** The application uses a connection pool to manage database connections rather than opening a new connection per request. Connection pools limit the number of simultaneous connections to the database and reuse connections across requests.

**How to check:** Search for connection pool configuration: `pool`, `connectionLimit`, `max_connections`, `pool_size`. Check the database connection setup — is it using a pool or a direct connection? For serverless environments (Lambda, Cloud Functions), check if an external pool (PgBouncer, RDS Proxy) is configured, since each invocation may create a new connection.

**Good enough:** A connection pool is configured with reasonable limits (not higher than the database's `max_connections` divided by the number of application instances). Pool exhaustion is handled gracefully (queued or rejected with a clear error, not hanging indefinitely).

**Gold standard:** Pool size is tuned based on load testing. Pool metrics are exposed (active connections, idle connections, wait time). Pool exhaustion triggers an alert. For serverless, an external pool (PgBouncer, RDS Proxy) is in place.

**If missing:** Risky. Without pooling, the application can exhaust the database's connection limit under load, causing new requests to fail. This is one of the most common causes of production database issues.

### Query performance tested at production scale

**What it means:** The queries the application runs have been tested against a dataset that matches production in size and shape. A query that runs in 5ms against 1,000 rows may take 30 seconds against 10 million rows.

**How to check:** Are there any slow query logs or query performance monitoring configured? Has the team run `EXPLAIN ANALYZE` (or equivalent) on the most critical queries against production-sized data? Are indexes in place for the query patterns the application actually uses?

**Good enough:** The team has identified the 5-10 most critical queries (login, search, listing, detail pages) and verified they perform acceptably against production-sized data. Missing indexes are identified and added.

**Gold standard:** All queries have been profiled. N+1 queries are identified and eliminated. Query performance is monitored in production — slow queries trigger alerts. The team has a query performance budget (e.g., no query should take more than 100ms at p99).

**If missing:** Risky. Poor query performance is the most common cause of latency issues that only appear at scale. A query that works fine in development can bring down a production database.

---

## 5. Performance

The system performs acceptably under expected load and has headroom for growth.

### Load tested at 2x expected peak

**What it means:** The system has been tested under load that exceeds the expected peak by at least 2x. This provides headroom for traffic spikes, growth, and unexpected events (viral moments, marketing campaigns, seasonal peaks).

**How to check:** Has a load test been run? What tool was used (k6, Locust, Artillery, JMeter)? What was the target load, and what was the actual peak? What did latency and error rate look like at peak? Were there any resource bottlenecks (CPU, memory, connections, disk I/O)?

**Good enough:** A load test has been run at 2x expected peak. The system remained stable (no crashes, no errors, latency within acceptable bounds). Bottlenecks are identified and documented, even if not yet fixed.

**Gold standard:** Load tests run in CI on every significant change. Load test scenarios cover not just throughput but also spike patterns (sudden 10x traffic), soak patterns (sustained load for hours to check for memory leaks), and stress patterns (load until failure to know the breaking point). Load test results are tracked over time to catch performance regressions.

**If missing:** Risky. Without load testing, the team is guessing about capacity. The first real load test is production — and if the system can't handle it, the first users to notice are paying customers.

### P99 latency acceptable

**What it means:** The slowest 1% of requests (p99 latency) complete within a time that's acceptable for the use case. P99, not average — the average hides the misery of the long tail.

**How to check:** Are latency percentiles being measured (p50, p95, p99)? What is the p99 for the most critical endpoints? Is it within the SLO? Is there a large gap between p50 and p99 (indicating high variance)?

**Good enough:** P99 latency is known and acceptable for user-facing endpoints. Endpoints with poor p99 are identified and have a plan for improvement.

**Gold standard:** P99 latency is within the SLO for every endpoint. Latency is tracked per-endpoint, and regressions trigger alerts. The team understands what drives the long tail (database contention, external call latency, garbage collection) and has strategies to reduce it.

**If missing:** Risky. If p99 latency is not measured, the team doesn't know how bad the worst-case experience is. 1% of requests sounds small, but at 1 million requests per day, that's 10,000 users having a bad time.

### No N+1 queries

**What it means:** The application doesn't execute one query to fetch a list of records and then one additional query per record to fetch related data. N+1 queries are the most common performance bug in applications using ORMs.

**How to check:** Enable query logging and walk the main flows (listing pages, detail pages, dashboard). Count the number of queries executed per page load. If a listing page with 50 items executes 51 queries (1 for the list + 50 for related data), that's N+1. Search for ORM usage without eager loading: `.find()`, `.all()` followed by property access that triggers lazy-loaded relations.

**Good enough:** The main listing and detail pages have been checked for N+1 queries. Any found have been fixed with eager loading (`include`, `join`, `prefetch_related`).

**Gold standard:** A query count assertion in tests for critical pages. Query logging in development that warns when more than N queries are executed per request. APM tooling that identifies N+1 patterns in production.

**If missing:** Risky at scale. N+1 queries scale linearly with data — a page with 10 items makes 11 queries, but with 100 items it makes 101. The database connection pool fills up, latency spikes, and the page becomes unusable.

### CDN and caching configured

**What it means:** Static assets (JS, CSS, images) are served from a CDN. Dynamic content that doesn't change per-user is cached at the appropriate level (CDN edge, application cache, database query cache).

**How to check:** Are static assets served from a CDN (CloudFront, Fastly, Cloudflare)? Do responses include appropriate `Cache-Control` headers? Is there application-level caching (Redis, Memcached) for expensive operations? Are cache invalidation strategies defined?

**Good enough:** Static assets are on a CDN with long cache lifetimes and cache-busting filenames (content hashes). The most expensive API responses are cached for a reasonable TTL.

**Gold standard:** Full caching strategy documented: what's cached where, what TTL, what the invalidation strategy is. Cache hit rates are monitored. Stale-while-revalidate is used where appropriate. No cache thundering herds — a mutex or leader election prevents all instances from regenerating an expired cache simultaneously.

**If missing:** Risky for user-facing performance. Without CDN, every user request for static assets hits the origin server. Without application caching, every request recalculates data that changes infrequently.

### Cold start time acceptable

**What it means:** The time from "new instance starts" to "ready to serve requests" is fast enough that scaling events and deployments don't cause user-visible latency spikes. Particularly important for serverless (Lambda cold starts) and containerized deployments.

**How to check:** Measure the time from container start to the first successful health check response. For serverless, measure cold start latency vs. warm request latency. Check for heavy initialization: large config files, eager database queries, synchronous API calls during startup.

**Good enough:** Cold start time is measured and acceptable for the deployment model. For containers behind a load balancer, health checks prevent traffic before the instance is ready. For serverless, provisioned concurrency is configured for latency-critical functions.

**Gold standard:** Startup is optimized — lazy initialization for non-critical dependencies, connection pooling that warms up in the background, minimal synchronous work before the health check returns. Cold start time is tracked as a metric and regressions are caught.

**If missing:** Nice-to-have for long-running services with rare restarts. Risky for serverless or auto-scaling services where new instances are created frequently.

---

## 6. Deployment

Shipping code to production is safe, reversible, and observable.

### Feature flags for new functionality

**What it means:** New features are deployed behind feature flags, allowing the team to enable them for a subset of users (or disable them entirely) without a new deployment. This decouples deployment (code in production) from release (feature available to users).

**How to check:** Search for feature flag libraries: `launchdarkly`, `unleash`, `flagsmith`, `flipper`, `split`. For new features, check if they're gated behind a flag. Check if there's a process for cleaning up old flags.

**Good enough:** New user-facing features are behind flags. Flags can be toggled without a deployment. The team has used flags to roll back a feature at least once.

**Gold standard:** Feature flags are used for all significant changes, including backend changes. Flags support percentage rollouts (1%, 10%, 50%, 100%). Flag states are logged so you can correlate behavior with flag configuration. Flag cleanup is part of the definition of done — a flag is removed within 2 weeks of full rollout.

**If missing:** Risky. Without feature flags, the only way to disable a broken feature is to deploy a revert. If the deploy pipeline takes 30 minutes, that's 30 minutes of broken behavior.

### Canary or staged rollout plan

**What it means:** New deployments go to a small percentage of traffic first, with monitoring, before rolling out to everyone. If the canary shows problems, the rollout stops and the canary is rolled back.

**How to check:** Does the deployment pipeline support canary or staged rollouts? Is there a defined rollout plan (e.g., 5% for 15 minutes, then 25%, then 100%)? Are there automated success criteria (error rate, latency) that gate each stage?

**Good enough:** The team has a documented rollout procedure. Critical services use a staged rollout (even if manual). Monitoring is checked between stages.

**Gold standard:** Automated canary analysis — the deployment pipeline automatically compares canary metrics to baseline and promotes or rolls back without human intervention. Rollout stages are defined per-service based on blast radius.

**If missing:** Risky. Deploying to 100% of traffic simultaneously means any bug affects every user immediately. For high-blast-radius services, this should be blocking.

### Rollback procedure tested

**What it means:** The team has actually performed a rollback — not just documented one. The procedure works, the time to rollback is known, and the system returns to a healthy state after rollback.

**How to check:** When was the last time the team rolled back a deployment? How long did it take? Did the system return to a healthy state? If there were database migrations, was the rollback of the migration also tested?

**Good enough:** The team can articulate the rollback procedure and has performed it at least once (in staging if not production). The expected rollback time is documented.

**Gold standard:** Rollback is one command or one button. Rollback time is under 5 minutes. Database migration rollbacks are tested alongside code rollbacks. Rollback triggers automatic verification (health checks pass, error rate returns to normal).

**If missing:** Blocking if the deployment includes database migrations or other stateful changes. If the deploy is purely stateless code, the container orchestrator's rollback is usually sufficient — but the team should still verify they know how to invoke it.

### Zero-downtime deploy verified

**What it means:** Deploying new code doesn't cause dropped requests, connection resets, or error spikes. Old instances drain their connections before shutting down. New instances are healthy before receiving traffic.

**How to check:** Does the deployment strategy support zero-downtime (rolling update, blue-green, canary)? Is graceful shutdown configured — does the application handle `SIGTERM` by stopping new request acceptance and draining in-flight requests? Are health checks configured so the load balancer only routes to healthy instances?

**Good enough:** Rolling deploys with health checks. Graceful shutdown with a drain period. No error spikes visible in metrics during deploys.

**Gold standard:** Blue-green or canary deployment with instant rollback. Deployment events are annotated in dashboards so the team can correlate deployments with metric changes. Long-running requests (WebSockets, file uploads) are handled during deploy transitions.

**If missing:** Risky. If deploys cause error spikes, the team will be reluctant to deploy frequently, leading to larger, riskier deployments. Zero-downtime deploys are a prerequisite for continuous delivery.

### Environment parity

**What it means:** Staging matches production closely enough that a successful staging deploy predicts a successful production deploy. Same OS, same database version, same dependency versions, same configuration shape (different values for secrets, but the same structure).

**How to check:** Compare staging and production: same database engine and version? Same runtime version? Same infrastructure topology (single instance vs. clustered)? Are there environment-specific code paths that only run in production?

**Good enough:** Same database engine and version. Same runtime version. Same deployment mechanism. Configuration differences are limited to expected things (hostnames, credentials, feature flags).

**Gold standard:** Staging is a scaled-down replica of production — same topology, same CDN configuration, same monitoring stack. Infrastructure is defined as code (Terraform, Pulumi, CloudFormation) so parity is enforced, not hoped for.

**If missing:** Risky. Every difference between staging and production is a potential source of "worked in staging, broke in production." The most common parity gaps: different database versions, different OS versions, single instance vs. clustered.

---

## 7. Resilience

The system continues to work (possibly in a degraded mode) when things go wrong. Dependencies fail, traffic spikes, networks drop — the system handles it.

### Graceful degradation when dependencies fail

**What it means:** When an external dependency (API, database, cache, queue) is unavailable, the system provides a degraded but usable experience rather than a full failure.

**How to check:** For each external dependency, ask: "What happens if this is down?" Trace the code path from the dependency call through error handling. Does the user see a 500, or does the system provide a fallback? Is the degradation intentional (designed) or accidental (the error happens to not crash the page)?

**Good enough:** The team has identified the critical dependencies and knows what happens when each one fails. For the most important ones, a deliberate fallback exists (cached data, default values, "temporarily unavailable" for the specific feature).

**Gold standard:** Every external dependency has a tested degradation path. The team regularly runs "failure injection" exercises — deliberately disabling a dependency in staging and verifying the degraded experience. Degradation modes are documented in the runbook.

**If missing:** Risky. Without graceful degradation, the system's availability is the product of all dependency availabilities. If you depend on five services each at 99.9%, your availability is at best 99.5% — unless you can tolerate individual failures.

### Rate limiting on public endpoints

**What it means:** Public-facing endpoints enforce limits on how many requests a client can make in a given time window. This protects against abuse (scraping, brute force) and accidental overload (runaway scripts, misconfigured clients).

**How to check:** Search for rate limiting middleware: `express-rate-limit`, `ratelimit`, `django-ratelimit`, `rack-throttle`. Check if rate limits are configured on authentication endpoints (login, registration, password reset), API endpoints, and any endpoint that does expensive work (search, report generation).

**Good enough:** Login and registration endpoints are rate-limited. Public API endpoints have a per-client rate limit. Rate limit responses include `Retry-After` headers so clients know when to try again.

**Gold standard:** Rate limiting at multiple levels: per-IP at the edge (CDN/WAF), per-user in the application, per-endpoint for expensive operations. Rate limits are tuned based on actual usage patterns. Rate limit hits are logged and trigger alerts if sustained (indicating an attack or a misconfigured client).

**If missing:** Risky for public-facing services. Without rate limiting, a single client can monopolize server resources, and brute-force attacks on login endpoints have no mitigation.

### Timeout on every external call

**What it means:** Every HTTP request, database query, and external service call has an explicit timeout. Without timeouts, a slow or hung dependency can consume all available threads/connections, causing the entire service to become unresponsive.

**How to check:** Search for HTTP client configuration: `timeout`, `connectTimeout`, `socketTimeout`. Check that default timeout values are set at the client level (not relying on the OS default, which can be minutes). Search for database query timeouts: `statement_timeout` (PostgreSQL), `net_read_timeout` (MySQL).

**Good enough:** HTTP client has a default timeout configured (e.g., 10 seconds). Database has a statement timeout. No calls use "infinite" or unset timeouts.

**Gold standard:** Timeouts are set per-dependency based on expected response time. A fast dependency (cache lookup) has a tight timeout (500ms). A slow dependency (report generation) has a longer timeout (30s). Timeout values are documented and tuned based on observed p99 latency.

**If missing:** Blocking. A missing timeout is a guaranteed outage when a dependency hangs. One hung connection leads to thread exhaustion, which leads to all requests queuing, which leads to the entire service being down. This is the single most common cause of cascading failures.

### Idempotent operations where needed

**What it means:** Operations that can be retried (payment processing, order creation, message sending) produce the same result whether they're executed once or multiple times. This prevents duplicate charges, duplicate orders, or duplicate notifications when retries happen.

**How to check:** Identify the operations that involve money, data creation, or notifications. Check if they use idempotency keys. Check if the retry logic (from the retry-with-backoff item above) can trigger duplicate side effects.

**Good enough:** Payment and order operations use idempotency keys. The most critical mutation endpoints are safe to retry.

**Gold standard:** All non-idempotent mutations accept an idempotency key from the client. The server stores the result of the first execution and returns it for subsequent calls with the same key. Idempotency keys have a TTL and are cleaned up.

**If missing:** Risky for operations involving money or data creation. A retry that creates a duplicate payment is a support ticket at best and a regulatory issue at worst. For operations with no real side effects (updating a name, toggling a setting), this is nice-to-have.

### Queue and retry for async work

**What it means:** Work that doesn't need to complete during the user's request (sending emails, generating reports, processing uploads) is enqueued for asynchronous processing. Failed jobs are retried with backoff. Jobs that permanently fail go to a dead letter queue for investigation.

**How to check:** Search for queue libraries: `bull`, `bullmq`, `celery`, `sidekiq`, `resque`, `SQS`, `RabbitMQ`. Check if async work has retry configuration. Check if a dead letter queue or failed job storage exists. Check if job processing is monitored — queue depth, processing rate, failure rate.

**Good enough:** Heavy or non-urgent work is enqueued. Failed jobs retry with backoff. Permanently failed jobs are visible somewhere (dead letter queue, failed job dashboard) for manual investigation.

**Gold standard:** All async work is enqueued, not processed in request handlers. Jobs are idempotent (safe to retry). Queue depth is monitored and alerted on. Dead letter queues are reviewed regularly. Job processing has its own observability — latency, throughput, error rate.

**If missing:** Risky. Inline processing of heavy work (sending emails in the request handler, generating PDFs synchronously) makes request latency unpredictable and ties the user's experience to the performance of background operations.

---

## 8. Operational

The team can operate the system — diagnose problems, respond to incidents, and understand what they're running.

### Health check endpoint

**What it means:** The service exposes an endpoint (typically `/health` or `/healthz`) that reports whether it's alive and ready to serve traffic. Used by load balancers, container orchestrators, and monitoring systems.

**How to check:** Does the endpoint exist? Does it check dependencies (database connectivity, required services) for readiness, or does it just return 200? Is it configured in the load balancer and container orchestrator? Does it respond quickly (under 1 second)?

**Good enough:** A `/health` endpoint exists, returns 200 when the service can serve requests, and returns a non-200 when it can't. It's configured in the load balancer's health check.

**Gold standard:** Separate liveness and readiness endpoints. Readiness checks database connectivity and critical dependencies. Both respond in under 200ms. Health check status is included in metrics. The health check doesn't have side effects (no writes, no heavy computation).

**If missing:** Blocking for containerized or load-balanced deployments. Without a health check, the load balancer can't tell if an instance is dead, and the orchestrator can't restart a stuck process. The result is traffic routed to dead instances.

### Runbook for common failure modes

**What it means:** A document that tells an on-call engineer what to do when something goes wrong. Not a general "how to debug" guide, but specific scenarios: "Alert X fired, here's what to check and what to do."

**How to check:** Does a runbook exist? Does it cover the alerts that are configured? Does it include dashboard links, log queries, and step-by-step remediation? When was it last updated?

**Good enough:** A runbook exists for the top 5 most likely failure modes. Each entry includes: what the alert means, what to check first, how to mitigate, and when to escalate. Dashboard and log links are included.

**Gold standard:** Every alert has a corresponding runbook entry. The runbook is linked directly from the alert (PagerDuty, OpsGenie, etc.). Runbooks are updated after every incident. New team members can follow the runbook without prior knowledge of the system.

**If missing:** Risky. Without a runbook, the on-call engineer's effectiveness depends entirely on their personal knowledge of the system. At 3 AM, that knowledge is unreliable.

### On-call rotation staffed

**What it means:** There is a defined on-call rotation with at least two people, so that alerts always reach someone. The rotation is configured in an on-call tool, not informal.

**How to check:** Is there an on-call schedule in PagerDuty, OpsGenie, or equivalent? Are there at least two people in the rotation? Is the rotation current (not a schedule created six months ago that nobody updates)?

**Good enough:** An on-call rotation exists with at least two people. The schedule is current. Escalation is configured.

**Gold standard:** The rotation is large enough that each person is on call no more than one week per month. On-call handoff includes a summary of recent issues. On-call load (number of pages, mean time to resolve) is tracked and used to prioritize reliability work.

**If missing:** Blocking for any service that receives real user traffic. Without on-call, overnight outages go unnoticed until morning. Weekend issues wait until Monday.

### Incident response process documented

**What it means:** The team knows what to do when something goes seriously wrong — who declares an incident, how it's communicated, what the severity levels mean, and how the postmortem works.

**How to check:** Is there an incident response document? Does it define severity levels? Does it define roles (incident commander, communicator)? Is there a communication channel for incidents (Slack channel, status page)?

**Good enough:** The team has a shared understanding of how to handle incidents: who to contact, where to communicate, and how to declare an incident. Severity levels are defined (even if informal).

**Gold standard:** A written incident response process with defined severity levels, roles, and communication templates. A status page for external communication. Postmortems are conducted for every significant incident and result in action items that are actually completed.

**If missing:** Risky. Without a process, incidents are handled ad-hoc — everyone does their own thing, communication is scattered, and the same incidents recur because there's no postmortem.

### Dependency inventory

**What it means:** The team knows what external services, APIs, databases, and infrastructure the system depends on. For each dependency: what it does, what happens if it's down, who owns it, and how to check its status.

**How to check:** Is there a list of dependencies? For each: is there a status page URL? Is there a fallback if it's down? Does the team know the SLA of each dependency?

**Good enough:** The team can list every external dependency from memory (or documentation). For each critical dependency, the team knows what happens if it goes down.

**Gold standard:** A dependency inventory document listing: name, purpose, owner, status page URL, SLA, what happens when it's down, and the fallback strategy. Updated when dependencies are added or removed. Used as input for disaster recovery planning.

**If missing:** Nice-to-have in isolation, but the absence makes incident response harder. When something breaks and the team doesn't know what depends on what, diagnosis takes longer than it should.
