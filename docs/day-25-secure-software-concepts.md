---
title: Day 25 — Secure Software Concepts
parent: Week 5 — Security, PWA and Assessment
nav_order: 1
---

# Day 25 — Secure Software Concepts

**Syllabus:** Explore fundamental software design security concepts — confidentiality, integrity, availability, authentication, authorisation, accountability
**Outcomes:** SE-12-06
**Textbook:** Secure Software Architecture

---

## Key Terms

**CIA Triad** — The three core principles of information security: Confidentiality, Integrity, Availability.

**Confidentiality** — Ensuring information is accessible only to those authorised to access it.

**Integrity** — Ensuring data is accurate, complete, and has not been altered unauthorised.

**Availability** — Ensuring systems and data are accessible to authorised users when needed.

**Authentication** — Verifying the identity of a user or system.

**Authorisation** — Determining what an authenticated user is permitted to do.

**Accountability** — The ability to trace actions back to a specific user or system. Achieved through logging and audit trails.

**Principle of least privilege** — Users and processes should have only the minimum access required to perform their function.

**Defence in depth** — Layering multiple security controls so that if one fails, others remain.

**Threat** — A potential cause of an unwanted security incident.

**Vulnerability** — A weakness in a system that could be exploited by a threat.

**Risk** — The likelihood and impact of a threat exploiting a vulnerability.

---

## Theory

### The CIA Triad

Every security decision in software development can be evaluated against these three principles:

**Confidentiality** — Who can see the data?
- Encryption of data at rest and in transit
- Access controls (only authorised users access sensitive data)
- Password hashing (passwords never stored in plaintext)

*Violated by:* data breaches, unauthorised access, unencrypted transmission

**Integrity** — Is the data trustworthy?
- Hash values to verify files have not been tampered with
- Database constraints preventing invalid data
- Input validation rejecting malformed data
- Digital signatures verifying message authenticity

*Violated by:* SQL injection altering records, man-in-the-middle attacks modifying data in transit

**Availability** — Can authorised users access the system?
- Redundant servers and failover systems
- DDoS protection
- Regular backups
- Rate limiting to prevent abuse

*Violated by:* DDoS attacks, server crashes, ransomware

### Authentication in Web Applications

**Levels of authentication:**

| Method | Security | Usability |
|---|---|---|
| Password only | Low | High |
| Password + MFA | High | Medium |
| Passkey / biometric | Very high | High |
| SSO (Google, GitHub) | Depends on provider | Very high |

**Secure password handling in Flask:**

```python
from werkzeug.security import generate_password_hash, check_password_hash

# When registering — never store plaintext
password_hash = generate_password_hash(password)
db.execute('INSERT INTO users (username, password_hash) VALUES (?, ?)',
           (username, password_hash))

# When logging in
user = db.execute('SELECT * FROM users WHERE username = ?',
                  (username,)).fetchone()

if user and check_password_hash(user['password_hash'], password):
    session['user_id'] = user['id']
    session['logged_in'] = True
```

### Authorisation Patterns

**Route protection — decorator pattern:**

```python
from functools import wraps
from flask import session, redirect, url_for, flash

def login_required(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        if not session.get('logged_in'):
            flash('Please log in to access this page.', 'error')
            return redirect(url_for('login'))
        return f(*args, **kwargs)
    return decorated

def admin_required(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        if not session.get('is_admin'):
            flash('Admin access required.', 'error')
            return redirect(url_for('home'))
        return f(*args, **kwargs)
    return decorated

# Apply to routes
@app.route('/dashboard')
@login_required
def dashboard():
    return render_template('dashboard.html')

@app.route('/admin')
@login_required
@admin_required
def admin_panel():
    return render_template('admin.html')
```

### Accountability — Logging

Logs create an audit trail — who did what, and when:

```python
import logging
from datetime import datetime

# Configure logging
logging.basicConfig(
    filename='app.log',
    level=logging.INFO,
    format='%(asctime)s %(levelname)s: %(message)s'
)

@app.route('/login', methods=['POST'])
def login():
    username = request.form.get('username')
    # ... validate ...

    if authenticated:
        logging.info(f'LOGIN SUCCESS: {username} from {request.remote_addr}')
        session['logged_in'] = True
    else:
        logging.warning(f'LOGIN FAILED: {username} from {request.remote_addr}')
        flash('Invalid credentials.', 'error')
```

Logs should record:
- Successful and failed authentication attempts
- Authorisation failures (access denied)
- Data modifications (who changed what, when)
- Errors and exceptions

**Important:** logs must not contain plaintext passwords or sensitive data.

### Principle of Least Privilege

Apply to both users and code:

- A student account cannot modify other students' records
- A database user for the web app has SELECT/INSERT/UPDATE only — not DROP TABLE
- A service worker only has access to the files it needs to cache

```python
# Wrong: checking admin in templates (can be bypassed)
# Right: check in the route and return 403 if unauthorised

@app.route('/admin/delete-user/<int:user_id>', methods=['POST'])
@login_required
def delete_user(user_id):
    if not session.get('is_admin'):
        return 'Forbidden', 403    # hard stop — no redirect, no flash
    # proceed with deletion
```

### Secure Development Checklist

| Principle | Implementation |
|---|---|
| Confidentiality | HTTPS, password hashing, encrypted storage |
| Integrity | Input validation, parameterised queries, hashing |
| Availability | Error handling, rate limiting, backups |
| Authentication | Secure login, session management, MFA option |
| Authorisation | Route protection, least privilege, role checks |
| Accountability | Logging, audit trails, no sensitive data in logs |

---

## Objective Response Questions

**1.** A web application stores user passwords as SHA-256 hashes. An attacker obtains the database. Which security principle was correctly applied, and which was not?

- A) Confidentiality applied (hashed); integrity not applied (no salt)
- B) Availability applied; confidentiality not applied
- C) Integrity applied; availability not applied
- D) None — SHA-256 passwords provide no security benefit

**Answer: A** — Hashing protects confidentiality (passwords not readable). However, unsalted SHA-256 hashes are vulnerable to rainbow table attacks. `werkzeug.security.generate_password_hash` adds a salt automatically.

---

**2.** A server crash takes a web application offline for 6 hours. Which component of the CIA triad has been violated?

- A) Confidentiality
- B) Integrity
- C) Availability
- D) Accountability

**Answer: C** — Availability means authorised users can access the system when needed.

---

**3.** A Flask route checks `if session.get('is_admin')` and redirects non-admins to the home page. An attacker manually edits their session cookie. Why does this not work?

- A) Flask session cookies are stored server-side and cannot be read by the client
- B) Flask session cookies are cryptographically signed — tampering invalidates the signature
- C) Redirects are processed before session data is checked
- D) Flask does not use cookies for session management

**Answer: B** — Flask signs session cookies using the `secret_key`. Any modification to the cookie contents causes the signature verification to fail and the session is rejected.

---

## Try It

Add security features to your Flask CRUD application:

1. Implement `generate_password_hash` and `check_password_hash` for the registration and login routes
2. Create a `login_required` decorator and apply it to all routes that require authentication
3. Add logging to record successful logins, failed login attempts (with IP address), and all data modifications
4. Create a simple admin role — add an `is_admin` column to the users table, protect an `/admin` route with both `@login_required` and `@admin_required`
5. Test that a non-admin user receives a 403 response when attempting to access `/admin`
