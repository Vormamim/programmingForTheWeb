---
title: Day 20 — Input Validation and Defensive Coding
parent: Week 4 — Back-End and Databases
nav_order: 2
---

# Day 20 — Input Validation and Defensive Coding

**Syllabus:** Design, develop and implement code using defensive data input handling practices — input validation, sanitisation and error handling
**Outcomes:** SE-12-02, SE-12-06
**Textbook:** Section 13.1 + Secure Software Architecture

---

## Key Terms

**Input validation** — Checking that user-supplied data meets expected rules (type, length, format, range) before processing it.

**Sanitisation** — Cleaning input data by removing or encoding characters that could cause harm. Example: converting `<script>` to `&lt;script&gt;`.

**Whitelist validation** — Accepting only known-good input. Example: only allow alphanumeric characters in a username.

**Blacklist validation** — Rejecting known-bad input. Less secure than whitelisting — attackers find ways around blacklists.

**Error handling** — Anticipating and gracefully managing runtime errors so the application does not crash or expose sensitive information.

**Exception** — A runtime error in Python. Handled with `try/except`.

**Escape** — Encoding special characters so they are treated as data, not code. Example: `<` becomes `&lt;` in HTML output.

---

## Theory

### Why Validation and Sanitisation Matter

User input is untrusted. Users may — accidentally or deliberately — submit:
- Empty fields where data is required
- Data of the wrong type (text where a number is expected)
- Data that is too long (buffer overflow attacks)
- Malicious code embedded in input (XSS, SQL injection — covered in Day 27)

**Defensive programming** assumes all input is potentially malicious until proven otherwise.

### Validation in Flask

```python
def validate_registration(username, email, password, confirm):
    errors = []

    # Required fields
    if not username or not email or not password:
        errors.append("All fields are required.")

    # Length
    if len(username) < 3:
        errors.append("Username must be at least 3 characters.")
    if len(username) > 30:
        errors.append("Username must be 30 characters or fewer.")
    if len(password) < 8:
        errors.append("Password must be at least 8 characters.")

    # Format — email
    import re
    email_pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    if not re.match(email_pattern, email):
        errors.append("Invalid email address.")

    # Whitelist — username (letters, numbers, underscore only)
    if not re.match(r'^[a-zA-Z0-9_]+$', username):
        errors.append("Username may only contain letters, numbers, and underscores.")

    # Match
    if password != confirm:
        errors.append("Passwords do not match.")

    return errors
```

```python
@app.route('/register', methods=['POST'])
def register():
    username = request.form.get('username', '').strip()
    email    = request.form.get('email', '').strip().lower()
    password = request.form.get('password', '')
    confirm  = request.form.get('confirm_password', '')

    errors = validate_registration(username, email, password, confirm)

    if errors:
        for error in errors:
            flash(error, 'error')
        return redirect(url_for('register'))

    # Safe to process
    flash('Registration successful!', 'success')
    return redirect(url_for('home'))
```

### Sanitisation

**Jinja2 auto-escapes HTML by default** — any variable output through `{{ }}` has HTML special characters converted:

```python
user_input = "<script>alert('XSS')</script>"
# In template: {{ user_input }}
# Rendered as: &lt;script&gt;alert('XSS')&lt;/script&gt;
# Displayed as literal text — not executed as JavaScript
```

Only use `{{ content | safe }}` when you are absolutely certain the content is trusted HTML — never for user input.

**Stripping dangerous content manually:**

```python
import re

def sanitise_input(text):
    # Remove HTML tags
    text = re.sub(r'<[^>]+>', '', text)
    # Strip leading/trailing whitespace
    text = text.strip()
    # Limit length
    text = text[:500]
    return text
```

### Error Handling

Never let unhandled errors expose stack traces to users — they reveal server structure and code.

```python
# Basic try/except
@app.route('/calculate')
def calculate():
    try:
        value = int(request.args.get('n', ''))
        result = 100 / value
        return f'Result: {result}'
    except ValueError:
        flash('Please enter a valid number.', 'error')
        return redirect(url_for('home'))
    except ZeroDivisionError:
        flash('Cannot divide by zero.', 'error')
        return redirect(url_for('home'))
    except Exception as e:
        # Log the error (don't show it to the user)
        app.logger.error(f'Unexpected error: {e}')
        return render_template('500.html'), 500
```

**Custom error pages:**

```python
@app.errorhandler(404)
def not_found(e):
    return render_template('404.html'), 404

@app.errorhandler(500)
def server_error(e):
    return render_template('500.html'), 500
```

### Validation Checklist

For every form field, ask:

| Check | Example |
|---|---|
| Is it required? | Username must not be empty |
| Correct type? | Age must be an integer |
| Correct length? | Password 8–128 characters |
| Correct format? | Email matches pattern |
| Within allowed range? | Age between 0 and 120 |
| Whitelisted characters? | Username: letters, numbers, underscore only |
| Does it match another field? | Password == confirm password |

### Defence in Depth

Validation should happen at multiple layers:

1. **HTML** — `required`, `type="email"`, `maxlength` attributes (easily bypassed — not sufficient alone)
2. **JavaScript** — client-side validation for fast feedback (also easily bypassed)
3. **Python/Flask** — server-side validation (mandatory — cannot be bypassed)
4. **Database** — constraints (NOT NULL, UNIQUE, CHECK) as a final safety net

Never rely solely on client-side validation. Any user can bypass it by sending a raw HTTP request.

---

## Objective Response Questions

**1.** A developer validates a username using a blacklist that rejects `<`, `>`, and `"`. An attacker bypasses this by using `&lt;` instead. What is the more secure approach?

- A) Add `&lt;` to the blacklist
- B) Use a whitelist that only permits alphanumeric characters and underscores
- C) Validate on the client side using JavaScript
- D) Increase the maximum username length

**Answer: B** — Whitelisting accepts only known-good characters. Blacklisting always risks missing attack variants.

---

**2.** A Flask application uses `{{ user_comment | safe }}` to display user-submitted comments. What security risk does this introduce?

- A) Comments will be truncated at 500 characters
- B) The `safe` filter disables Jinja2's automatic HTML escaping, allowing XSS attacks if comments contain malicious scripts
- C) The application will crash if comments contain special characters
- D) Comments will be stored in plaintext in the database

**Answer: B** — `| safe` tells Jinja2 to render the content as raw HTML. If a user submits `<script>alert('XSS')</script>`, it will execute in other users' browsers.

---

**3.** Why is server-side validation mandatory even when client-side JavaScript validation is already implemented?

- A) JavaScript validation is too slow for production use
- B) Server-side validation checks the database; client-side cannot
- C) A user can bypass client-side validation by sending a direct HTTP request to the server
- D) Flask does not support client-side JavaScript

**Answer: C**

---

## Try It

Improve the registration form from Day 19:

1. Add server-side validation for all fields using a `validate_registration()` function
2. Use a regex whitelist for the username field
3. Add custom error pages: create `templates/404.html` and `templates/500.html`
4. Register the error handlers in `app.py`
5. Add a route that intentionally raises an exception and verify your 500 page appears (with `debug=False`)
6. Test your validation by submitting:
   - An empty form
   - A username with special characters
   - Mismatched passwords
   - An invalid email address
