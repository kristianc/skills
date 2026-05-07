# REMEDIATION-PATTERNS.md

For each vulnerability category in the taxonomy, the standard fix. Each section covers: the fix pattern, code examples, common mistakes when implementing the fix, and how to verify the fix works. The goal is to fix vulnerabilities correctly the first time — a bad fix is often worse than no fix because it creates false confidence.

## Injection

### SQL injection fix: parameterized queries

**The pattern:** Never construct SQL from strings and user input. Use parameterized queries (also called prepared statements) where the query structure and the data are sent separately to the database. The database engine treats parameters as data, not as SQL code, regardless of their content.

**Node.js (pg):**
```javascript
// Parameterized query — $1 is always treated as a value
const result = await pool.query(
  "SELECT * FROM users WHERE email = $1 AND org_id = $2",
  [email, orgId]
);
```

**Python (psycopg2):**
```python
# Parameterized query — %s placeholders, tuple of values
cursor.execute(
    "SELECT * FROM users WHERE email = %s AND org_id = %s",
    (email, org_id)
)
```

**ORM usage (Prisma, SQLAlchemy, ActiveRecord):**
```javascript
// ORM methods are parameterized by default
const user = await prisma.user.findUnique({ where: { email } });
```

**Common mistakes when implementing this fix:**

- **Parameterizing some queries but not all.** Search the entire codebase for raw SQL. The one query that was "too complex for the ORM" is usually the one with the injection.
- **Using an ORM's raw query escape hatch with concatenation.** `sequelize.query("SELECT * FROM users WHERE name = '" + name + "'")` is still injection, even inside an ORM. Use bind parameters: `sequelize.query("SELECT * FROM users WHERE name = ?", { replacements: [name] })`.
- **Parameterizing values but not identifiers.** Table names and column names can't be parameterized in most databases. If you need dynamic table/column names, use an allowlist: `if (!["name", "email", "created_at"].includes(sortBy)) throw new Error("Invalid sort column")`.

**How to verify the fix:**

- Confirm every database query uses parameterized syntax. Search for string concatenation near query calls.
- Test with injection payloads: `' OR 1=1--`, `'; DROP TABLE users--`, `" OR ""="`. The query should return no results or an error, never extra data.
- If using an ORM, search for every use of the raw query escape hatch and verify each one uses bind parameters.

### XSS fix: output encoding and CSP

**The pattern:** Encode all user-controlled data when rendering it in HTML, and use Content Security Policy as a second layer of defense.

**For HTML context — use the framework's default escaping:**
```javascript
// React — JSX auto-escapes by default. This is safe:
<p>{userInput}</p>

// If you must render HTML, use a sanitizer:
import DOMPurify from "dompurify";
<div dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(userInput) }} />
```

**For non-React contexts:**
```javascript
// Use textContent, not innerHTML
element.textContent = userInput;

// If rendering HTML from user input, always sanitize
import DOMPurify from "dompurify";
element.innerHTML = DOMPurify.sanitize(userInput);
```

**Content Security Policy (second layer):**
```
Content-Security-Policy: default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data:; connect-src 'self' https://api.example.com
```

This prevents inline scripts from executing even if XSS is present — the browser refuses to run scripts that don't match the policy.

**Common mistakes when implementing this fix:**

- **Fixing `innerHTML` in the component but not in the template.** If using a template engine (EJS, Handlebars, Jinja2), check for unescaped output directives: `<%- %>`, `{{{ }}}`, `| safe`. These bypass escaping.
- **Sanitizing on input instead of on output.** Don't sanitize when saving to the database. Sanitize when rendering. The same data may be safe in JSON but dangerous in HTML. Encoding at the wrong boundary either breaks data or misses a context.
- **CSP that's too permissive.** `script-src 'unsafe-inline' 'unsafe-eval'` defeats the purpose of CSP. If inline scripts are needed, use nonces: `script-src 'nonce-abc123'` with a fresh nonce per response.
- **Forgetting `postMessage` handlers.** XSS can come from other windows via `postMessage`. Always check `event.origin` before processing the message.

