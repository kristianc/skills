# GLOSSARY.md

The vocabulary the skill uses. Security has a lot of overlapping terminology borrowed from different communities — penetration testing, compliance, software engineering, risk management. The point of this glossary is to fix the meaning of terms the skill relies on and use them consistently. Sloppy vocabulary produces sloppy analysis: confusing authentication with authorization, or vulnerability with exploit, leads to wrong priorities.

## Core terms

**Attack surface** — every point where untrusted input enters the system. An HTTP endpoint that accepts a JSON body is attack surface. A database query that interpolates user input is attack surface. A file upload handler, a webhook receiver, a URL parameter parsed by the frontend, a cookie read by server middleware — all attack surface. The goal of a security review is to enumerate the attack surface and check each point for weaknesses.

Common misuse: treating attack surface as a synonym for "the parts of the code that feel risky." Attack surface is concrete and enumerable. If untrusted data touches it, it's attack surface. If it only processes trusted internal data, it's not — though the boundary where internal meets external is itself attack surface.

**Threat model** — a structured assessment of what could go wrong, who would do it, how they'd do it, and what the impact would be. A threat model answers: who are the adversaries (anonymous users, authenticated users, insiders, automated scripts), what are they after (data theft, privilege escalation, denial of service, account takeover), and which attack surfaces would they target?

Common misuse: using "threat model" to mean "a list of scary things." A threat model is specific about actors and their capabilities. "Someone might hack us" is not a threat model. "An unauthenticated attacker could enumerate valid email addresses via the registration endpoint's error messages, enabling targeted phishing" is.

**Vulnerability** — a concrete, exploitable weakness in the system. A SQL injection in a login form is a vulnerability. A missing authorization check on an API endpoint is a vulnerability. A hardcoded API key in source code is a vulnerability. The key word is "exploitable" — there must be a realistic path from the weakness to actual harm.

Common misuse: conflating vulnerability with risk or concern. "We're using an older version of React" is a concern. "We're using a version of React with CVE-2024-XXXX, which allows XSS via dangerouslySetInnerHTML in our user profile component" is a vulnerability. The difference is specificity and a concrete exploitation path.

**Exploit** — the specific technique or steps used to take advantage of a vulnerability. The vulnerability is the unlocked door; the exploit is walking through it. This skill identifies vulnerabilities and describes how they could be exploited in general terms. It does not produce working exploit code.

