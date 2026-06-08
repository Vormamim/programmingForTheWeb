---
title: Day 21 — SQL Basics
parent: Week 4 — Back-End and Databases
nav_order: 3
---

# Day 21 — SQL: SELECT, WHERE and ORDER BY

**Syllabus:** Apply a web-based database — selecting fields, WHERE constraints, common SQL queries, ORDER BY
**Outcomes:** SE-12-02
**Textbook:** Section 13.2

---

## Key Terms

**SQL (Structured Query Language)** — The standard language for managing and querying relational databases.

**Table** — A collection of related data organised in rows and columns. Similar to a spreadsheet.

**Row (record)** — A single entry in a table. Example: one student's data.

**Column (field)** — A specific attribute stored for each record. Example: `name`, `score`, `year`.

**Primary key** — A column (or combination of columns) that uniquely identifies each row. Usually an auto-incrementing integer `id`.

**Query** — A SQL statement that retrieves or modifies data.

**SQLite** — A lightweight, file-based SQL database. No server required. Used in Flask development and this course.

---

## Theory

### SQL Basics

SQL statements are written in uppercase by convention (not required):

```sql
-- Retrieve all columns from the students table
SELECT * FROM students;

-- Retrieve specific columns
SELECT name, score FROM students;

-- Retrieve with a condition
SELECT name, score FROM students WHERE year = 12;

-- Sort results
SELECT name, score FROM students ORDER BY score DESC;

-- Limit results
SELECT name, score FROM students ORDER BY score DESC LIMIT 5;
```

### Creating a Table

```sql
CREATE TABLE students (
    id      INTEGER PRIMARY KEY AUTOINCREMENT,
    name    TEXT NOT NULL,
    email   TEXT UNIQUE NOT NULL,
    year    INTEGER NOT NULL,
    score   REAL DEFAULT 0.0
);
```

**Constraints:**
- `NOT NULL` — field cannot be empty
- `UNIQUE` — no two rows can have the same value
- `DEFAULT` — value used if none is provided
- `PRIMARY KEY` — uniquely identifies the row
- `AUTOINCREMENT` — automatically increases for each new row

### INSERT

```sql
INSERT INTO students (name, email, year, score)
VALUES ('Alex Chen', 'alex@school.edu', 12, 85.5);

INSERT INTO students (name, email, year, score)
VALUES ('Sam Nguyen', 'sam@school.edu', 11, 92.0);
```

### SELECT with WHERE

```sql
-- Exact match
SELECT * FROM students WHERE year = 12;

-- Comparison operators
SELECT * FROM students WHERE score >= 90;
SELECT * FROM students WHERE score BETWEEN 70 AND 90;

-- Text matching
SELECT * FROM students WHERE name = 'Alex Chen';
SELECT * FROM students WHERE name LIKE 'A%';   -- starts with A
SELECT * FROM students WHERE email LIKE '%@school.edu';

-- Multiple conditions
SELECT * FROM students WHERE year = 12 AND score >= 80;
SELECT * FROM students WHERE year = 11 OR year = 12;

-- Negation
SELECT * FROM students WHERE NOT year = 12;
SELECT * FROM students WHERE score IS NOT NULL;
```

### ORDER BY

```sql
-- Ascending (default)
SELECT * FROM students ORDER BY name ASC;

-- Descending
SELECT * FROM students ORDER BY score DESC;

-- Multiple columns
SELECT * FROM students ORDER BY year DESC, score DESC;
```

### UPDATE and DELETE

```sql
-- Update specific records
UPDATE students SET score = 95.0 WHERE id = 1;
UPDATE students SET year = 12 WHERE year = 11;

-- Delete specific records
DELETE FROM students WHERE id = 3;
DELETE FROM students WHERE score < 50;

-- ⚠ Without WHERE, affects ALL rows:
DELETE FROM students;   -- deletes everything!
UPDATE students SET score = 0;  -- resets all scores!
```

Always include a `WHERE` clause with UPDATE and DELETE unless you intend to affect all rows.

### Using SQLite in Flask

```python
import sqlite3
from flask import Flask, render_template

app = Flask(__name__)
DATABASE = 'school.db'

def get_db():
    conn = sqlite3.connect(DATABASE)
    conn.row_factory = sqlite3.Row   # rows behave like dicts
    return conn

def init_db():
    conn = get_db()
    conn.execute('''
        CREATE TABLE IF NOT EXISTS students (
            id    INTEGER PRIMARY KEY AUTOINCREMENT,
            name  TEXT NOT NULL,
            year  INTEGER NOT NULL,
            score REAL DEFAULT 0.0
        )
    ''')
    conn.commit()
    conn.close()

@app.route('/students')
def students():
    conn = get_db()
    rows = conn.execute(
        'SELECT * FROM students ORDER BY score DESC'
    ).fetchall()
    conn.close()
    return render_template('students.html', students=rows)

if __name__ == '__main__':
    init_db()
    app.run(debug=True)
```

```html
<!-- templates/students.html -->
{% extends "base.html" %}
{% block content %}
<h1>Students</h1>
<table>
    <tr><th>Name</th><th>Year</th><th>Score</th></tr>
    {% for student in students %}
    <tr>
        <td>{{ student['name'] }}</td>
        <td>{{ student['year'] }}</td>
        <td>{{ student['score'] }}</td>
    </tr>
    {% endfor %}
</table>
{% endblock %}
```

---

## Objective Response Questions

**1.** Which SQL query retrieves the names and scores of all Year 12 students, sorted from highest to lowest score?

- A) `SELECT name, score FROM students WHERE year = 12 ORDER BY score ASC`
- B) `SELECT name, score FROM students ORDER BY score DESC WHERE year = 12`
- C) `SELECT name, score FROM students WHERE year = 12 ORDER BY score DESC`
- D) `SELECT * FROM students SORT BY score WHERE year = 12`

**Answer: C** — WHERE comes before ORDER BY. DESC gives highest first.

---

**2.** A developer runs `DELETE FROM students` without a WHERE clause. What is the result?

- A) An error — DELETE requires a WHERE clause
- B) The most recently inserted row is deleted
- C) All rows in the students table are deleted
- D) The students table itself is deleted

**Answer: C** — Without WHERE, DELETE affects every row. The table structure remains but all data is gone.

---

**3.** Which constraint ensures no two students can have the same email address?

- A) `NOT NULL`
- B) `PRIMARY KEY`
- C) `UNIQUE`
- D) `DEFAULT`

**Answer: C**

---

## Try It

In your Codespaces project:

1. Create `init_db.py` that creates a `students` table with: id, name, email, year, score
2. Insert at least six students with varied years and scores
3. Write and test these queries in a Python script:
   - All students in Year 12
   - Top 3 students by score
   - All students with score between 70 and 90
   - All students whose name starts with a specific letter
4. Add a `/students` route to Flask that displays results in an HTML table
