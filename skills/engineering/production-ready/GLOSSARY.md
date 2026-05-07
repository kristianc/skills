# GLOSSARY.md

The vocabulary the skill uses. Precise terms prevent ambiguity in readiness discussions. "We need better monitoring" is vague. "We need alerts on SLO violations for the checkout flow" is actionable.

## Terms

### Production readiness

The state where a system can be operated by someone who didn't write it, at scale, without the original author on call. The test: if the person who built it goes on vacation, can the on-call engineer understand what's happening, diagnose problems, and recover from failures using only the system's own observability and documentation?

**Common misuse:** Treating "it passes tests" as production-ready. Tests verify correctness. Production readiness verifies operability — that the system can be monitored, debugged, deployed, and rolled back by humans who are not the author.

### Observability

The ability to understand what a system is doing from its external outputs — logs, metrics, and traces. Not the same as monitoring (which is watching specific signals for known conditions). Observability answers questions you didn't think to ask in advance.

**The three pillars:**

- **Logs** — discrete events. "User 123 created a workspace at 14:32:01." Structured logs (JSON with consistent fields) are searchable. Unstructured logs (free-form strings) are not.
- **Metrics** — aggregated measurements over time. Request count, error rate, p99 latency, queue depth. Metrics tell you "something is wrong" but not "what specifically is wrong."
- **Traces** — the path of a single request across services. When a request touches service A, then B, then C, a trace connects the dots so you can see where the time was spent and where it failed.

**Common misuse:** Equating observability with "we have logging." Logging is one pillar. A system that logs everything but has no metrics or traces is observable in theory but opaque in practice — you can find a specific request's history but you can't answer "is the system healthy right now?" without reading individual log lines.

**Example:** A checkout flow takes 8 seconds. Logs tell you each step happened. Metrics tell you latency spiked at 2:15 PM. A trace tells you that 7 of those 8 seconds were spent waiting for the payment gateway.

### SLO (Service Level Objective)

An internal target for how well a service should perform — expressed as a percentage over a time window. "99.9% of requests complete in under 500ms over a rolling 30-day window." SLOs are chosen by the team based on what users actually need, not what sounds impressive.

**Common misuse:** Setting SLOs at 99.99% because it sounds better than 99.9%. The difference is 52 minutes of allowed downtime per year vs. 8.7 hours. If the team can't actually maintain 99.99%, the SLO is fiction — alerts fire constantly, the error budget is always exhausted, and the team ignores both.

### SLI (Service Level Indicator)

The metric that measures whether the SLO is being met. If the SLO is "99.9% of requests under 500ms," the SLI is the actual measured percentage of requests under 500ms. The SLI is a number you can look at right now; the SLO is the target that number should meet.

**Common misuse:** Using server-side metrics as SLIs when the user experience happens client-side. The server might respond in 200ms, but if the client takes 3 seconds to render it, the user's experienced latency is 3.2 seconds.

### SLA (Service Level Agreement)

A contractual commitment to customers — backed by consequences (credits, refunds, contract terms) if violated. SLAs should always be less strict than SLOs. If your SLO is 99.9%, your SLA might be 99.5%. The gap between SLA and SLO is your safety margin.

**Common misuse:** Treating SLOs and SLAs as the same thing. SLOs are internal engineering targets that can be revised. SLAs are legal commitments that trigger financial consequences. Violating an SLO means the team needs to prioritize reliability work. Violating an SLA means the business owes customers money.

### Error budget

The allowed amount of unreliability over a time window, derived from the SLO. If the SLO is 99.9% over 30 days, the error budget is 0.1% — about 43 minutes. When the budget is healthy, the team can ship features and take risks. When the budget is exhausted, the team focuses on reliability.

**Example:** The team has a 99.9% availability SLO. It's day 20 of the month and they've used 30 of their 43 allowed minutes of downtime. They have 13 minutes left for the remaining 10 days. This is tight — risky deployments should wait, or the team should invest in reducing deployment risk first.

**Common misuse:** Not actually tracking the error budget. An SLO without a tracked error budget is just a number on a wiki page. The budget needs to be visible and to actually influence decisions about what to ship and when.

### Blast radius

The scope of impact when something fails. A function that crashes affects one request. A service that crashes affects all its consumers. A database that corrupts affects everything downstream. The blast radius determines how much caution a change deserves.

**Example:** Deploying a change to the notification service has a small blast radius — if it breaks, users don't get notifications but can still use the product. Deploying a change to the authentication service has a large blast radius — if it breaks, no one can log in.

**Common misuse:** Ignoring blast radius when planning deployments. A low-risk code change deployed to a high-blast-radius service is still a high-risk deployment.

### Rollback

Reverting a deployment to the previous known-good state. A successful rollback means the system returns to the state it was in before the broken deployment, including database state if migrations were involved.

**Common misuse:** Assuming rollback is free. A deployment that runs a database migration may not be rollback-safe — the old code may not work with the new schema, and the migration may not be reversible. Rollback must be tested, not assumed.

### Canary deploy

Deploying a new version to a small subset of traffic (e.g., 5%) and monitoring for errors before rolling it out to everyone. The canary absorbs the risk — if it fails, only 5% of users are affected, and the deploy can be rolled back before reaching 100%.

**Example:** Deploy v2.3.1 to 5% of traffic. Monitor error rate and latency for 15 minutes. If error rate stays below 0.1% and p99 latency stays under 500ms, promote to 25%, then 50%, then 100%. If any threshold is breached, roll back to v2.3.0.

**Common misuse:** Running a canary for 2 minutes with no defined success criteria. A canary without metrics thresholds and a minimum bake time is a formality, not a safety mechanism.

