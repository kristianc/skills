# SCORING-RUBRIC.md

How to score every finding from the assessment. Each vulnerability gets rated 1-5 on four axes. The scores drive prioritization — what to fix now, what to fix soon, and what to backlog.

The point of scoring is triage, not precision. A codebase with 40 findings needs to know which 5 to fix before launch. The scores tell you where the real risk is.

## Axis 1: Severity

What is the impact if this vulnerability is successfully exploited?

**5 — Critical.** Full system compromise. The attacker gains access to all data, can impersonate any user, can execute arbitrary code, or can access production infrastructure.

- Remote code execution via command injection
- SQL injection on a query that returns all user records
- Hardcoded AWS root credentials in source code
- JWT signed with `alg: "none"` accepted by the server
- Admin API endpoint with no authentication

The question: "If exploited, is the entire system compromised?" If yes, it's a 5.

**4 — High.** Significant damage to multiple users or significant privilege escalation. The attacker gains access they shouldn't have, but not complete system control.

- IDOR that exposes other users' sensitive data (financial records, personal messages)
- Stored XSS on a page viewed by all users (can steal sessions)
- Privilege escalation from regular user to admin
- Passwords stored with MD5 or SHA-256 (no salt, fast hash — crackable)
- Session tokens that don't invalidate on logout (session hijacking window)

The question: "If exploited, are many users affected or is sensitive data exposed?" If yes, it's a 4.

**3 — Medium.** Limited damage or requires significant conditions. The attacker gains some information or can affect individual users under specific conditions.

- Reflected XSS that requires user to click a crafted link
- CORS misconfiguration that allows cross-origin requests to non-critical endpoints
- Missing rate limiting on a public API (enables scraping, not account takeover)
- Information disclosure: stack traces revealing internal paths and library versions
- Session fixation requiring the attacker to set a cookie on the victim's browser

The question: "If exploited, is the damage contained and conditional?" If yes, it's a 3.

**2 — Low.** Minor information disclosure or security weakness that aids further attacks but isn't directly harmful.

- Missing `X-Content-Type-Options` header
- Version numbers exposed in HTTP headers
- Verbose error messages on non-sensitive endpoints
- Missing HSTS on a site that redirects HTTP to HTTPS anyway
- Email enumeration via different error messages for "email not found" vs. "wrong password"

The question: "Does this help an attacker but not directly harm users?" If yes, it's a 2.

**1 — Informational.** Best practice deviation with no current exploitation path. Worth noting for security posture improvement but not urgently actionable.

- Missing `Permissions-Policy` header
- Development dependencies with known CVEs that don't affect production
- Console.log statements that reveal non-sensitive internal state
- Missing subresource integrity (SRI) on CDN-hosted scripts that are also served over HTTPS

The question: "Would fixing this improve security posture but not address a current threat?" If yes, it's a 1.

---

## Axis 2: Exploitability

How easy is it for an attacker to exploit this vulnerability?

**5 — Trivial.** An unauthenticated attacker can exploit this with a single HTTP request, a URL manipulation, or a simple script. No special tools, no insider knowledge, no social engineering.

- SQL injection in a public search endpoint: `?q=' OR 1=1--`
- IDOR on a public API: change the ID in the URL
- Hardcoded API key visible in the public JavaScript bundle
- Open admin panel at `/admin` with no authentication
- Secret committed in a public GitHub repository

Any attacker who looks will find it. Automated scanners will find it.

**4 — Easy.** Requires authentication or minor reconnaissance, but the exploit is straightforward once the attacker has access.

- IDOR that requires a valid user session (any authenticated user can exploit it)
- Stored XSS that requires the attacker to have an account (to post the malicious content)
- Privilege escalation via mass assignment on an authenticated endpoint
- Session hijacking via missing HttpOnly flag (requires XSS first, but XSS is easy to chain)

Requires one prerequisite, but that prerequisite is easy to obtain.

**3 — Moderate.** Requires multiple steps, specific conditions, or some insider knowledge.

- Reflected XSS that requires crafting a URL and getting the victim to click it
- CSRF on a state-changing action that requires the victim to visit an attacker-controlled page while logged in
- NoSQL injection that requires understanding the query structure (not visible from the outside)
- Race condition that requires precise timing of concurrent requests

Requires effort and some luck, but a motivated attacker with basic skills can do it.

**2 — Difficult.** Requires significant expertise, insider access, or a chain of multiple vulnerabilities.

- Timing attacks against cryptographic operations (requires statistical analysis of many requests)
- Exploiting a deserialization vulnerability that requires understanding the internal object model
- Chaining three vulnerabilities: information disclosure to learn the schema, IDOR to read a token, then privilege escalation
- Supply chain attack requiring compromise of an upstream maintainer

A skilled, motivated attacker could do it, but it's not opportunistic.

**1 — Theoretical.** Requires conditions that are extremely unlikely in practice — physical access, compromise of trusted infrastructure, or cryptographic breakthroughs.

- Side-channel attacks that require co-located hardware
- Brute-forcing a properly implemented bcrypt hash with a cost factor of 12
- Exploiting a timing difference of less than a microsecond
- Vulnerabilities that require the attacker to already be a server admin

Possible in theory, not practical in reality. Include in the report for completeness but don't prioritize.

---

## Axis 3: Scope

What is the blast radius if this vulnerability is exploited?

**5 — System-wide.** All users, all data, or the entire infrastructure is affected. There is no containment.

