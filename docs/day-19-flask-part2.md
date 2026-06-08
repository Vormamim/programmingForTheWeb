---
title: Day 19 — Flask Part 2 — Jinja2 and Forms
parent: Week 4 — Back-End and Databases
nav_order: 1
---

# Day 19 — Flask Part 2: Jinja2 and Forms

**Syllabus:** Template engines; safe input handling; observe back-end web request process
**Outcomes:** SE-12-02
**Textbook:** Section 13.1

---

## Key Terms

**Form** — An HTML element that collects user input and sends it to a server via HTTP POST or GET.

**POST** — HTTP method for sending data to the server (login, signup, form submission). Data is in the request body — not visible in the URL.

**GET** — HTTP method for retrieving data. Parameters appear in the URL. Used for search, filters.

**Jinja2 filter** — A modifier applied to a variable in a template. Example: `{{ name | upper }}` outputs the name in uppercase.

**Flash message** — A one-time message stored in the session and displayed to the user on the next page load. Used for success/error feedback after form submission.

**Session** — Server-side storage that persists data across requests for a single user. Requires a secret key.

---

## Theory

### Handling HTML Forms in Flask

A form with POST method:

```html
<!-- templates/contact.html -->
{% extends "base.html" %}
{% block content %}

<h1>Contact Us</h1>

{% with messages = get_flashed_messages(with_categories=true) %}
  {% for category, message in messages %}
    <div class="alert alert-{{ category }}">{{ message }}</div>
  {% endfor %}
{% endwith %}

<form method="POST" action="/contact">
    <label for="name">Name</label>
    <input type="text" id="name" name="name" required>

    <label for="email">Email</label>
    <input type="email" id="email" name="email" required>

    <label for="message">Message</label>
    <textarea id="message" name="message" rows="5" required></textarea>

    <button type="submit">Send</button>
</form>

{% endblock %}
```

```python
from flask import Flask, render_template, request, redirect, url_for, flash

app = Flask(__name__)
app.secret_key = 'dev-secret-change-in-production'

@app.route('/contact', methods=['GET', 'POST'])
def contact():
    if request.method == 'POST':
        name    = request.form.get('name', '').strip()
        email   = request.form.get('email', '').strip()
        message = request.form.get('message', '').strip()

        # Basic validation
        if not name or not email or not message:
            flash('All fields are required.', 'error')
            return redirect(url_for('contact'))

        # Process (save to DB, send email, etc.)
        print(f"Message from {name} ({email}): {message}")

        flash('Message sent successfully!', 'success')
        return redirect(url_for('contact'))

    return render_template('contact.html')
```

**Post-Redirect-Get pattern:** Always redirect after a successful POST. This prevents the browser from resubmitting the form if the user refreshes the page.

### Jinja2 Filters

Filters transform variables in templates:

```html
{{ name | upper }}              <!-- ALEX CHEN -->
{{ name | lower }}              <!-- alex chen -->
{{ name | title }}              <!-- Alex Chen -->
{{ description | truncate(50) }}<!-- First 50 characters... -->
{{ price | round(2) }}          <!-- 19.99 -->
{{ items | length }}            <!-- 5 -->
{{ html_content | safe }}       <!-- render HTML (use carefully) -->
{{ date | default('Unknown') }} <!-- fallback if date is None -->
```

### Jinja2 Macros

Reusable template fragments — like functions in Python:

```html
<!-- templates/macros.html -->
{% macro form_field(label, name, type='text', required=true) %}
<div class="field">
    <label for="{{ name }}">{{ label }}</label>
    <input type="{{ type }}"
           id="{{ name }}"
           name="{{ name }}"
           {% if required %}required{% endif %}>
</div>
{% endmacro %}
```

```html
<!-- Use in another template -->
{% from 'macros.html' import form_field %}

<form method="POST">
    {{ form_field('Username', 'username') }}
    {{ form_field('Password', 'password', type='password') }}
    {{ form_field('Email', 'email', type='email') }}
    <button type="submit">Register</button>
</form>
```

### Sessions

```python
from flask import session

app.secret_key = 'change-this-in-production'

@app.route('/login', methods=['POST'])
def login():
    username = request.form.get('username')
    # (validate credentials against database here)
    session['username'] = username
    session['logged_in'] = True
    return redirect(url_for('dashboard'))

@app.route('/dashboard')
def dashboard():
    if not session.get('logged_in'):
        return redirect(url_for('login'))
    return render_template('dashboard.html',
                           username=session['username'])

@app.route('/logout')
def logout():
    session.clear()
    return redirect(url_for('home'))
```

In Jinja2 templates, session data is accessible directly:

```html
{% if session.logged_in %}
    <p>Welcome, {{ session.username }}</p>
    <a href="/logout">Logout</a>
{% else %}
    <a href="/login">Login</a>
{% endif %}
```

### Complete Login Example

```python
# Simple in-memory user store (Day 23 replaces this with a database)
USERS = {
    'alex': 'password123',
    'sam': 'letmein'
}

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form.get('username', '').strip().lower()
        password = request.form.get('password', '')

        if username in USERS and USERS[username] == password:
            session['username'] = username
            session['logged_in'] = True
            flash('Logged in successfully.', 'success')
            return redirect(url_for('dashboard'))
        else:
            flash('Invalid username or password.', 'error')
            return redirect(url_for('login'))

    return render_template('login.html')
```

---

## Objective Response Questions

**1.** After a successful form POST in Flask, a developer calls `return redirect(url_for('home'))` instead of `return render_template('home.html')`. Why is the redirect approach better?

- A) Templates cannot be rendered after a POST request
- B) The redirect prevents the browser from resubmitting the form if the page is refreshed
- C) Redirects are faster than rendering templates
- D) Flask does not allow `render_template` after POST

**Answer: B** — This is the Post-Redirect-Get pattern. Without it, refreshing the page after a POST resubmits the form, potentially duplicating data.

---

**2.** A developer stores a user's login status in a Flask session. What is required for sessions to work in Flask?

- A) A database connection
- B) HTTPS must be enabled
- C) A secret key must be set on the Flask app
- D) The user must accept cookies explicitly

**Answer: C** — Flask sessions are signed using the `secret_key`. Without it, sessions cannot be created.

---

**3.** In a Jinja2 template, what does `{{ user.name | title }}` output if `user.name` is `"alex chen"`?

- A) `alex chen`
- B) `ALEX CHEN`
- C) `Alex Chen`
- D) `Alex chen`

**Answer: C** — The `title` filter capitalises the first letter of each word.

---

## Try It

Add to your Flask project:

1. A `/register` route with GET (show form) and POST (process form) handlers
2. The registration form collects: username, email, password, confirm password
3. Validate that:
   - All fields are filled
   - Password and confirm password match
   - Username is at least 3 characters
4. On success, flash a success message and redirect
5. On failure, flash an error message and redirect back to the form
6. Store the registered user in a Python dictionary (database comes in Day 23)
