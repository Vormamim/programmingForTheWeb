---
title: Day 30 — Assessment Review
parent: Week 5 — Security, PWA and Assessment
nav_order: 6
---

# Day 30 — Assessment Review: Full Stack Walkthrough

**Syllabus:** Design, develop and implement a PWA; observe and describe the back-end process used to manage a web request; assess the contribution of back-end web development
**Outcomes:** SE-12-02, SE-12-03, SE-12-04, SE-12-06

---

## Purpose

This lesson consolidates the full 30-day topic into a coherent picture. It maps every major concept to how it appears in the assessment and provides a revision framework for the HSC exam.

---

## The Complete Web Request Flow

When a user interacts with your PWA, here is what happens at every layer:

```
User taps "Submit" on a form
        │
        ▼
[1. BROWSER / CLIENT]
  Service worker intercepts fetch request
  If offline → returns cached response
  If online → passes to network

        │ HTTPS (TLS encrypted)
        ▼

[2. NETWORK]
  DNS resolves domain to IP address
  TCP connection established
  TLS handshake — certificates verified, session key agreed
  HTTP POST request sent with encrypted payload

        │
        ▼

[3. WEB SERVER (Gunicorn / Flask dev server)]
  Receives HTTP request
  Passes to Flask application

        │
        ▼

[4. FLASK APPLICATION]
  Router matches URL to view function
  Middleware runs (session check, CSRF validation)
  View function executes:
    - Input validated and sanitised
    - Parameterised SQL query sent to database
    - Business logic applied
    - Response built (render_template or redirect)

        │
        ▼

[5. DATABASE (SQLite)]
  SQL query executes
  Returns rows as result set
  Connection returned to pool

        │
        ▼

[6. FLASK — RESPONSE]
  Jinja2 renders template with data
  Security headers added
  HTTP response sent

        │ HTTPS
        ▼

[7. BROWSER]
  Response received by service worker
  Cached if appropriate
  Page updated — DOM manipulated by JavaScript
  User sees result
```

### Mapping to Syllabus Dot Points

| Layer | Concept | Day |
|---|---|---|
| Browser | PWA, service workers, manifest | 28 |
| Browser | DOM manipulation, events | 13 |
| Network | DNS, IP, data packets | 2 |
| Network | HTTP/HTTPS, TLS, protocols | 3, 4 |
| Web server | Back-end request flow, WSGI | 18 |
| Flask | Routes, templates, Jinja2 | 18, 19 |
| Flask | Input validation, sanitisation | 20 |
| Flask | Authentication, sessions | 19, 25 |
| Flask | CSRF, XSS protection | 27 |
| Database | SQL queries, parameterisation | 21, 22 |
| Database | CRUD operations | 23 |
| All | Security — CIA triad | 25 |
| All | Version control | 29 |

---

## Assessment Checklist

Your PWA assessment should demonstrate:

### Functionality
- [ ] Multiple pages/routes with navigation
- [ ] At least one form with server-side validation
- [ ] Database — CREATE, READ, UPDATE, DELETE operations
- [ ] User authentication (register, login, logout)
- [ ] PWA manifest and service worker (installable, offline page)

### Security
- [ ] Passwords hashed with `werkzeug.security`
- [ ] All SQL queries parameterised (no f-strings in SQL)
- [ ] CSRF tokens on all forms
- [ ] Input validation on all form fields
- [ ] `@login_required` on protected routes
- [ ] Secure session configuration
- [ ] No hardcoded secrets (use environment variables)
- [ ] Custom 404 and 500 error pages
- [ ] Debug mode disabled in production

### Design and Accessibility
- [ ] Responsive layout (works on mobile and desktop)
- [ ] WCAG AA colour contrast on all text
- [ ] All images have `alt` attributes
- [ ] Keyboard navigable (focus indicators visible)
- [ ] Skip navigation link present
- [ ] Consistent navigation across all pages
- [ ] Flash messages for all user feedback

### Code Quality
- [ ] Meaningful Git commit history
- [ ] Jinja2 template inheritance (base.html)
- [ ] CSS variables for colours and fonts
- [ ] No inline JavaScript event handlers (`onclick=` in HTML)
- [ ] Functions have a single responsibility
- [ ] No commented-out code left in final submission

