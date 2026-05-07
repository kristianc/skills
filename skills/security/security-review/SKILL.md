---
name: security-review
description: Audit a codebase for security vulnerabilities — OWASP Top 10, auth/authz gaps, secrets in code, injection vectors, dependency risks, and misconfiguration. Use before launch, after adding authentication or authorization, when reviewing a PR with security implications, during periodic security audits, or when checking for OWASP Top 10 compliance. Defensive only — for finding and fixing vulnerabilities in your own product.
---

# Security Review

Systematically audit a codebase for security vulnerabilities, score them by severity and exploitability, and produce actionable remediation.

This skill is defensive. It finds and fixes vulnerabilities in your own product. It is not for attacking others, testing third-party systems without authorization, or generating exploit code. Every finding should include a fix, not just a diagnosis.

## Glossary

Use these terms precisely. Full definitions in GLOSSARY.md.

- **Attack surface** — every point where untrusted input enters the system.
- **Threat model** — the structured assessment of what could go wrong, who would do it, and how.
- **Vulnerability** — a concrete weakness that can be exploited. Not a theoretical concern.
- **Trust boundary** — the line between trusted and untrusted contexts.
- **Defense in depth** — multiple independent layers of protection, so one failure doesn't mean compromise.

## Process

### 1. Scope

Establish what is being reviewed: the full application, a specific feature, a PR, or a particular concern (e.g., "we just added OAuth, is it right?"). Ask the user if not obvious.

Define the trust model: who are the actors (anonymous users, authenticated users, admins, internal services), what should each be able to do, and where are the trust boundaries?

### 2. Inventory the attack surface

Map every point where untrusted input enters the system:

- **Authentication flows** — login, registration, password reset, OAuth, token handling, session management
- **API endpoints** — what accepts user input, what validates it, what doesn't
- **Data flows** — where user data goes (database, logs, third-party services, browser), what's encrypted, what isn't
- **External inputs** — file uploads, webhooks, URL parameters, headers, cookies, form fields
- **Secrets** — API keys, credentials, tokens in code, config files, environment variables, git history
- **Dependencies** — third-party packages, their versions, known CVEs

Use the Agent tool with `subagent_type=Explore` to walk the codebase. Search for authentication middleware, input handling, database queries, secret patterns, and dependency manifests.

### 3. Assess

Walk each surface in the inventory against the vulnerability taxonomy in VULNERABILITY-TAXONOMY.md. For each surface, check every applicable category: injection, authentication weaknesses, access control gaps, misconfiguration, data exposure, and dependency risks.

Don't stop at the first finding per surface. A single endpoint can have multiple vulnerabilities — missing input validation AND broken access control AND logging sensitive data.

### 4. Score and present findings

For each vulnerability found, score it on four axes: **severity**, **exploitability**, **scope**, and **remediation effort**. Full rubric in SCORING-RUBRIC.md.

Present findings as a prioritized list:

- **Finding** — what the vulnerability is, in plain language
- **Location** — file, function, line
- **Category** — from the taxonomy
- **Scores** — severity, exploitability, scope, remediation effort
- **Priority** — derived from scores (fix now / fix soon / backlog)
- **Evidence** — the specific code pattern that constitutes the vulnerability
- **Remediation** — what to do, with the standard fix pattern from REMEDIATION-PATTERNS.md

Ask the user which findings to fix.

### 5. Fix

For each selected finding, implement the remediation pattern from REMEDIATION-PATTERNS.md. After implementing:

- Verify the fix addresses the root cause, not just the symptom
- Check that the fix doesn't introduce new vulnerabilities
- Confirm the fix follows the defense-in-depth principle — one layer of protection, not the only layer

## Rules

- Never generate exploit code or proof-of-concept attacks. Describe vulnerabilities; don't weaponize them.
- Every finding must include a remediation. A vulnerability report without fixes is a worry list, not a security review.
- Distinguish real vulnerabilities from theoretical concerns. "This endpoint doesn't rate-limit" is real if the endpoint is public-facing login; it's theoretical if it's an internal admin tool behind a VPN.
- Don't cry wolf. False positives erode trust in the review. If you're unsure, label it "potential" and explain what would need to be true for it to be exploitable.
- Secrets found in code are always critical, regardless of context. Flag them immediately.
- Check git history for secrets, not just the current working tree. A rotated key that's still in git history is still exposed.