**How to verify the fix:**

- Test with `<script>alert(1)</script>` and `<img src=x onerror=alert(1)>` in every user input field that renders to HTML.
- Check that CSP is present on all HTML responses. Use browser DevTools (Console tab shows CSP violations).
- Verify that `dangerouslySetInnerHTML`, `v-html`, `innerHTML`, and template engine unescape directives are only used with sanitized input.

### Command injection fix: avoid shell, use argument arrays

**The pattern:** Never pass user input through a shell. Use `execFile` or `spawn` (Node.js), `subprocess.run` with `shell=False` (Python), or the equivalent in your language — these execute the command directly without shell interpretation.

```javascript
// Node.js — execFile passes arguments directly, no shell
const { execFile } = require("child_process");
execFile("convert", [inputPath, "-resize", "200x200", outputPath], callback);
```

```python
# Python — shell=False (the default) prevents shell interpretation
subprocess.run(["convert", input_path, "-resize", "200x200", output_path])
```

**Common mistakes:** Using `shell=True` or `exec` (instead of `execFile`) "because the command is complex." If the command genuinely needs shell features (pipes, redirects), construct the pipeline in code using the language's process APIs, not by passing a shell string.

**How to verify:** Search for `exec(`, `execSync(`, `os.system(`, `subprocess.run(` with `shell=True`. Each one must be validated as not including user input, or refactored to use argument arrays.

---

## Authentication

### Password storage: use bcrypt or argon2

**The pattern:** Hash passwords with a slow, salted, purpose-built password hashing algorithm. bcrypt and argon2 are the two accepted choices. Never use MD5, SHA-1, SHA-256, or any fast hash for passwords.

```javascript
// Node.js — bcrypt with cost factor 12
const bcrypt = require("bcrypt");
const hash = await bcrypt.hash(password, 12);

// Verification
const match = await bcrypt.compare(candidatePassword, storedHash);
```

```python
# Python — bcrypt
import bcrypt
hashed = bcrypt.hashpw(password.encode(), bcrypt.gensalt(rounds=12))

# Verification
bcrypt.checkpw(candidate.encode(), stored_hash)
```

**Cost factor guidance:** bcrypt cost factor 12 takes ~250ms on modern hardware. Increase if login latency allows. argon2id is preferred for new systems (memory-hard, resistant to GPU attacks) but bcrypt is acceptable.

**Common mistakes:**

- **Using bcrypt but with a low cost factor (e.g., 4).** This makes hashes nearly as fast to crack as SHA-256. Use 12 or higher.
- **Migrating hashes all at once.** Instead, re-hash on next successful login: verify with old hash, then re-hash with bcrypt and update the stored hash. Mark accounts that haven't logged in since migration for forced password reset.
- **Hashing the password on the client side.** This makes the hash the password — an attacker who obtains the hash can send it directly. Hash server-side only. Use TLS to protect the plaintext password in transit.

**How to verify:** Search for `md5(`, `sha1(`, `sha256(`, `crypto.createHash` near password handling code. All should be replaced with bcrypt or argon2.

### Session management: secure cookies, proper lifecycle

**The pattern:** Sessions should use HttpOnly, Secure, SameSite cookies with proper expiration, and must be invalidated server-side on logout and password change.

