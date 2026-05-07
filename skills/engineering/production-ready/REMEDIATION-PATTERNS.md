# REMEDIATION-PATTERNS.md

Standard fix patterns for each readiness dimension. For every finding, this is the how — the concrete implementation approach, the common mistakes, and how to verify the fix works. Patterns are framework-agnostic but include specific library recommendations where one library is clearly the standard.

---

## Error handling patterns

### Structured error types

**The pattern:** Define a hierarchy of error types that the application uses instead of generic `Error` or bare strings. Each type carries semantic meaning that error handlers can use to determine the response.

```
AppError (base)
├── ValidationError    → 400, show field-level details to user
├── AuthenticationError → 401, redirect to login
├── AuthorizationError  → 403, show "you don't have permission"
├── NotFoundError       → 404, show "not found" with navigation
├── ConflictError       → 409, show what conflicted and how to resolve
├── RateLimitError      → 429, show retry-after
├── ExternalServiceError → 503, show degraded experience or retry
└── InternalError       → 500, show generic message, log full details
```

The error handler maps error types to HTTP status codes and response shapes. Business logic throws typed errors. The handler layer translates them.

**Common mistakes:**
- Defining error types but not using them consistently (half the codebase still throws `new Error("something")`)
- Putting HTTP status codes in business logic (the domain shouldn't know about HTTP)
- Creating too many error types — 5-8 covers most applications. If you have 40 error types, you've rebuilt an exception hierarchy no one can remember

**How to verify:** Search for `throw new Error(` or `raise Exception(` — every instance should use a typed error instead. Search for catch blocks — they should branch on error type, not on string matching error messages.

### Retry middleware

**The pattern:** Wrap external calls in retry logic that handles transient failures automatically. Configure per-dependency: how many retries, what backoff strategy, which errors are retriable.

**Key decisions:**
- **Which errors to retry:** Timeouts, 503s, 429s (with respect for Retry-After), connection resets. Never retry 400s, 401s, 403s, 404s — these are permanent failures.
- **Backoff strategy:** Exponential with jitter. Base delay of 100-500ms, multiplied by 2 on each retry, with random jitter of +/- 50%. Jitter prevents thundering herds when many clients retry simultaneously.
- **Maximum retries:** 2-3 for user-facing operations (they can't wait forever), 5-10 for background jobs.

**Libraries:**
- Node.js: `axios-retry`, `cockatiel`, `p-retry`
- Python: `tenacity`, `backoff`, `urllib3` (built-in retry)
- Java: `resilience4j`, `spring-retry`

**Common mistakes:**
- Retrying without backoff (hammers the struggling dependency)
- Retrying non-idempotent operations without an idempotency key (creates duplicates)
- No maximum retry count (retries forever, consuming resources)
- Retrying 400 errors (the same invalid request will fail the same way every time)
- Fixed delay instead of exponential backoff with jitter (causes retry storms)

**How to verify:** Check that the retry configuration exists. Check that it uses exponential backoff. Check that non-retriable errors are excluded. Check that there's a maximum retry count. Run a test where the dependency returns 503 three times then succeeds — the call should succeed after retries.

### Circuit breaker implementation

**The pattern:** Wrap dependency calls in a circuit breaker that tracks failures and "trips" when a threshold is reached. When tripped, calls fail fast with a fallback instead of waiting for a timeout.

**Key decisions:**
- **Failure threshold:** 5-10 consecutive failures, or 50% failure rate over a 30-second window. The threshold should distinguish between "the dependency is down" and "one request had a network blip."
- **Reset timeout:** 30-60 seconds. After the breaker trips, it periodically lets one request through (half-open state) to check if the dependency has recovered.
- **Fallback:** What to return when the breaker is open. Cached data, default values, or an honest "this feature is temporarily unavailable." Never silently return empty data if the user expects results.

**Libraries:**
- Node.js: `opossum`, `cockatiel`
- Python: `pybreaker`, `circuitbreaker`
- Java: `resilience4j`

**Common mistakes:**
- No fallback — the breaker trips and the user gets a raw error instead of a degraded experience
- Threshold too low — one network blip trips the breaker, and the dependency is "down" for 30 seconds when it was fine
- Threshold too high — 100 failures before tripping, by which time the connection pool is exhausted
- Not exposing breaker state as a metric — the team doesn't know the breaker tripped until they investigate a degraded experience report

**How to verify:** Check that the breaker is configured with a reasonable threshold. Check that a fallback is defined. Check that breaker state is exposed as a metric or logged. Test by making the dependency fail and verifying the fallback is served.

---

## Observability patterns

### Structured logging setup

**The pattern:** Replace unstructured log calls (`console.log("User created: " + userId)`) with structured JSON logging (`logger.info({ event: "user_created", userId, duration })`) using a logging library that outputs consistent JSON.

**Key fields to include on every log entry:**
- `timestamp` — ISO 8601
- `level` — info, warn, error
- `message` — human-readable description
- `requestId` — correlation ID for tracing a request across log entries
- `service` — the service name (for multi-service architectures)
- `event` — machine-readable event name (for searching and aggregating)
- `duration` — for operations that have meaningful timing

**Libraries:**
- Node.js: `pino` (fast, structured), `winston` (flexible, structured)
- Python: `structlog`, `python-json-logger`
- Java: `logback` with JSON encoder, `log4j2` with JSON layout

**Common mistakes:**
- Logging sensitive data — passwords, tokens, PII, credit card numbers. Set up a redaction layer that strips sensitive fields before writing.
- Logging too much — verbose logging in production fills disks and makes search slow. Use debug level for verbose output and only enable it when investigating.
- Logging too little — only logging errors misses the context that makes errors debuggable. Log the start and completion of significant operations.
- Inconsistent fields — one service logs `userId`, another logs `user_id`, another logs `uid`. Standardize field names across services.

**How to verify:** Run the application and check log output. Is it JSON? Does every entry have a timestamp, level, and request ID? Search for `console.log` — there should be none in production code (all logging through the structured logger).

### OpenTelemetry integration

**The pattern:** Instrument the application with OpenTelemetry to collect traces and metrics. OpenTelemetry is the vendor-neutral standard — it exports to any backend (Jaeger, Datadog, Grafana Tempo, Honeycomb).

**Setup steps:**
1. Install the SDK and auto-instrumentation packages for your framework (express, fastify, django, flask, etc.)
2. Configure the exporter to send to your backend
3. Auto-instrumentation covers HTTP calls, database queries, and framework middleware automatically
4. Add custom spans for business-critical operations that aren't covered by auto-instrumentation

**Common mistakes:**
- Installing OpenTelemetry but not configuring trace sampling — sending 100% of traces to the backend is expensive at scale. Start with 10-20% sampling and increase for specific operations.
- Not adding custom spans — auto-instrumentation shows HTTP and database calls but not business logic. If the trace shows "500ms in the request handler" but not "400ms building the recommendation," you've lost the useful information.
- Not propagating trace context — traces break at service boundaries if the context isn't passed in HTTP headers. Most auto-instrumentation handles this, but custom HTTP clients may need manual propagation.

**How to verify:** Make a request and check the tracing backend for a trace. Does it show the full request lifecycle? Are custom spans present for important operations? For multi-service architectures, does the trace span across service boundaries?

### Dashboard templates

**The pattern:** Create dashboards that answer the three fundamental questions: Is it working? (RED metrics) How loaded is it? (USE metrics) What changed? (deploy annotations)

**RED dashboard (request-focused):**
- Request rate (per endpoint, per status code)
- Error rate (percentage of requests returning 5xx)
- Duration (p50, p95, p99 latency per endpoint)

**USE dashboard (resource-focused):**
- Utilization — CPU usage, memory usage, disk usage
- Saturation — connection pool usage, queue depth, thread count
- Errors — resource-level errors (disk full, out of memory, connection refused)

**Business dashboard:**
- Key business metrics: signups, conversions, payments, active users
- Alongside technical metrics so the team can correlate "signups dropped 50%" with "error rate spiked at 2:15 PM"

**Common mistakes:**
- Dashboards that show averages instead of percentiles — the average hides the misery. Always show p50, p95, and p99.
- Too many panels — a dashboard with 40 panels requires expertise to interpret. The overview dashboard should have 6-10 panels that are instantly readable. Detailed dashboards can have more.
- No deploy annotations — without knowing when a deploy happened, it's hard to correlate metric changes with code changes.

**How to verify:** Open the dashboard. Can you tell within 5 seconds whether the system is healthy? Can you tell when the last deploy happened? Can you see if latency or errors spiked? If yes, the dashboard is working.

---

## Database patterns

### Migration safety: expand-contract

**The pattern:** Instead of changing a column in place (which requires the old code to be updated simultaneously), use a multi-step process:

1. **Expand** — add the new column (or table) alongside the old one
2. **Migrate code** — update the application to write to both old and new columns
3. **Backfill** — copy data from the old column to the new column
4. **Switch reads** — update the application to read from the new column
5. **Contract** — remove the old column (in a later migration, after the code change is stable)

Each step is independently deployable and reversible. If step 3 fails, you still have the old column with valid data.

**When to use it:** Renaming columns, changing column types, splitting or merging columns, moving data between tables. Not needed for adding new columns or new tables (those are always safe).

**Common mistakes:**
- Skipping the dual-write step — going straight from old column to new column means a deploy failure leaves the application writing to a column that doesn't exist
- Not backfilling — the new column exists but is empty for old records, causing null pointer errors
- Contracting too early — removing the old column before confirming the new code is stable. Wait at least one full deploy cycle.

**How to verify:** Check that the migration only adds things (new column, new table, new index). If it changes or removes anything, verify the expand-contract pattern is being followed.

### Connection pool tuning

**The pattern:** Configure the connection pool size based on the formula: `pool_size = (number_of_app_instances * connections_per_instance) <= database_max_connections * 0.8`. Leave 20% headroom for admin connections, migration scripts, and monitoring.

**Key settings:**
- **Pool size:** Start with 10-20 connections per instance. Increase only if connection wait time is high.
- **Idle timeout:** Close idle connections after 30-60 seconds to free resources. Shorter for serverless (where connections may not be reused).
- **Connection timeout:** How long to wait for a connection from the pool. 5-10 seconds. Fail fast rather than queue indefinitely.
- **Statement timeout:** Maximum query execution time. 30 seconds for most queries, longer for known expensive operations.

**Common mistakes:**
- Setting pool size too high — 100 connections per instance across 10 instances is 1,000 connections to a database that might only support 200. The database runs out of memory and crashes.
- No idle timeout — connections accumulate and are never released, eventually exhausting the database's connection limit
- Missing pool for serverless — every Lambda invocation opens a new connection, quickly overwhelming the database. Use an external pool (RDS Proxy, PgBouncer).

**How to verify:** Check the pool configuration. Calculate total connections across all instances and compare to the database's max_connections. Monitor pool metrics: active connections, idle connections, wait time. If wait time is consistently high, the pool is too small. If idle connections are consistently high, the pool is too large.

---

## Performance patterns

### Query optimization

**The pattern:** For every slow query (identified by slow query log, APM, or `EXPLAIN ANALYZE`), apply this checklist:

1. **Is there an index?** Check that the columns in `WHERE`, `JOIN`, and `ORDER BY` clauses are indexed. Use `EXPLAIN` to verify the query uses the index.
2. **Is it an N+1?** If the query runs inside a loop, replace with a batch query or eager loading.
3. **Is it selecting too much?** `SELECT *` when you only need two columns wastes memory and network bandwidth.
4. **Is the join order optimal?** The database optimizer usually handles this, but for complex queries, check the execution plan.
5. **Can it be cached?** If the query returns the same result for the same input and the data changes infrequently, cache it.

**Common mistakes:**
- Adding indexes without considering write performance — every index slows down inserts and updates. Index the columns that are actually queried, not every column.
- Premature optimization — don't optimize queries that run once a day and take 2 seconds. Focus on queries that run thousands of times per minute.
- Caching without invalidation strategy — cached data that's never refreshed serves stale results. Define the TTL and the invalidation trigger.

**How to verify:** Run `EXPLAIN ANALYZE` (PostgreSQL) or `EXPLAIN` (MySQL) on the query before and after optimization. Check that the query uses indexes (no sequential scans on large tables). Measure the actual execution time improvement.

### Load test setup

**The pattern:** Create realistic load test scenarios that simulate actual user behavior, not just hammer a single endpoint.

**Steps:**
1. Define the traffic profile: which endpoints are hit, in what ratio, and with what concurrency. Base this on real traffic patterns if available (analytics, access logs).
2. Create test data: enough to be realistic. A test with 10 users in the database doesn't reveal the same problems as a test with 1 million.
3. Run the test against an environment that matches production (same database size, same number of instances, same configuration).
4. Monitor during the test: latency percentiles, error rate, CPU, memory, connection pool, database load.
5. Identify the bottleneck: what fails first? Database connections? CPU? Memory? External dependency latency?

**Tools:** k6 (JavaScript-based, developer-friendly), Locust (Python-based, scriptable), Artillery (YAML config, good for quick tests), JMeter (Java-based, complex but powerful).

**Common mistakes:**
- Testing from the same machine as the server (network latency is zero, which doesn't reflect production)
- Testing with a tiny dataset (queries are fast when the table has 100 rows)
- Testing only throughput, not latency (the system handles 1000 RPS but p99 is 30 seconds)
- Not running the test long enough (memory leaks and connection pool exhaustion only appear over time)

**How to verify:** The load test ran to completion at 2x expected peak. Latency and error rate stayed within acceptable bounds. The bottleneck is identified and documented. The load test can be run again (it's scripted, not a one-off manual process).

---

## Deployment patterns

### Feature flag implementation

**The pattern:** Implement feature flags that can be toggled at runtime without a deployment. At minimum, support boolean flags (on/off). Ideally, support percentage rollouts (1%, 10%, 50%, 100%) and user targeting (enable for specific users or segments).

**Implementation options:**
- **Third-party service:** LaunchDarkly, Unleash, Flagsmith, Split. Best for teams that need percentage rollouts, targeting, and audit logs without building them.
- **Self-hosted:** Unleash (open source), Flipt (open source). Good balance of features and control.
- **Simple in-app:** A feature flag table in the database with an admin UI. Sufficient for small teams that need boolean flags and don't need percentage rollouts.
- **Environment variables:** The simplest option. Requires a redeploy to toggle. Not recommended for production — the whole point is toggling without deploying.

**The lifecycle of a flag:**
1. Create the flag (default: off)
2. Deploy the code behind the flag
3. Enable for internal testing
4. Enable for a percentage of users (if percentage rollout is supported)
5. Enable for all users
6. Remove the flag and the old code path (within 2 weeks of full rollout)

**Common mistakes:**
- Feature flags that are never cleaned up — the codebase accumulates dead code paths. Every flag should have an owner and a cleanup deadline.
- Testing only the "on" path — the "off" path is also code that runs in production. Test both.
- Feature flags in the critical path without a fallback — if the flag service is down, what happens? The flag evaluation should default to a safe state (usually "off" for new features).

**How to verify:** Check that the feature flag system is integrated. Check that new features are behind flags. Check that there's a process for cleaning up old flags (even if it's just a convention). Test that toggling a flag changes the behavior without a deployment.

### Canary deploy setup

**The pattern:** Deploy the new version to a small percentage of traffic, monitor for problems, then gradually increase the percentage.

**Steps:**
1. Deploy the new version to 5% of traffic (the canary)
2. Wait for a bake time (15-30 minutes) while monitoring error rate and latency
3. Compare canary metrics to the baseline (the remaining 95% on the old version)
4. If canary metrics are within threshold, promote to 25%, then 50%, then 100%
5. If canary metrics breach threshold, roll back the canary

**Metrics to monitor during canary:**
- Error rate (5xx responses)
- Latency (p50, p95, p99)
- Business metrics (conversion rate, signup rate) if available
- Saturation metrics (CPU, memory, connections)

**Common mistakes:**
- Bake time too short — 2 minutes isn't enough to catch slow-onset problems (memory leaks, connection pool exhaustion)
- No success criteria defined — the canary runs and someone eyeballs the dashboard, but there's no objective threshold for "good" vs. "bad"
- Canary traffic not representative — if the canary only gets traffic from one region or one user segment, it doesn't test the full behavior

**How to verify:** Check that the deployment pipeline supports canary or staged rollouts. Check that success criteria are defined (error rate, latency thresholds). Check that the bake time is at least 15 minutes for critical services.

### Rollback scripts

**The pattern:** Ensure that rolling back a deployment is a single command that returns the system to the previous known-good state. The rollback command should be documented in the runbook and tested.

**What a rollback includes:**
- Reverting the application code to the previous version
- If database migrations were applied: running the migration rollback (if safe) or deploying the old code that works with the new schema (if the expand-contract pattern was used)
- Verifying the system is healthy after rollback (health checks pass, error rate returns to normal)

**Common mistakes:**
- Assuming rollback means "deploy the old code" — if a migration ran, the old code might not work with the new schema
- Not documenting the rollback command — during an incident, the on-call engineer shouldn't have to figure out the rollback procedure
- Not verifying after rollback — the system might be in an inconsistent state (half-migrated data, orphaned records)

**How to verify:** Run through the rollback procedure in staging. Check that the system returns to a healthy state. Measure the time from "decide to roll back" to "system is healthy again."

---

## Resilience patterns

### Timeout middleware

**The pattern:** Set explicit timeouts on every external call — HTTP requests, database queries, cache lookups, queue operations. Use a hierarchical timeout strategy: the overall request timeout is shorter than the sum of individual call timeouts, so the request fails before all retries are exhausted.

**Recommended defaults:**
- HTTP calls to external services: 5-10 seconds
- Database queries: 30 seconds (shorter for simple lookups)
- Cache lookups: 1-2 seconds (if the cache is slow, skip it)
- Overall request timeout: 30 seconds (the user shouldn't wait longer than this)

**Common mistakes:**
- No timeout at all — the default is often "infinite" or "OS default" (which can be minutes). One hung connection ties up a thread forever.
- Timeout too long — a 5-minute timeout on an HTTP call means 5 minutes of a thread being blocked, doing nothing, waiting for a dependency that's probably dead.
- Timeout too short — a 100ms timeout on a database query means complex queries always fail. Set timeouts based on observed p99 latency with headroom.
- Not handling the timeout error — the timeout fires but the catch block doesn't distinguish it from other errors. Timeout errors should trigger a specific response (retry, fallback, or a "taking longer than expected" message).

**How to verify:** Check that every HTTP client has a timeout configured. Check that the database connection has a statement timeout. Check that there are no calls using the default "no timeout." Test by introducing artificial latency (network proxy, dependency mock) and verifying the timeout fires.

### Rate limiter setup

**The pattern:** Enforce request limits per client (IP or authenticated user) per time window. Return 429 with a `Retry-After` header when the limit is exceeded.

**Key decisions:**
- **What to limit:** Authentication endpoints (strict — 5-10 per minute per IP), public API (moderate — 100-1000 per minute per user), internal endpoints (loose or none).
- **Limit strategy:** Fixed window (simple, but allows bursts at window boundaries), sliding window (smoother, slightly more complex), token bucket (best for APIs — allows short bursts while enforcing average rate).
- **Storage:** In-memory (simple, but per-instance — doesn't work with multiple instances), Redis (shared across instances, standard for production).

**Libraries:**
- Node.js: `express-rate-limit` (simple), `rate-limiter-flexible` (Redis-backed, flexible)
- Python: `django-ratelimit`, `flask-limiter`
- At the infrastructure level: NGINX rate limiting, AWS WAF, Cloudflare rate limiting

**Common mistakes:**
- Rate limiting by IP only — misses authenticated abuse and over-limits shared IPs (offices, VPNs). Use authenticated user ID when available, fall back to IP.
- No `Retry-After` header — the client doesn't know when to try again and retries immediately, making the problem worse.
- Rate limiting too strict — a limit of 10 requests per minute on a page that makes 5 API calls means the user can load 2 pages per minute.
- Not rate limiting login — the single most important endpoint to rate limit. Without it, brute-force attacks are trivial.

**How to verify:** Check that rate limiting middleware is configured. Check that login endpoints have strict limits. Check that the response includes a `Retry-After` header. Test by exceeding the limit and verifying you get a 429 with the correct header.

### Dead letter queues

**The pattern:** When a message in a queue fails processing after all retries, move it to a dead letter queue (DLQ) instead of discarding it. The DLQ holds failed messages for investigation and reprocessing.

**Setup:**
1. Configure the main queue with a retry policy (3-5 retries with backoff)
2. Configure a DLQ that receives messages that exceed the retry limit
3. Set up monitoring on DLQ depth — any messages in the DLQ trigger an alert
4. Build a mechanism to reprocess DLQ messages (redrive) after the underlying issue is fixed

**Common mistakes:**
- No DLQ — failed messages are silently discarded. The team never learns about the failure.
- DLQ without monitoring — messages accumulate in the DLQ but no one looks. It becomes a graveyard.
- No redrive mechanism — messages are in the DLQ but there's no way to reprocess them without writing a custom script each time.
- DLQ without context — the DLQ message doesn't include why it failed. When investigating, the team has to guess.

**How to verify:** Check that a DLQ is configured for every queue. Check that DLQ depth is monitored and alerted on. Check that there's a documented process (or tool) for reprocessing DLQ messages. Test by sending a message that will always fail and verify it lands in the DLQ.
