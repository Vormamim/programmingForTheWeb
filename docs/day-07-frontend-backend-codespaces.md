---
title: Day 7 — Front-End vs Back-End + Codespaces
parent: Week 2 — Front-End Foundations
nav_order: 1
---

# Day 7 — Front-End vs Back-End + GitHub Codespaces Setup

**Syllabus:** Model elements that form a web development system — client-side (front-end) web programming, server-side (back-end) web programming, interfacing with databases (SQL and non-SQL)
**Outcomes:** SE-12-02
**Textbook:** Section 12.2

---

## Key Terms

**Client** — The device or application that requests data or services. In web development this is typically the user's browser.

**Server** — A computer that receives requests and sends back responses. Web servers host websites and applications.

**Front-end (client-side)** — Everything the user sees and interacts with directly in the browser. Built with HTML, CSS, and JavaScript. Runs on the user's machine.

**Back-end (server-side)** — The logic, data processing, and database interactions that happen on the server. The user never sees this directly. Built with languages like Python (Flask), JavaScript (Node.js), Ruby, PHP.

**Full-stack** — A developer (or application) that handles both front-end and back-end.

**API (Application Programming Interface)** — A defined set of rules for how two systems communicate. In web development, a back-end API receives requests and returns data (usually as JSON) to the front-end.

**Database** — An organised collection of structured data stored and accessed electronically.

**SQL (Structured Query Language)** — A language used to manage and query relational databases. Examples: SQLite, PostgreSQL, MySQL.

**NoSQL** — A category of databases that do not use the traditional table-based relational model. Better suited to unstructured or rapidly changing data. Examples: MongoDB (document), Redis (key-value), Cassandra (wide-column).

**HTTP request** — A message sent from the client to the server asking for data or an action. Common methods: GET (retrieve), POST (send data), PUT (update), DELETE (remove).

**HTTP response** — The server's reply to a request, including a status code and (usually) data.

**GitHub Codespaces** — A cloud-based development environment hosted by GitHub. Provides a full VS Code editor and Linux terminal in the browser — no local setup required.

---

## Theory

### The Client-Server Model

Every web interaction follows the same pattern:

```
Client (browser)          Server
      |                      |
      |--- HTTP request ----->|
      |                      | (processes request,
      |                      |  queries database,
      |                      |  builds response)
      |<-- HTTP response -----|
      |                      |
  Renders HTML/CSS/JS
```

**Example — loading a product page on an online store:**
1. Browser sends GET request to `https://store.com/products/42`
2. Server receives request
3. Server queries database: "Get product with id=42"
4. Database returns product data
5. Server builds an HTML page with the product data inserted
6. Server sends HTML back to browser
7. Browser renders the page

### Front-End Development

The front-end is everything rendered in the browser:

| Technology | Role |
|---|---|
| HTML | Structure — headings, paragraphs, links, forms |
| CSS | Presentation — colours, fonts, layout, spacing |
| JavaScript | Behaviour — interactivity, dynamic updates, API calls |

Front-end code runs on the **user's machine**. This means:
- It can be viewed by anyone (right-click → View Source)
- Never put passwords or secret keys in front-end code
- Performance depends on the user's device

### Back-End Development

The back-end handles:
- Business logic (calculating prices, checking stock)
- Authentication (verifying login credentials)
- Database operations (reading and writing data)
- Serving the front-end files

Back-end code runs on the **server**. Users never see it directly.

**Python Flask** is the back-end framework used in this course. A minimal Flask application:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return '<h1>Hello from the server!</h1>'

if __name__ == '__main__':
    app.run()