```javascript
// Express session with proper flags
app.use(session({
  secret: process.env.SESSION_SECRET,
  cookie: {
    httpOnly: true,     // JS can't read it (prevents XSS session theft)
    secure: true,       // only sent over HTTPS
    sameSite: "strict", // not sent with cross-origin requests
    maxAge: 3600000     // 1 hour
  },
  resave: false,
  saveUninitialized: false
}));

// Logout — destroy server-side session
app.post("/logout", (req, res) => {
  req.session.destroy(() => {
    res.clearCookie("connect.sid");
    res.redirect("/login");
  });
});

// Password change — invalidate all sessions
app.post("/change-password", auth, async (req, res) => {
  await updatePassword(req.user.id, req.body.newPassword);
  await destroyAllSessionsForUser(req.user.id); // invalidate other devices
  // create a new session for this device
  req.session.regenerate(() => {
    res.json({ success: true });
  });
});
```

**Common mistakes:**

- **Setting `secure: true` but not enforcing HTTPS.** In development, this breaks cookies. Use a conditional: `secure: process.env.NODE_ENV === "production"`.
- **Clearing the cookie without destroying the server session.** The session is still valid — an attacker with the token can still use it.
- **Not regenerating the session after login.** Session fixation: an attacker sets a session cookie before the user logs in, then uses that session after login. Regenerate on authentication.

**How to verify:** Check cookie flags in browser DevTools (Application tab). Verify logout destroys the server session by capturing the session cookie, logging out, then replaying the cookie — it should be rejected.

### Token management: JWTs done right

**The pattern:** If using JWTs, sign with a strong secret (or asymmetric keys for multi-service), set short expiration, validate all claims on every request, and never store in localStorage.

```javascript
// Signing — strong secret, short expiration, explicit algorithm
const token = jwt.sign(
  { sub: user.id, role: user.role },
  process.env.JWT_SECRET, // 256+ bit random secret
  { algorithm: "HS256", expiresIn: "15m" }
);

// Verification — explicit algorithm to prevent alg:none attacks
const payload = jwt.verify(token, process.env.JWT_SECRET, {
  algorithms: ["HS256"] // reject tokens with any other algorithm
});
```

**Common mistakes:**

- **Not specifying `algorithms` in `jwt.verify`.** Without this, some libraries accept `alg: "none"`, allowing anyone to forge tokens.
- **Long-lived JWTs with no refresh mechanism.** A 30-day JWT that's stolen gives the attacker 30 days. Use short-lived access tokens (15 minutes) with a refresh token flow.
- **Putting sensitive data in the JWT payload.** JWTs are base64-encoded, not encrypted. Anyone can decode the payload. Don't include passwords, internal IDs the user shouldn't see, or PII beyond what's needed.

---

## Access control

### IDOR fix: scope queries to the authenticated user

**The pattern:** Every database query for user-specific data must include the authenticated user's ID as a filter, not just the resource ID. Alternatively, look up the resource by ID, then verify ownership before returning it.

```javascript
// Pattern 1 — scope the query (preferred)
app.get("/api/documents/:id", auth, async (req, res) => {
  const doc = await db.query(
    "SELECT * FROM documents WHERE id = $1 AND owner_id = $2",
    [req.params.id, req.user.id]
  );
  if (!doc) return res.status(404).json({ error: "Not found" });
  res.json(doc);
});

// Pattern 2 — fetch then verify (when ownership is complex)
app.get("/api/documents/:id", auth, async (req, res) => {
  const doc = await db.query("SELECT * FROM documents WHERE id = $1", [req.params.id]);
  if (!doc) return res.status(404).json({ error: "Not found" });
  if (!await userCanAccess(req.user, doc)) {
    return res.status(404).json({ error: "Not found" }); // 404, not 403 — don't confirm existence
  }
  res.json(doc);
});
```

**Common mistakes:**

- **Returning 403 instead of 404 for unauthorized resources.** A 403 confirms the resource exists. Use 404 to prevent enumeration.
- **Checking ownership in some endpoints but not others.** Access control must be consistent. Check every endpoint that takes a resource ID.
- **Only checking in the GET handler.** If you check ownership on read but not on update or delete, an attacker can't see the resource but can still modify or destroy it.

**How to verify:** For every endpoint that takes a resource ID, test with a valid session for a different user. The response should be 404 (not 403, not the resource data).

