# Secure Coding Practices Checklist

## Input and Output
- [ ] Validate all input on the server (type, length, format, allow-lists)
- [ ] Use parameterized queries / prepared statements
- [ ] Encode output for the right context (HTML, JS, URL)
- [ ] Never build commands or queries from raw user input

## Authentication and Sessions
- [ ] Hash passwords with bcrypt or Argon2 (with salt)
- [ ] Enforce strong passwords and offer MFA
- [ ] Rate limit and lock out after repeated failures
- [ ] Regenerate session ID after login; invalidate on logout
- [ ] Cookies: `HttpOnly`, `Secure`, `SameSite`

## Access Control
- [ ] Deny by default
- [ ] Enforce authorization server-side on every request
- [ ] Apply least privilege to users, services, and database accounts
- [ ] Avoid exposing predictable IDs without ownership checks

## Data Protection
- [ ] HTTPS/TLS everywhere
- [ ] Do not store secrets in source code; use environment variables or a secrets manager
- [ ] Encrypt sensitive data at rest
- [ ] Do not log passwords or tokens

## Configuration and Dependencies
- [ ] Remove default accounts and debug pages
- [ ] Show generic error messages to users
- [ ] Add security headers: `Content-Security-Policy`, `X-Content-Type-Options`, `Strict-Transport-Security`, `X-Frame-Options`
- [ ] Keep dependencies updated; scan with `npm audit`, `pip-audit`, or Dependabot

## Logging and Monitoring
- [ ] Log logins, failures, access denials, and input validation failures
- [ ] Alert on suspicious patterns
- [ ] Protect log integrity

## Process
- [ ] Threat model new features
- [ ] Code review with a security focus
- [ ] Run SAST/DAST scans (e.g. ZAP) in the pipeline