```

When a browser requests `/`, Flask runs the `home()` function and returns the HTML string as a response.

### SQL vs NoSQL Databases

**SQL (Relational) databases** store data in structured tables with defined columns:

```
students table:
| id | name       | year | score |
|----|------------|------|-------|
|  1 | Alex Chen  |   12 |    85 |
|  2 | Sam Nguyen |   11 |    92 |
```

Data is queried using SQL:
```sql
SELECT name, score FROM students WHERE year = 12;
```

Best for: structured data with clear relationships (e.g. school records, banking, e-commerce).

**NoSQL databases** store data in flexible formats:

```json
// MongoDB document
{
  "_id": "abc123",
  "name": "Alex Chen",
  "year": 12,
  "subjects": ["Software Engineering", "Maths"],
  "profile": { "bio": "...", "avatar": "..." }
}
```

Best for: flexible/varying data structures, large scale, rapid development (e.g. social media posts, product catalogues, real-time analytics).

| | SQL | NoSQL |
|---|---|---|
| Structure | Fixed schema (columns defined upfront) | Flexible schema |
| Query language | SQL | Varies (JSON-based queries, etc.) |
| Relationships | Strong (foreign keys, joins) | Weak or application-managed |
| Scale | Vertical (bigger server) | Horizontal (more servers) |
| Examples | SQLite, PostgreSQL, MySQL | MongoDB, Redis, Cassandra |

---

## GitHub Codespaces Setup

From today, you will write and run code in GitHub Codespaces — a full development environment in your browser. No installation required.

### Step 1 — Create a GitHub Account

If you do not already have one:
1. Go to [github.com](https://github.com)
2. Click **Sign up**
3. Use a personal email address
4. Choose a username you are comfortable using professionally

### Step 2 — Create a New Repository

1. Click the **+** icon (top right) → **New repository**
2. Name it: `hsc-software-engineering`
3. Set to **Public** (required for free GitHub Pages later)
4. Check **Add a README file**
5. Click **Create repository**

### Step 3 — Open a Codespace

1. From your new repository, click the green **Code** button
2. Select the **Codespaces** tab
3. Click **Create codespace on main**
4. Wait ~30 seconds for the environment to load
5. You now have a full VS Code environment in your browser

### Step 4 — Set Up Python and Flask

In the Codespaces terminal (bottom panel):

```bash
# Verify Python is installed
python3 --version

# Install Flask
pip install flask

# Create a test file
echo "from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return '<h1>Flask is working!</h1>'

if __name__ == '__main__':
    app.run(debug=True)" > app.py

# Run it
python3 app.py
```

When Flask starts, Codespaces will show a popup offering to open the running app in a browser tab. Click **Open in Browser**.

### Step 5 — Save Your Work

```bash
git add .
git commit -m "Flask setup test"
git push
```

Your code is now saved to GitHub.

---

## Objective Response Questions

**1.** A web application displays a personalised dashboard showing a user's recent orders. The order data is retrieved from a database and inserted into the HTML by the server before being sent to the browser. This is an example of:

- A) Client-side rendering
- B) Server-side rendering
- C) Static site generation
- D) Content delivery network caching

**Answer: B** — The server processes the data and builds the HTML before sending it to the client.

---

**2.** A developer needs to store product data for an online store. Each product has a fixed set of attributes: name, price, stock level, and category. Which type of database is most appropriate?

- A) NoSQL document database
- B) Key-value store
- C) SQL relational database
- D) Graph database

**Answer: C** — The data is structured with fixed attributes, making a relational SQL database appropriate.

---

**3.** Which of the following pieces of information should NEVER be placed in front-end JavaScript code?

- A) The URL of an API endpoint
- B) A database password
- C) A user's display name
- D) The current date

**Answer: B** — Front-end code is visible to anyone. Secrets like database passwords must only exist on the server.

---

## Try It

Complete the Codespaces setup steps above. Once Flask is running:

1. Modify `app.py` to add a second route:
   ```python
   @app.route('/about')
   def about():
       return '<h1>About Page</h1><p>This is my HSC Software Engineering project.</p>'
   ```
2. Restart Flask and visit `/about` in the browser.
3. Add your name and the current year to the home page response.
4. Commit and push your changes:
   ```bash
   git add .
   git commit -m "Added about route"
   git push
   ```