### Authorization: middleware-based, deny by default

**The pattern:** Authorization should be enforced by middleware that runs before the route handler, not by checks scattered inside handlers. The default should be deny — a route without explicit authorization middleware should be inaccessible.

```javascript
// Middleware approach
function requireRole(...roles) {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({ error: "Forbidden" });
    }
    next();
  };
}

// Apply to routes
app.delete("/api/admin/users/:id", auth, requireRole("admin"), deleteUser);
app.get("/api/users/me", auth, requireRole("user", "admin"), getProfile);

// Deny by default — catch unprotected routes in tests
// In your test suite:
test("all routes have auth middleware", () => {
  const unprotectedRoutes = getRoutes().filter(r => !r.middleware.includes("auth"));
  expect(unprotectedRoutes).toEqual(publicRoutes); // only explicitly public routes
});
```

**Common mistakes:**

- **Checking roles in the handler instead of middleware.** Scattered checks get inconsistent. Some handlers will forget. Middleware is harder to skip.
- **Allow by default.** "Everything is accessible unless we add a check" guarantees that new endpoints are vulnerable until someone remembers to add authorization.
- **Client-side-only authorization.** Hiding the admin button doesn't protect the admin API. An attacker doesn't use your UI.

**How to verify:** List all routes. For each, confirm it has auth middleware or is explicitly public. Test admin endpoints with non-admin credentials.

### Mass assignment fix: explicit allowlists

**The pattern:** Never pass the raw request body to a database update. Explicitly destructure or pick the fields that are allowed for each endpoint.

```javascript
// Explicit allowlist
app.patch("/api/users/me", auth, async (req, res) => {
  const { name, email, timezone } = req.body; // only these fields
  await db.query(
    "UPDATE users SET name = $1, email = $2, timezone = $3 WHERE id = $4",
    [name, email, timezone, req.user.id]
  );
});
```

```python
# Django — explicit fields in the serializer
class UserUpdateSerializer(serializers.Serializer):
    name = serializers.CharField(max_length=100)
    email = serializers.EmailField()
    # role is NOT here — cannot be set via this endpoint
```

**How to verify:** Search for patterns where the full request body is passed to an update operation: `UPDATE ... SET ?` with `req.body`, `Model.update(req.body)`, `Object.assign(user, req.body)`. Each one should be replaced with an explicit field list.

---

## Secrets

### Remove secrets from code: environment variables and secret managers

**The pattern:** Secrets belong in environment variables or a secret manager. Never in source files, never committed to git.

**Environment variables:**
```javascript
// Read from environment, not from code
const apiKey = process.env.STRIPE_API_KEY;
if (!apiKey) throw new Error("STRIPE_API_KEY environment variable is required");
```

**Secret manager (for production):**
```javascript
// AWS Secrets Manager example
const { SecretsManagerClient, GetSecretValueCommand } = require("@aws-sdk/client-secrets-manager");
const client = new SecretsManagerClient();

async function getSecret(name) {
  const response = await client.send(new GetSecretValueCommand({ SecretId: name }));
  return JSON.parse(response.SecretString);
}
```

**.gitignore (prevent accidental commits):**
```
.env
.env.*
*.pem
*.key
credentials.json
service-account*.json
```

**How to clean git history after a secret is found:**

Removing a secret from the current code is not sufficient. The secret is still in git history and accessible to anyone who clones the repository.

1. **Rotate the secret immediately.** This is the priority — even before cleaning history. Generate a new key, update the production config, and revoke the old one.
2. **Clean git history** (if the repository is private and history cleaning is feasible):
   ```bash
   # Using git-filter-repo (preferred over filter-branch)
   git filter-repo --invert-paths --path path/to/file-with-secret
   # Or to replace a specific string across all history:
   git filter-repo --replace-text <(echo "OLD_SECRET==>REDACTED")
   ```
