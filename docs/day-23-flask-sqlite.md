---
title: Day 23 — Flask + SQLite
parent: Week 4 — Back-End and Databases
nav_order: 5
---

# Day 23 — Flask + SQLite: Connecting Front to Back

**Syllabus:** Apply a web-based database; assess the contribution of back-end web development to the success of a web application
**Outcomes:** SE-12-02
**Textbook:** Section 13.2

---

## Key Terms

**CRUD** — The four fundamental database operations: Create, Read, Update, Delete.

**Context** — In Flask, a request context is active for the duration of a single HTTP request. Database connections should be opened per-request and closed afterwards.

**g object** — A Flask object (`g`) for storing data during a request. Commonly used to store the database connection.

**teardown** — A Flask hook that runs after each request — used to close the database connection.

---

## Theory

### Full CRUD Application Pattern

This is the foundation of almost every web application — a complete flow from form to database to display.

**Database setup:**

```python
import sqlite3
from flask import Flask, render_template, request, redirect, url_for, flash, g

app = Flask(__name__)
app.secret_key = 'change-in-production'
DATABASE = 'app.db'

def get_db():
    if 'db' not in g:
        g.db = sqlite3.connect(DATABASE)
        g.db.row_factory = sqlite3.Row
    return g.db

@app.teardown_appcontext
def close_db(e=None):
    db = g.pop('db', None)
    if db is not None:
        db.close()

def init_db():
    db = get_db()
    db.executescript('''
        CREATE TABLE IF NOT EXISTS posts (
            id         INTEGER PRIMARY KEY AUTOINCREMENT,
            title      TEXT NOT NULL,
            content    TEXT NOT NULL,
            created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        );
    ''')
    db.commit()
```

### CREATE — Adding Records

```python
@app.route('/posts/new', methods=['GET', 'POST'])
def new_post():
    if request.method == 'POST':
        title   = request.form.get('title', '').strip()
        content = request.form.get('content', '').strip()

        if not title or not content:
            flash('Title and content are required.', 'error')
            return redirect(url_for('new_post'))

        db = get_db()
        db.execute(
            'INSERT INTO posts (title, content) VALUES (?, ?)',
            (title, content)
        )
        db.commit()
        flash('Post created.', 'success')
        return redirect(url_for('list_posts'))

    return render_template('posts/new.html')
```

### READ — Displaying Records

```python
@app.route('/posts')
def list_posts():
    db = get_db()
    posts = db.execute(
        'SELECT * FROM posts ORDER BY created_at DESC'
    ).fetchall()
    return render_template('posts/list.html', posts=posts)

@app.route('/posts/<int:post_id>')
def view_post(post_id):
    db = get_db()
    post = db.execute(
        'SELECT * FROM posts WHERE id = ?', (post_id,)
    ).fetchone()

    if post is None:
        flash('Post not found.', 'error')
        return redirect(url_for('list_posts'))

    return render_template('posts/view.html', post=post)
```

### UPDATE — Editing Records

```python
@app.route('/posts/<int:post_id>/edit', methods=['GET', 'POST'])
def edit_post(post_id):
    db = get_db()
    post = db.execute(
        'SELECT * FROM posts WHERE id = ?', (post_id,)
    ).fetchone()

    if post is None:
        return redirect(url_for('list_posts'))

    if request.method == 'POST':
        title   = request.form.get('title', '').strip()
        content = request.form.get('content', '').strip()

        db.execute(
            'UPDATE posts SET title = ?, content = ? WHERE id = ?',
            (title, content, post_id)
        )
        db.commit()
        flash('Post updated.', 'success')
        return redirect(url_for('view_post', post_id=post_id))

    return render_template('posts/edit.html', post=post)
```

### DELETE — Removing Records

```python
@app.route('/posts/<int:post_id>/delete', methods=['POST'])
def delete_post(post_id):
    db = get_db()
    db.execute('DELETE FROM posts WHERE id = ?', (post_id,))
    db.commit()
    flash('Post deleted.', 'success')
    return redirect(url_for('list_posts'))
```

Delete via POST, not GET. A GET request can be triggered by simply loading a URL (e.g. a crawler). Destructive actions must require a form submission.

### Templates for CRUD

```html
<!-- templates/posts/list.html -->
{% extends "base.html" %}
{% block content %}
<h1>Posts</h1>
<a href="/posts/new" class="btn">New Post</a>

{% for post in posts %}
<div class="card">
    <h2><a href="/posts/{{ post['id'] }}">{{ post['title'] }}</a></h2>
    <p>{{ post['created_at'] }}</p>
</div>
{% else %}
<p>No posts yet.</p>
{% endfor %}
{% endblock %}
```

```html
<!-- templates/posts/edit.html -->
{% extends "base.html" %}
{% block content %}
<h1>Edit Post</h1>
<form method="POST">
    <input type="text" name="title" value="{{ post['title'] }}" required>
    <textarea name="content" required>{{ post['content'] }}</textarea>
    <button type="submit">Save</button>
    <a href="/posts/{{ post['id'] }}">Cancel</a>
</form>

<!-- Delete button — separate POST form -->
<form method="POST" action="/posts/{{ post['id'] }}/delete"
      onsubmit="return confirm('Delete this post?')">
    <button type="submit" class="btn-danger">Delete</button>
</form>
{% endblock %}
```

### Back-End Contribution to Web Application Success

The back-end is what makes a web application genuinely useful:

| Without back-end | With back-end |
|---|---|
| Static content only | Dynamic, personalised content |
| No data persistence | Data saved between sessions |
| No authentication | Secure user accounts |
| No business logic | Rules enforced consistently |
| Single user only | Multiple concurrent users |

A well-designed back-end directly determines: performance under load, data integrity, security posture, and the ability to scale.

---

## Objective Response Questions

**1.** A delete button is implemented as a plain `<a href="/posts/5/delete">`. What is the security risk?

- A) The link will not work in all browsers
- B) Web crawlers or browser prefetching could trigger the delete without user intent
- C) GET requests cannot reach Flask routes
- D) The id must be passed as a POST parameter

**Answer: B** — GET requests can be triggered by anything that follows links. Destructive actions must use POST via a form.

---

**2.** A Flask application uses `g` to store the database connection. What is the main advantage of this approach over creating a new connection in every function?

- A) `g` connections are faster than regular SQLite connections
- B) The connection is shared across all requests simultaneously
- C) One connection is created per request and automatically closed when the request ends
- D) `g` connections do not require `db.commit()`

**Answer: C** — `g` is request-scoped. The `@app.teardown_appcontext` hook closes the connection cleanly after each request.

---

**3.** A developer forgets to call `db.commit()` after an INSERT statement. What is the result?

- A) Flask raises an exception immediately
- B) The data is written to the database but cannot be queried
- C) The INSERT is not saved — the transaction is rolled back when the connection closes
- D) The data is saved automatically after 30 seconds

**Answer: C** — SQLite requires explicit `commit()` to persist changes. Without it, the transaction is discarded.

---

## Try It

Build a complete CRUD application in Flask:

1. Topic: a simple **student results tracker** — table with student name, subject, score
2. Implement all four operations: list all, add new, edit existing, delete
3. All forms must use POST with parameterised queries
4. Add validation: score must be a number between 0 and 100
5. Use flash messages for all success and error feedback
6. Commit and push when working:
   ```bash
   git add .
   git commit -m "CRUD student results app"
   git push
   ```
