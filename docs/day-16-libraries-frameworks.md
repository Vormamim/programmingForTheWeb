---
title: Day 16 — Code Libraries and Frameworks
parent: Week 3 — Scripting and Frameworks
nav_order: 4
---

# Day 16 — Code Libraries and Frameworks

**Syllabus:** Explore the types and significance of code libraries for front-end web development — frameworks, template engines, predesigned CSS classes
**Outcomes:** SE-12-04
**Textbook:** Section 12.5

---

## Key Terms

**Library** — A collection of pre-written code that provides specific functionality. A developer chooses when to call it. Example: jQuery (DOM manipulation).

**Framework** — A structure that dictates how an application is built. The framework calls your code, not the other way around. Example: Flask, Django, React.

**Template engine** — Software that combines HTML templates with data to generate dynamic pages. Example: Jinja2 (used with Flask).

**CSS framework** — A pre-built stylesheet providing ready-made classes for layout, typography, and components. Example: Bootstrap, Tailwind CSS.

**CDN (Content Delivery Network)** — A network of servers delivering files from the nearest location. Used to load libraries without local installation.

**Package manager** — A tool that installs and manages code libraries. Python: `pip`. JavaScript: `npm`.

---

## Theory

### Library vs Framework

The key distinction is **control flow**:

| | Library | Framework |
|---|---|---|
| Who's in control | Your code calls the library | Framework calls your code |
| Flexibility | High — use what you need | Lower — follow the structure |
| Learning curve | Lower | Higher |
| Examples | jQuery, Lodash, Requests | Flask, Django, React, Vue |

**Analogy:** A library is a toolbox — you pick up tools when you need them. A framework is a production line — it tells you where to stand and what to do.

### Front-End CSS Frameworks

CSS frameworks provide pre-built classes that handle common styling patterns, eliminating the need to write repetitive CSS from scratch.

**Bootstrap** — the most widely used. Grid system, components, utilities:

```html
<!-- Link Bootstrap via CDN -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">

<!-- Pre-built classes -->
<div class="container">
    <div class="row">
        <div class="col-md-4">
            <div class="card shadow p-3">
                <h5 class="card-title">Card Title</h5>
                <p class="card-text">Some content here.</p>
                <a href="#" class="btn btn-primary">Go somewhere</a>
            </div>
        </div>
    </div>
</div>
```

**Tailwind CSS** — utility-first, compose styles directly in HTML:

```html
<div class="max-w-sm rounded overflow-hidden shadow-lg p-4 bg-white">
    <h2 class="text-xl font-bold text-gray-800">Card Title</h2>
    <p class="text-gray-600 mt-2">Some content here.</p>
    <button class="mt-4 bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-700">
        Click me
    </button>
</div>
```

**Comparison:**

| | Bootstrap | Tailwind |
|---|---|---|
| Approach | Component-based | Utility-first |
| Customisation | Moderate | High |
| File size | Larger | Smaller (purges unused) |
| Learning curve | Lower | Higher |
| Best for | Rapid prototyping | Custom designs |

### Template Engines — Jinja2

Jinja2 is Flask's default template engine. It lets you embed Python-like expressions into HTML:

```html
<!-- templates/index.html -->
<!DOCTYPE html>
<html>
<head><title>{{ page_title }}</title></head>
<body>
    <h1>Welcome, {{ username }}!</h1>

    <!-- Conditional -->
    {% if logged_in %}
        <p>You are logged in.</p>
    {% else %}
        <a href="/login">Log in</a>
    {% endif %}

    <!-- Loop -->
    <ul>
    {% for item in items %}
        <li>{{ item }}</li>
    {% endfor %}
    </ul>
</body>
</html>
```

```python
# app.py — pass data to the template
@app.route('/')
def home():
    return render_template('index.html',
        page_title="Home",
        username="Alex",
        logged_in=True,
        items=["HTML", "CSS", "JavaScript"]
    )
```

Jinja2 syntax summary:

| Syntax | Purpose |
|---|---|
| `{{ variable }}` | Output a variable |
| `{% if %} {% endif %}` | Conditional |
| `{% for %} {% endfor %}` | Loop |
| `{% extends "base.html" %}` | Template inheritance |
| `{% block content %} {% endblock %}` | Define/fill a block |

### Template Inheritance

Avoids repeating the same HTML structure on every page:

```html
<!-- templates/base.html -->
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}My Site{% endblock %}</title>
    <link rel="stylesheet" href="/static/css/style.css">
</head>
<body>
    <nav>...</nav>
    <main>
        {% block content %}{% endblock %}
    </main>
    <footer>...</footer>
</body>
</html>
```

```html
<!-- templates/about.html -->
{% extends "base.html" %}

{% block title %}About Us{% endblock %}

{% block content %}
    <h1>About This Site</h1>
    <p>We build web applications.</p>
{% endblock %}
```

---

## Objective Response Questions

**1.** A developer is using Flask and notices they are copying the same `<nav>` and `<footer>` HTML into every template. What is the most appropriate solution?

- A) Use a CSS framework to generate the navigation automatically
- B) Use Jinja2 template inheritance with a base template
- C) Store the nav HTML in a JavaScript variable and inject it with the DOM
- D) Use a separate Flask route for the navigation

**Answer: B** — Template inheritance allows a base layout to be shared across all pages, with child templates filling in specific content blocks.

---

**2.** Which of the following best distinguishes a framework from a library?

- A) Frameworks are always larger in file size than libraries
- B) Libraries are only used for back-end development
- C) A framework controls the flow of the application and calls the developer's code; a library is called by the developer's code
- D) Frameworks require a CDN; libraries do not

**Answer: C**

---

**3.** A developer adds Bootstrap to a project and uses the class `btn btn-primary` on a button. The button is styled correctly without writing any CSS. This demonstrates which benefit of CSS frameworks?

- A) Template inheritance
- B) Predesigned CSS classes reducing development time
- C) Server-side rendering
- D) Responsive media queries only

**Answer: B**

---

## Try It

1. Add Bootstrap to your `index.html` via CDN
2. Convert your card layout from Day 11 to use Bootstrap's grid and card classes
3. Create `templates/base.html` and `templates/index.html` using Jinja2 template inheritance in your Flask project
4. Pass a Python list of five items to the template and render them as an HTML list using a `{% for %}` loop