- SQL injection that gives access to the entire database
- RCE that gives shell access to the production server
- Compromised secrets that grant access to the cloud account (AWS root key, GCP service account with owner role)
- A vulnerability in the authentication system that affects every user's login

**4 — Multi-tenant / all users.** Affects all or most users of the application, but doesn't compromise the underlying infrastructure.

- Stored XSS on a shared page (every user who views it is affected)
- An IDOR that allows enumeration of all user records
- A broken access control that lets any user read any other user's data
- Session management flaw that makes all sessions stealable

**3 — Multiple users.** Affects a subset of users or a specific feature area. The impact is significant but bounded.

- XSS in a team feature (affects all members of the targeted team)
- IDOR on a specific resource type (e.g., invoices, but not profiles)
- Missing access control on a feature used by a specific role
- Data exposure limited to a single table or resource type

**2 — Single user.** Affects only the targeted user. Exploitation must be repeated for each victim.

- Reflected XSS targeting one user at a time (requires each victim to click a link)
- CSRF that affects only the user who visits the attacker's page
- Account takeover that requires targeting a specific account
- Self-XSS (the user can only attack themselves — barely a vulnerability)

**1 — Attacker only.** The vulnerability affects only the attacker's own data or session, or requires targeting with no realistic victim.

- A race condition that can corrupt only the attacker's own data
- An information disclosure that reveals only the current user's non-sensitive data
- A missing header that has no practical impact given the application's context

---

## Axis 4: Remediation effort

How much work is required to fix this vulnerability?

**5 — Trivial fix.** Minutes. Adding a header, changing a configuration value, adding a flag to a cookie, updating a dependency.

- Add `HttpOnly` flag to session cookie
- Set `X-Content-Type-Options: nosniff` header
- Run `npm audit fix` for a patch-level dependency update
- Remove a `console.log` that leaks sensitive data
- Add `.env` to `.gitignore`

**4 — Quick fix.** Hours. Adding middleware, wrapping a function, updating a query, adding validation.

- Add authorization middleware to an unprotected route
- Replace a raw SQL query with a parameterized query
- Add rate limiting to a login endpoint
- Sanitize a specific input field
- Add CORS origin allowlist

**3 — Moderate fix.** Days. Requires changes across multiple files or components, testing across features, or a small refactor.

- Implement proper session management (creation, rotation, invalidation)
- Add Content Security Policy (requires auditing all scripts, styles, and inline handlers)
- Replace string concatenation in multiple SQL queries across the codebase
- Implement proper error handling that doesn't leak details in production
- Add input validation to all API endpoints

**2 — Significant effort.** Weeks. Requires architectural changes, data migration, or cross-team coordination.

- Migrate from in-house authentication to a battle-tested auth library
- Implement role-based access control across all endpoints
- Rehash all passwords from MD5 to bcrypt (requires migration strategy, user communication)
- Implement field-level encryption for PII in the database
- Rotate all compromised secrets and update all dependent systems

**1 — Architectural change.** Months. Requires fundamental redesign of a core system.

- Redesign a system from single-tenant to multi-tenant with proper isolation
- Rebuild the authorization model from scratch (e.g., from ad-hoc checks to policy-based)
- Migrate from a monolith where all code has database admin access to microservices with least-privilege
- Redesign the data pipeline to support encryption at rest throughout

---

## Using the scores

### Priority matrix

Combine severity and exploitability for immediate priority, then use scope and remediation effort to sequence the work:

| Priority | Condition | Action |
|----------|-----------|--------|
| **Fix now** | Severity 4-5 AND exploitability 4-5 | Drop everything. This is actively dangerous. |
| **Fix before launch** | Severity 4-5 AND exploitability 3, OR severity 3 AND exploitability 4-5 | Must be fixed before the next deployment to production. |
| **Fix soon** | Severity 3 AND exploitability 3, OR any 4+ on severity/exploitability with scope 1-2 | Schedule within the current sprint. |
| **Backlog** | Severity 1-2, OR exploitability 1-2 with severity 3 | Track it, fix it when working in that area. |
| **Accept** | Severity 1 AND exploitability 1 | Document the decision to accept the risk. |

### Breaking ties

When two findings have the same severity and exploitability:

1. **Higher scope wins.** A medium-severity vulnerability affecting all users is more urgent than one affecting a single user.
2. **Easier fix wins.** Between two equally severe findings, fix the one that takes an hour before the one that takes a week — you reduce risk faster.
3. **Data sensitivity wins.** Between two otherwise equal findings, the one touching financial or health data takes priority over one touching preferences.

### Distinguishing real vulnerabilities from theoretical concerns

The most common failure in security review is treating theoretical concerns with the same urgency as real vulnerabilities. The scoring system helps, but here are explicit tests:

**It's a real vulnerability if:**
- You can describe a concrete exploitation path (not "an attacker could..." but "an unauthenticated attacker sends this request and gets this result")
- The exploitation path uses only capabilities the attacker plausibly has
- The impact is observable (data exposed, state changed, access granted)

**It's a theoretical concern if:**
- The exploitation requires conditions you can't confirm exist ("if the attacker already had admin access...")
- The impact is speculative ("this could theoretically lead to...")
- The exploitation requires capabilities beyond the threat model (physical access to the server for a web app review)
- It's a best-practice deviation without a concrete attack scenario in this specific application

Theoretical concerns should still be documented — as informational findings (severity 1) — but they should never crowd out real vulnerabilities in the priority queue.