3. **Force push** the cleaned history (requires coordination if others have cloned).
4. **If the repository is public**, assume the secret is compromised permanently. Rotation is the only fix. Git history cleaning is cosmetic at that point.

**Common mistakes:**

- **Using `.env` files as the secrets management strategy.** `.env` is better than hardcoding, but it's a file on disk that can be committed accidentally, read by any process, and isn't auditable. Use a real secret manager for production.
- **Rotating the secret but not cleaning history.** The old secret is still in git. If it grants any access (even reduced), it's still a vulnerability.
- **Checking for secrets only in the current working tree.** Always search git history: `git log -p --all -S "pattern"`.

---

## Dependencies

### Audit and update: lockfiles, audit commands, automated scanning

**The pattern:** Use lockfiles, run audit commands regularly, and automate dependency scanning.

**Lockfiles:**
```bash
# Ensure lockfile exists and is committed
# Node.js: package-lock.json or yarn.lock or pnpm-lock.yaml
# Python: requirements.txt with pinned versions, or poetry.lock, or pip-compile
# Ruby: Gemfile.lock
# Rust: Cargo.lock
```

**Audit commands:**
```bash
# Node.js
npm audit
npm audit fix  # auto-fix patch/minor updates

# Python
pip-audit      # or safety check

# Ruby
bundle audit

# Go
govulncheck ./...
```

**Automated scanning:** Enable Dependabot, Renovate, or Snyk on the repository. These create pull requests automatically when vulnerabilities are discovered.

**Common mistakes:**

- **Running `npm audit fix --force` without reviewing.** Force-fixing can bump major versions, breaking your application. Review each fix.
- **Ignoring audit findings because "it's a dev dependency."** Dev dependencies can be exploited during CI/CD. A compromised build tool is a supply chain attack. Evaluate whether the CVE applies to your usage.
- **Not committing lockfiles.** Without a lockfile, `npm install` resolves to whatever the latest matching version is. A compromised package can be injected between installs.

**How to verify:** Run `npm audit` (or equivalent) and confirm zero critical/high findings. Verify the lockfile is committed and matches the installed packages.

---

## Defense in depth

The fixes above are each one layer of defense. A secure system doesn't rely on any single layer. Here's how the layers compose:

### Against injection

1. **Input validation** — reject obviously invalid input at the boundary (type checking, length limits, format validation)
2. **Parameterized queries** — ensure valid-looking input can't be interpreted as code
3. **Least privilege** — the database user has only the permissions the application needs (no DROP TABLE, no access to unrelated tables)
4. **Output encoding** — if injected data somehow gets stored, it's encoded before rendering
5. **Monitoring** — anomalous query patterns trigger alerts

One layer failing is survivable. Two layers failing is survivable. All five failing simultaneously is what's required for a successful attack.

### Against authentication bypass

1. **Strong password hashing** — stolen hashes are useless without months of cracking
2. **Session management** — proper cookie flags, server-side invalidation, rotation
3. **Rate limiting** — brute force is throttled
4. **MFA** — password alone isn't sufficient
5. **Monitoring** — anomalous login patterns trigger alerts

### Against data exposure

1. **Encryption in transit** — TLS on all connections
2. **Encryption at rest** — database and file storage encryption
3. **Access control** — only authorized users can request the data
4. **Logging redaction** — sensitive data never reaches logs
5. **Least privilege** — services and users have minimum necessary access

### Why multiple layers matter

Every security control has a failure mode. Input validation fails when a developer forgets to validate one field. Parameterized queries fail when a developer uses a raw query for a "quick fix." Rate limiting fails when the attacker uses distributed IPs. Each layer is imperfect. The compound probability of all layers failing simultaneously is what makes the system secure.

When implementing a fix, always ask: "If this fix fails — if someone removes it, bypasses it, or it has a bug — what's the next layer that stops the attack?" If the answer is "nothing," add another layer.
