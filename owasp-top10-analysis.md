# OWASP Top 10 Analysis

Based on the OWASP Top 10 (2021 list). Check owasp.org for the latest edition, as the list is updated periodically.

| # | Category | What it is | Example | Prevention |
|---|----------|------------|---------|------------|
| A01 | **Broken Access Control** | Users act outside their permissions | Changing `?id=5` to `?id=6` shows another user's data (IDOR) | Deny by default, enforce checks server-side, use indirect references |
| A02 | **Cryptographic Failures** | Sensitive data not properly protected | Passwords stored in plain text, HTTP instead of HTTPS | TLS everywhere, strong hashing (bcrypt/Argon2), no weak algorithms |
| A03 | **Injection** | Untrusted input interpreted as commands | SQL injection via a login form | Parameterized queries, input validation, ORM, least-privilege DB user |
| A04 | **Insecure Design** | Missing security in the design itself | No rate limit on password reset | Threat modeling, secure design patterns |
| A05 | **Security Misconfiguration** | Insecure defaults or settings | Default admin password, verbose error pages, directory listing | Hardening, remove defaults, minimal features, config reviews |
| A06 | **Vulnerable and Outdated Components** | Using libraries with known flaws | Old framework with a public CVE | Dependency scanning, patching, remove unused packages |
| A07 | **Identification and Authentication Failures** | Weak login/session handling | Weak passwords allowed, no MFA, session ID in URL | MFA, strong password policy, lockout/rate limiting, secure sessions |
| A08 | **Software and Data Integrity Failures** | Trusting unverified code or data | Unsigned updates, insecure deserialization | Signed packages, integrity checks, secure CI/CD |
| A09 | **Security Logging and Monitoring Failures** | Attacks go unnoticed | No logs for failed logins | Log security events, alerting, protect logs |
| A10 | **Server-Side Request Forgery (SSRF)** | Server fetches attacker-chosen URLs | App fetches an internal URL supplied by the user | Allow-list destinations, block internal ranges, validate URLs |

## Deep Dive: Top Issues Tested in the Lab

### Injection (SQLi)
- **Cause:** user input concatenated into a query.
- **Lab test (DVWA, own sandbox):** enter a single quote `'` in an input and observe a database error, which indicates unsanitized input.
- **Fix:** use parameterized queries.
```python
# Vulnerable
query = "SELECT * FROM users WHERE name = '" + name + "'"
# Secure
cursor.execute("SELECT * FROM users WHERE name = %s", (name,))
```

### Cross-Site Scripting (XSS)
- **Cause:** user input shown in a page without encoding.
- **Lab test:** submit harmless text like `<b>test</b>` and see if it renders as HTML.
- **Fix:** output encoding, Content-Security-Policy, HttpOnly cookies, sanitize input.

### Broken Access Control
- **Lab test:** log in as a normal user and try accessing another user's record or an admin URL.
- **Fix:** check authorization on every request on the server.

### Authentication Failures
- **Lab test:** check password rules, lockout after failed attempts, session handling after logout.
- **Fix:** MFA, rate limiting, secure session cookies, session invalidation on logout.

## My Analysis
[Write which categories you tested, what you found, and what surprised you.]
