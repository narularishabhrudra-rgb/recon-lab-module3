# Web Application Fundamentals

## 1. HTTP Request/Response Lifecycle
1. User enters a URL; browser performs a DNS lookup.
2. Browser opens a TCP (and TLS for HTTPS) connection.
3. Browser sends an **HTTP request**.
4. Server processes it (app logic, database) and returns an **HTTP response**.
5. Browser renders the page.

**Request example**
```
GET /profile?id=5 HTTP/1.1
Host: example.com
Cookie: session=abc123
User-Agent: Mozilla/5.0
```

**Response example**
```
HTTP/1.1 200 OK
Content-Type: text/html
Set-Cookie: session=abc123; HttpOnly; Secure; SameSite=Lax
```

### Common Methods
| Method | Use |
|--------|-----|
| GET | fetch data |
| POST | submit data |
| PUT/PATCH | update |
| DELETE | remove |

### Status Codes
200 OK, 301/302 redirect, 400 bad request, 401 unauthenticated, 403 forbidden, 404 not found, 500 server error.

## 2. Cookies
Small data stored by the browser and sent with each request to that site.

Security flags:
- `HttpOnly`: JavaScript cannot read the cookie (limits XSS theft)
- `Secure`: only sent over HTTPS
- `SameSite`: limits cross-site sending (helps against CSRF)

## 3. Sessions
HTTP is stateless, so the server tracks users with a **session ID** (stored in a cookie).
- Session ID must be random and unpredictable
- Regenerate it after login
- Expire it on logout and after inactivity

## 4. Authentication vs Authorization
| | Authentication | Authorization |
|---|----------------|---------------|
| Question | Who are you? | What are you allowed to do? |
| Examples | password, MFA, OAuth login | admin vs user roles, access to other users' data |
| Failure leads to | account takeover | broken access control |

## 5. Useful Browser Tools
- DevTools → **Network** tab: inspect requests/responses
- DevTools → **Application** tab: view cookies and storage
- Burp Suite / ZAP: intercept and modify requests (lab only)

## Key Takeaway
Never trust data coming from the client. Validate everything on the server.