---

## Exam Revision: Key Questions and Answers

**Q: What is the difference between authentication and authorisation?**
Authentication verifies identity (who are you?). Authorisation determines permissions (what can you do?). Authentication always occurs first.

**Q: Why are parameterised queries required for all SQL that uses user input?**
String-formatted SQL allows SQL injection — attackers can manipulate the query structure. Parameterised queries treat user input as data, not executable SQL.

**Q: What does a service worker do?**
A service worker is a JavaScript file running in the background that intercepts network requests. It enables offline functionality by serving cached responses when the network is unavailable.

**Q: Explain the TLS handshake.**
The client and server exchange certificates and negotiate a shared symmetric encryption key using asymmetric encryption. Subsequent communication is encrypted with the symmetric key for efficiency.

**Q: What is the CIA triad?**
Confidentiality (data accessible only to authorised parties), Integrity (data is accurate and unaltered), Availability (systems accessible when needed). All security decisions can be evaluated against these three principles.

**Q: How does CSRF work and how is it prevented?**
CSRF exploits the browser's automatic inclusion of cookies with requests. An attacker tricks an authenticated user into submitting a forged request. Prevention: include a unique CSRF token in every form. The server validates the token — forged requests from other origins cannot include it (same-origin policy).

**Q: What is the difference between SAST and DAST?**
SAST analyses source code without running it (white-box). DAST tests a running application by simulating attacks (black-box). SAST finds code-level issues early; DAST finds runtime and configuration issues.

**Q: What makes a web app a PWA?**
HTTPS, a web app manifest (name, icons, display mode), and a registered service worker. These enable installation and offline functionality.

**Q: What is big data and how does it affect web architecture?**
Big data refers to datasets characterised by Volume, Velocity, and Variety that exceed traditional database capabilities. Web architectures respond with distributed systems, CDNs, NoSQL databases, and stream processing pipelines.

---

## Final Project Push

Before submitting ensure:

```bash
# Remove debug mode
# In app.py: app.run(debug=False) or use environment variable

# Move secret key to environment variable
import os
app.secret_key = os.environ.get('SECRET_KEY', 'dev-only-change-this')

# Final commit
git add .
git commit -m "Final submission — PWA complete"
git push origin main

# Tag the release
git tag v1.0
git push origin v1.0
```

---

## Objective Response Questions

**1.** A student's PWA works when online but shows a blank screen when offline. Which component is most likely missing or incorrectly configured?

- A) The Web App Manifest
- B) The service worker fetch event handler
- C) CSRF protection
- D) The database connection

**Answer: B** — The fetch event handler in the service worker is responsible for serving cached content when offline. Without it, requests fail when the network is unavailable.

---

**2.** A developer stores `SECRET_KEY = "abc123"` directly in `app.py` and pushes it to a public GitHub repository. Which security principle is violated, and what is the consequence?

- A) Availability — the app will stop working
- B) Confidentiality — anyone can read the key, forge session cookies, and impersonate any user
- C) Integrity — the database becomes corrupted
- D) Accountability — logs are disabled

**Answer: B** — Flask's session cookies are signed with the secret key. Anyone with the key can forge valid session cookies, bypassing authentication entirely.

---

**3.** Which combination of tools provides the most comprehensive security testing coverage for a Flask web application?

- A) Bandit only
- B) OWASP ZAP only
- C) Code review + Bandit (SAST) + OWASP ZAP (DAST)
- D) Lighthouse only

**Answer: C** — No single tool covers everything. Manual code review catches logic issues, SAST catches code patterns, DAST catches runtime vulnerabilities. Combined coverage is most effective.

---

## Try It — Final Review

Complete these tasks as a final check before assessment submission:

1. Walk through the complete request flow diagram at the top of this lesson — for each layer, identify the specific code in your project that handles it
2. Run the assessment checklist — tick off completed items, action any gaps
3. Run Bandit: `bandit -r . -ll` — fix any medium or high severity findings
4. Run Lighthouse — target 80+ for Performance and Accessibility
5. Test offline mode in DevTools — verify the offline page appears
6. Test keyboard-only navigation through your entire app
7. Make your final commit and push
8. Enable GitHub Pages and verify the lesson notes site is live