Common misuse: using "exploit" and "vulnerability" interchangeably. A vulnerability can exist without a known exploit (theoretical weakness). An exploit requires a vulnerability (you can't walk through a locked door). Severity assessment considers both — a vulnerability with a trivial, well-known exploit is higher priority than one requiring sophisticated, multi-step exploitation.

**Severity** — the impact if a vulnerability is successfully exploited. Measured by what an attacker gains: read access to all user data (high), ability to execute arbitrary code on the server (critical), access to a single user's non-sensitive preferences (low). Severity is about consequences, not likelihood.

The four levels, used consistently throughout the skill:

- **Critical** — full system compromise, arbitrary code execution, access to all user data, ability to impersonate any user, secrets exposed that grant access to production infrastructure.
- **High** — access to other users' sensitive data, privilege escalation from regular user to admin, ability to modify other users' data, authentication bypass.
- **Medium** — access to non-sensitive data of other users, limited information disclosure, denial of service against individual users, session fixation requiring user interaction.
- **Low** — information disclosure that aids further attacks but isn't directly harmful (version numbers, internal paths, stack traces in errors), missing security headers that don't have a direct exploitation path in context.
- **Informational** — deviations from best practice that don't have a current exploitation path but indicate weak security posture. Missing HSTS on a site that doesn't handle sensitive data. Verbose error messages on an internal tool.

**CVSS (Common Vulnerability Scoring System)** — the industry-standard framework for rating vulnerability severity on a 0-10 scale. This skill uses a simplified version — the four-axis scoring in SCORING-RUBRIC.md — rather than full CVSS, because full CVSS requires information (network topology, compensating controls) that a code review often can't determine. When communicating with security teams, mapping findings to approximate CVSS ranges helps: critical = 9.0-10.0, high = 7.0-8.9, medium = 4.0-6.9, low = 0.1-3.9.

**Authentication** — verifying who someone is. "Prove you are who you claim to be." Login forms, API keys, OAuth tokens, session cookies, JWTs — all authentication mechanisms. Authentication answers the question "who is this?"

**Authorization** — verifying what someone is allowed to do. "Now that I know who you are, can you do this specific thing?" Role checks, permission systems, access control lists, resource ownership verification — all authorization mechanisms. Authorization answers the question "is this allowed?"

Common misuse: conflating the two, or assuming authentication implies authorization. A user can be fully authenticated (we know exactly who they are) and still unauthorized for an action (they don't have permission). The most common access control vulnerability is checking authentication but not authorization — the endpoint verifies the user is logged in but doesn't verify they own the resource they're accessing.

**Least privilege** — the principle that every actor, component, and process should have only the minimum permissions needed to perform its function. A database user that only reads from one table should not have write access to all tables. An API key that only needs to send emails should not have access to billing data. A frontend that only needs to display user profiles should not receive admin tokens.

Common misuse: treating least privilege as aspirational rather than actionable. It's not "we should probably restrict this someday." It's a concrete audit: for each credential, token, role, and service account, what permissions does it have, and which of those does it actually use? The gap between "has" and "uses" is the excess privilege that an attacker inherits upon compromise.

**Defense in depth** — the principle that security should not depend on any single control. If input validation is your only defense against SQL injection, one missed field compromises the database. If parameterized queries are your only defense, a developer who bypasses the ORM for a "quick" raw query reintroduces the vulnerability. Defense in depth means both: validate input AND use parameterized queries AND limit database permissions AND monitor for anomalous queries.

Common misuse: using "defense in depth" to justify adding security controls without thinking about what each one protects against. Three controls that all defend against the same attack vector in the same way aren't depth — they're redundancy. Depth means independent layers that catch different failure modes.

**Trust boundary** — the line between contexts with different levels of trust. The boundary between the browser and the server is a trust boundary (never trust client-side validation alone). The boundary between your application and a third-party API is a trust boundary (their response could be malformed or malicious). The boundary between user input and a database query is a trust boundary (the input must be sanitized before crossing).

Common misuse: assuming trust boundaries only exist at the network edge. Internal service-to-service communication crosses trust boundaries too. A microservice that accepts requests from another internal service without validating the payload is trusting that the calling service will never be compromised, misconfigured, or have a bug that sends malformed data.

**Input validation** — verifying that data conforms to expected format, type, length, and range before the system processes it. Validation is the first line of defense at every trust boundary. A field expecting an email should reject anything that isn't an email. A field expecting a number should reject strings. A field expecting a 255-character max should reject longer input.

Common misuse: treating input validation as sufficient security. Validation is necessary but not sufficient. A perfectly validated SQL string can still cause injection if it's concatenated into a query instead of parameterized. Validation reduces attack surface; it doesn't eliminate vulnerability classes. It's one layer in defense in depth.

**Output encoding** — transforming data before rendering it in a different context so that it's treated as data, not code. When user input is displayed in HTML, it must be HTML-encoded so that `<script>` becomes `&lt;script&gt;` and is displayed as text, not executed as JavaScript. When user input is inserted into a SQL query, it must be parameterized so that `'; DROP TABLE users--` is treated as a string value, not as SQL.

Common misuse: encoding once at input time instead of at output time. The same data may be rendered in HTML, JSON, SQL, and URL contexts — each requires different encoding. Encoding at input time either over-encodes (breaking legitimate data) or under-encodes (missing a context). Encode at the boundary where data crosses into a new context.

**Secrets management** — the practice of keeping credentials, API keys, tokens, and other sensitive values out of source code and into secure, access-controlled storage. Secrets in code are visible to everyone with repository access, persisted in git history forever (even after "deletion"), and impossible to rotate without a code change. Secrets belong in environment variables, secret managers (AWS Secrets Manager, HashiCorp Vault, Doppler), or encrypted configuration — never in source files, never in git.

Common misuse: thinking ".env files are secrets management." An .env file is better than hardcoding, but it's not secrets management — it's a file on disk that can be committed accidentally, copied insecurely, or read by any process with filesystem access. Real secrets management provides access control, audit logging, rotation, and encryption at rest.
