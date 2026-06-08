---
title: Day 22 — SQL Advanced
parent: Week 4 — Back-End and Databases
nav_order: 4
---

# Day 22 — SQL: GROUP BY, JOIN and Constraints

**Syllabus:** GROUP BY, table joins, constraints using WHERE keyword, compare ORM to SQL
**Outcomes:** SE-12-02
**Textbook:** Section 13.2

---

## Key Terms

**GROUP BY** — Aggregates rows sharing a common value. Used with aggregate functions like COUNT, AVG, SUM, MAX, MIN.

**Aggregate function** — A function that performs a calculation on a set of rows and returns a single value.

**JOIN** — Combines rows from two or more tables based on a related column.

**Foreign key** — A column in one table that references the primary key of another table, establishing a relationship.

**INNER JOIN** — Returns only rows where the join condition is met in both tables.

**LEFT JOIN** — Returns all rows from the left table, and matched rows from the right. Unmatched right-table rows are NULL.

**ORM (Object-Relational Mapping)** — A technique that lets developers interact with a database using Python objects instead of writing SQL directly. Example: SQLAlchemy.

---

## Theory

### Aggregate Functions

```sql
SELECT COUNT(*) FROM students;                    -- total rows
SELECT COUNT(*) FROM students WHERE year = 12;    -- count Year 12
SELECT AVG(score) FROM students;                  -- average score
SELECT MAX(score) FROM students;                  -- highest score
SELECT MIN(score) FROM students;                  -- lowest score
SELECT SUM(score) FROM students;                  -- sum of all scores
```

### GROUP BY

Groups rows with the same value in a column, then applies an aggregate function to each group:

```sql
-- Average score per year level
SELECT year, AVG(score) AS avg_score, COUNT(*) AS student_count
FROM students
GROUP BY year
ORDER BY year;
```

Result:
```
year | avg_score | student_count
-----|-----------|---------------
  11 |      78.4 |            12
  12 |      82.1 |            15
```

```sql
-- Count students per year, only years with more than 10 students
SELECT year, COUNT(*) AS total
FROM students
GROUP BY year
HAVING COUNT(*) > 10;
```

`HAVING` filters groups (like WHERE, but applied after GROUP BY).

### Multiple Tables and Foreign Keys

Real applications use multiple related tables:

```sql
CREATE TABLE subjects (
    id   INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL
);

CREATE TABLE enrolments (
    id         INTEGER PRIMARY KEY AUTOINCREMENT,
    student_id INTEGER NOT NULL,
    subject_id INTEGER NOT NULL,
    score      REAL,
    FOREIGN KEY (student_id) REFERENCES students(id),
    FOREIGN KEY (subject_id) REFERENCES subjects(id)
);
```

### JOIN

```sql
-- INNER JOIN: students and their enrolled subjects
SELECT students.name, subjects.name AS subject, enrolments.score
FROM enrolments
INNER JOIN students ON enrolments.student_id = students.id
INNER JOIN subjects ON enrolments.subject_id = subjects.id
ORDER BY students.name;
```

Result:
```
name      | subject              | score
----------|----------------------|-------
Alex Chen | Software Engineering |  85.0
Alex Chen | Mathematics          |  91.0
Sam Nguyen| Software Engineering |  78.5
```

```sql
-- LEFT JOIN: all students, including those not enrolled in anything
SELECT students.name, subjects.name AS subject
FROM students
LEFT JOIN enrolments ON students.id = enrolments.student_id
LEFT JOIN subjects   ON enrolments.subject_id = subjects.id;
```

Students with no enrolments appear with NULL for subject.

### Using JOINs in Flask

```python
@app.route('/results')
def results():
    conn = get_db()
    rows = conn.execute('''
        SELECT students.name, subjects.name AS subject, enrolments.score
        FROM enrolments
        INNER JOIN students ON enrolments.student_id = students.id
        INNER JOIN subjects ON enrolments.subject_id = subjects.id
        ORDER BY students.name, subjects.name
    ''').fetchall()
    conn.close()
    return render_template('results.html', results=rows)
```

### ORM vs SQL

An ORM (like SQLAlchemy) maps database tables to Python classes:

```python
# SQL approach
conn.execute('SELECT * FROM students WHERE year = 12')

# ORM approach (SQLAlchemy)
Student.query.filter_by(year=12).all()
```

| | Raw SQL | ORM |
|---|---|---|
| Control | Full | Limited by ORM capabilities |
| Readability | Requires SQL knowledge | Python-like syntax |
| Security | Manual parameterisation required | Parameterisation built-in |
| Performance | Optimal — write exactly what you need | May generate inefficient queries |
| Portability | Database-specific syntax | Switch databases more easily |
| Learning curve | Need SQL | Need ORM + some SQL |

**When to use raw SQL:** complex queries, performance-critical paths, learning SQL fundamentals.
**When to use ORM:** rapid development, simple queries, team familiar with Python but not SQL.

This course uses raw SQL with parameterised queries to build genuine SQL competence.

### Parameterised Queries (Critical Security Practice)

Never insert user input directly into SQL strings — this enables SQL injection attacks:

```python
# ❌ DANGEROUS — SQL injection vulnerability
username = request.form.get('username')
conn.execute(f"SELECT * FROM users WHERE username = '{username}'")
# Attacker inputs: ' OR '1'='1
# Resulting query: SELECT * FROM users WHERE username = '' OR '1'='1'
# Returns ALL users — authentication bypassed

# ✅ SAFE — parameterised query
conn.execute("SELECT * FROM users WHERE username = ?", (username,))
```

Always use `?` placeholders. SQLite handles escaping automatically.

---

## Objective Response Questions

**1.** A developer wants to find the average score for each subject, but only show subjects where the average is above 75. Which SQL clause filters groups after aggregation?

- A) `WHERE`
- B) `HAVING`
- C) `GROUP BY`
- D) `ORDER BY`

**Answer: B** — `HAVING` filters aggregated groups. `WHERE` filters individual rows before aggregation.

---

**2.** A database has a `students` table and an `enrolments` table. A student may have no enrolments. Which JOIN type returns all students, including those with no enrolments?

- A) INNER JOIN
- B) RIGHT JOIN
- C) LEFT JOIN
- D) CROSS JOIN

**Answer: C** — LEFT JOIN returns all rows from the left table (students) regardless of whether a match exists in the right table (enrolments).

---

**3.** A developer writes: `conn.execute(f"SELECT * FROM users WHERE id = {user_id}")`. What security risk does this introduce?

- A) The query will fail if `user_id` is a large number
- B) SQL injection — malicious input in `user_id` could manipulate the query
- C) The query returns too many columns
- D) SQLite does not support f-strings

**Answer: B** — String formatting SQL queries with user input allows SQL injection. Use parameterised queries with `?` placeholders instead.

---

## Try It

Extend your database from Day 21:

1. Create a `subjects` table and an `enrolments` table with foreign keys
2. Insert data: at least three subjects, each student enrolled in two or more subjects with scores
3. Write queries for:
   - Average score per subject using GROUP BY
   - All enrolments showing student name and subject name using INNER JOIN
   - Subjects with average score above 75 using HAVING
4. Add a `/results` route displaying the JOIN query results in a formatted table
5. Demonstrate one parameterised query and one dangerous f-string query (comment out the dangerous one with a note explaining why it is unsafe)
