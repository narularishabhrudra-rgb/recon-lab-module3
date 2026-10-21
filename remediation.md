# Remediation Recommendations

## Priority Overview
| Priority | Finding | Fix | Status |
|----------|---------|-----|--------|
| High | SQL injection | Parameterized queries | [Fixed / Planned] |
| High | Broken access control (IDOR) | Server-side ownership checks | [ ] |
| Medium | XSS | Output encoding + CSP | [ ] |
| Medium | Weak session cookies | HttpOnly, Secure, SameSite | [ ] |
| Low | Missing security headers | Add headers | [ ] |

## Fix Details

### SQL Injection
```python
# Before
query = "SELECT * FROM users WHERE id = " + user_id
# After
cursor.execute("SELECT * FROM users WHERE id = %s", (user_id,))
```
Also: validate input type, use a least-privilege DB account.

### Broken Access Control (IDOR)
```python
# Before
def get_profile(request, id):
    return User.objects.get(id=id)
# After
def get_profile(request, id):
    if request.user.id != int(id) and not request.user.is_admin:
        raise PermissionDenied
    return User.objects.get(id=id)
```

### Cross-Site Scripting
- Encode output (e.g. template auto-escaping)
- Add header: `Content-Security-Policy: default-src 'self'`
- Mark cookies `HttpOnly`

### Session and Authentication
```
Set-Cookie: session=...; HttpOnly; Secure; SameSite=Strict
```
- Regenerate session ID on login; destroy on logout
- Add rate limiting and MFA

### Security Headers
```
Content-Security-Policy: default-src 'self'
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

## Verification (Re-test)
After each fix, repeat the original test and confirm the issue no longer works.

| Finding | Re-test result |
|---------|----------------|
| [V-01] | [Blocked / still vulnerable] |
