---
title: Day 18 — Flask Part 1 — Routes and Templates
parent: Week 3 — Scripting and Frameworks
nav_order: 6
---

# Day 18 — Flask Part 1: Routes and Templates

**Syllabus:** Observe and describe the back-end process used to manage a web request — role of web server software, web framework, objects, libraries, databases
**Outcomes:** SE-12-02
**Textbook:** Section 13.1

---

## Key Terms

**Route** — A URL pattern mapped to a Python function in Flask. When a request matches the pattern, Flask calls the function.

**View function** — The Python function associated with a route. Returns a response — HTML, JSON, or a redirect.

**Request object** — A Flask object (`request`) containing data from the incoming HTTP request — form data, query parameters, headers, method.

**Response** — What Flask sends back to the client. Can be HTML, JSON, a file, or a redirect.

**render_template()** — A Flask function that loads a Jinja2 template, inserts data, and returns the resulting HTML.

**Debug mode** — Running Flask with `debug=True` enables automatic reloading on code changes and detailed error pages. Never use in production.

**URL parameter** — A variable embedded in a URL pattern. Example: `/user/<username>` captures the username from the URL.

---

## Theory

### The Back-End Request Flow

When a browser requests a Flask page:

```
Browser                    Flask Application
  |                              |
  |-- GET /products/5 ---------->|
  |                         Router matches /products/<id>
  |                         View function called with id=5
  |                         Queries database
  |                         render_template('product.html', product=...)
  |<-- 200 OK + HTML -----------|
  |                              |
Renders HTML
```

### Minimal Flask Application

```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/about')
def about():
    return '<h1>About Page</h1>'

if __name__ == '__main__':
    app.run(debug=True)
```

### Routes

```python
# Basic route
@app.route('/contact')
def contact():
    return render_template('contact.html')

# URL parameter — captures part of the URL
@app.route('/user/<username>')
def profile(username):
    return f'<h1>Profile: {username}</h1>'

# Typed URL parameter
@app.route('/product/<int:product_id>')
def product(product_id):
    return f'<h1>Product #{product_id}</h1>'

# Multiple methods
@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        return 'Processing login...'
    return render_template('login.html')
```

### The Request Object

```python
from flask import request

@app.route('/search')
def search():
    # Query string: /search?q=flask&page=2
    query = request.args.get('q', '')       # GET parameter
    page = request.args.get('page', 1)
    return f'Searching for: {query} (page {page})'

@app.route('/login', methods=['POST'])
def login():
    username = request.form.get('username')  # POST form data
    password = request.form.get('password')
    return f'Logging in: {username}'
```

### Passing Data to Templates

```python
@app.route('/dashboard')
def dashboard():
    user = {
        'name': 'Alex Chen',
        'year': 12,
        'subjects': ['Software Engineering', 'Maths', 'Physics']
    }
    recent_scores = [85, 92, 78, 90]

    return render_template('dashboard.html',
        user=user,
        scores=recent_scores,
        average=sum(recent_scores) / len(recent_scores)
    )
```

```html
<!-- templates/dashboard.html -->
{% extends "base.html" %}
{% block content %}

<h1>Welcome, {{ user.name }}</h1>
<p>Year {{ user.year }}</p>

<h2>Subjects</h2>
<ul>
{% for subject in user.subjects %}
    <li>{{ subject }}</li>
{% endfor %}
</ul>

<h2>Recent Scores</h2>
<ul>
{% for score in scores %}
    <li class="{% if score >= 90 %}high{% elif score >= 50 %}pass{% else %}fail{% endif %}">
        {{ score }}
    </li>
{% endfor %}
</ul>

<p>Average: {{ average | round(1) }}</p>

{% endblock %}
```

### Redirects and Errors

```python
from flask import redirect, url_for, abort

@app.route('/old-page')
def old_page():
    return redirect(url_for('home'))    # redirect to home route

@app.route('/admin')
def admin():
    if not user_is_admin:
        abort(403)                      # return 403 Forbidden
    return render_template('admin.html')

@app.errorhandler(404)
def not_found(e):
    return render_template('404.html'), 404
```

### Project Structure

```
my-app/
├── app.py
├── requirements.txt
├── static/
│   ├── css/
│   │   └── style.css
│   └── js/
│       └── script.js
└── templates/
    ├── base.html
    ├── index.html
    ├── about.html
    └── dashboard.html
```

Flask automatically serves files from `static/` at `/static/filename`.

---

## Objective Response Questions

**1.** A Flask route is defined as `@app.route('/student/<int:student_id>')`. A browser requests `/student/42`. What value does `student_id` have inside the view function?

- A) `"42"` (string)
- B) `42` (integer)
- C) `None`
- D) A Flask request object

**Answer: B** — The `int:` type converter ensures the captured value is an integer.

---

**2.** Which Flask function is used to load an HTML template file and insert data into it before sending the response?

- A) `request.render()`
- B) `Flask.template()`
- C) `render_template()`
- D) `url_for()`

**Answer: C**

---

**3.** A developer runs Flask with `debug=True` in a production environment. What is the primary security risk?

- A) The application will run slower
- B) The interactive debugger exposes server-side code and allows arbitrary code execution
- C) Debug mode disables HTTPS
- D) Templates will not render correctly

**Answer: B** — Flask's debug mode includes an interactive browser-based debugger that allows execution of Python code on the server — a critical vulnerability in production.

---

## Try It

In your Codespaces Flask project:

1. Create a `base.html` with header, nav, main block, and footer
2. Create routes and templates for: `/` (home), `/about`, `/topics`
3. Pass a Python list of web topics to the `/topics` route and render them as cards using a `{% for %}` loop
4. Add a URL parameter route `/topic/<name>` that displays a detail page for that topic
5. Run Flask and verify all routes work: `python3 app.py`