### Feature flag

A runtime toggle that enables or disables functionality without deploying new code. Feature flags decouple deployment (putting code in production) from release (making functionality available to users). The code ships dark, and the flag turns it on — for everyone, for a percentage, or for specific users.

**Common misuse:** Feature flags that are never cleaned up. A codebase with 200 feature flags, 180 of which are permanently on, has 180 sources of dead-code confusion and potential bugs. Feature flags need a retirement process — once the feature is fully launched and stable, remove the flag and the old code path.

### Circuit breaker

A pattern that stops calling a failing dependency after a threshold of failures, returning a fallback response instead. Like an electrical circuit breaker — it "trips" to prevent cascading failure, then periodically retries to see if the dependency has recovered.

**States:** Closed (normal — requests go through), Open (tripped — requests get the fallback immediately), Half-open (testing — one request goes through to check if the dependency recovered).

**Example:** The product calls a recommendation engine. The circuit breaker is configured to trip after 5 consecutive failures. After it trips, users see a default "Popular items" list instead of personalized recommendations. Every 30 seconds, the breaker lets one request through to check if the engine is back.

**Common misuse:** Setting the failure threshold too high (100 failures before tripping, by which time downstream is overwhelmed) or too low (1 failure trips it, causing false positives on network blips). The threshold should reflect how many failures indicate a real outage vs. transient noise.

### Graceful degradation

The ability to continue providing partial service when a dependency or component fails. Instead of showing a 500 error, the system provides a degraded but usable experience.

**Example:** The search service goes down. Instead of showing "Something went wrong," the product shows a cached version of popular results with a notice: "Search results may be out of date." Users can still browse, even if they can't search for specific items.

**Common misuse:** Confusing graceful degradation with error handling. Error handling catches the exception. Graceful degradation decides what to show the user instead. A caught exception that returns a generic error page is error handling, not graceful degradation.

### Health check

An endpoint (typically `/health` or `/healthz`) that reports whether the service is running and can serve requests. Health checks are used by load balancers, orchestrators (Kubernetes), and monitoring systems to determine if a service instance should receive traffic.

**Two types:**

- **Liveness** — "is the process running?" If this fails, restart the process. Should be cheap and fast — don't check dependencies.
- **Readiness** — "can this instance serve requests?" Checks that the service has connected to its database, loaded its configuration, and is ready to handle traffic. If this fails, stop sending traffic but don't restart.

**Common misuse:** A health check that always returns 200 regardless of the service's actual state. Also: a liveness check that queries the database — if the database is slow, the liveness check times out, the orchestrator restarts the service, which creates more database connections, which makes the database slower.

### Runbook

A document that tells an on-call engineer what to do when a specific alert fires. The runbook describes the symptom, the likely cause, the diagnostic steps, and the remediation steps. Written by the team that built the service, used by whoever is on call at 3 AM.

**What a good runbook contains:** Alert name and description, severity, who to escalate to, what to check first (dashboard links, log queries), common causes with fixes, and when to page the service owner vs. when to handle it yourself.

**Common misuse:** Runbooks that say "investigate and fix." If the on-call engineer knew what to investigate, they wouldn't need the runbook. Also: runbooks that are never updated after the system changes, so they describe a system that no longer exists.

### Migration safety

The set of practices that ensure database schema changes don't cause downtime, data loss, or backward-incompatibility. Safe migrations can be deployed independently of application code, can be reversed, and don't lock tables for extended periods.

**Example:** Adding a column is safe. Renaming a column is unsafe — the old code references the old name, so renaming it will break the running application. The safe approach: add the new column, deploy code that writes to both, backfill the new column, deploy code that reads from the new column, then drop the old column.

**Common misuse:** Running `ALTER TABLE` on a table with 50 million rows without testing the lock time. On MySQL with certain storage engines, an `ALTER TABLE` can lock the table for minutes, causing downtime for every query that touches that table.

### Idempotency

An operation is idempotent if performing it multiple times produces the same result as performing it once. Critical for retry logic — if a payment request is retried due to a timeout, an idempotent implementation charges the customer once, not twice.

**Example:** `PUT /users/123 { name: "Alice" }` is idempotent — calling it three times still results in the name being "Alice." `POST /payments { amount: 50 }` is not idempotent by default — calling it three times creates three payments. Making it idempotent requires an idempotency key: `POST /payments { amount: 50, idempotency_key: "abc-123" }` — the server checks if it already processed that key.

**Common misuse:** Assuming all POST endpoints need idempotency. Read operations (GET) are naturally idempotent. Only mutations that can be retried need explicit idempotency handling — and only when the retry would cause harm (duplicate charges, duplicate records, duplicate notifications).

### Backpressure

A mechanism that lets a system signal to its callers that it's overloaded, causing them to slow down rather than continuing to pile on requests. Without backpressure, an overloaded system accepts more work than it can handle, queues grow without bound, latency spikes, and eventually the system crashes.

**Example:** A queue consumer processes 100 messages per second. The producer suddenly sends 500 per second. With backpressure, the queue rejects messages above a threshold, the producer receives a "slow down" signal (HTTP 429, or a full queue), and it reduces its rate. Without backpressure, the queue grows until memory is exhausted and the consumer crashes.

**Common misuse:** Conflating backpressure with rate limiting. Rate limiting protects against abuse — it caps external clients at a fixed rate regardless of system load. Backpressure is adaptive — it responds to actual system load. A system needs both: rate limiting at the edge (protecting against abuse) and backpressure internally (protecting against overload).
