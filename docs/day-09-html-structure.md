---
title: Day 9 — HTML Structure and Tags
parent: Week 2 — Design and Front-End Foundations
nav_order: 4
---

# Day 9 — HTML Structure and Tags

**Syllabus:** Foundation for CSS and web application development — structure of HTML documents, semantic elements
**Outcomes:** SE-12-02
**Textbook:** Section 12.4 (foundation)

---

## Key Terms

**HTML (HyperText Markup Language)** — The standard language for creating web pages. HTML describes the structure and content of a page using tags.

**Tag** — An HTML element marker enclosed in angle brackets. Most tags come in pairs: an opening tag `<p>` and a closing tag `</p>`.

**Element** — A complete HTML component consisting of an opening tag, content, and closing tag. Example: `<p>Hello</p>`.

**Attribute** — Additional information provided inside an opening tag. Format: `name="value"`. Example: `<img src="photo.jpg" alt="A photo">`.

**Semantic HTML** — Using HTML elements that convey meaning about the content, not just its appearance. Example: `<nav>` tells the browser (and screen readers) this is navigation, whereas `<div>` conveys nothing.

**Void element** — An HTML element with no closing tag and no content. Examples: `<img>`, `<br>`, `<input>`, `<link>`, `<meta>`.

**DOCTYPE** — A declaration at the top of an HTML file that tells the browser which version of HTML is being used. Modern pages use `<!DOCTYPE html>`.

**Head section** — The part of an HTML document containing metadata about the page (title, character encoding, linked stylesheets). Not displayed on the page.

**Body section** — The part of an HTML document containing visible content.

---

## Theory

### HTML Document Structure

Every HTML document follows the same basic structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Page</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Hello, world!</h1>
    <p>This is a paragraph.</p>
</body>
</html>
```

- `<!DOCTYPE html>` — tells the browser to use HTML5
- `<html lang="en">` — root element, declares the page language
- `<head>` — metadata: character set, title, linked CSS
- `<meta charset="UTF-8">` — character encoding (supports all languages)
- `<meta name="viewport">` — makes the page scale correctly on mobile
- `<title>` — text shown in the browser tab
- `<body>` — everything visible on the page

### Core HTML Elements

#### Headings
Six levels of heading, h1 (most important) to h6 (least):

```html
<h1>Main Page Title</h1>
<h2>Section Heading</h2>
<h3>Subsection Heading</h3>
```

Use headings to create a logical hierarchy — not to make text bigger. Screen readers use heading structure to navigate pages.

#### Paragraphs and Text

```html
<p>This is a paragraph of text.</p>

<strong>Bold text</strong> — semantic, means "important"
<em>Italic text</em> — semantic, means "emphasis"
<br> — line break (void element, no closing tag)
```

#### Links

```html
<!-- Basic link -->
<a href="https://www.example.com">Visit Example</a>

<!-- Open in new tab -->
<a href="https://www.example.com" target="_blank" rel="noreferrer">External Link</a>

<!-- Link to another page in same site -->
<a href="about.html">About</a>

<!-- Link to a section on the same page -->
<a href="#contact">Jump to Contact</a>
<h2 id="contact">Contact Us</h2>
```

#### Images

```html
<!-- alt text is required for accessibility -->
<img src="photo.jpg" alt="Students working on computers">

<!-- Control size with CSS, not HTML width/height where possible -->
<img src="logo.png" alt="School logo" style="width: 200px;">
```

#### Lists

```html
<!-- Unordered (bullet) list -->
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
</ul>

<!-- Ordered (numbered) list -->
<ol>
    <li>Plan</li>
    <li>Design</li>
    <li>Build</li>
    <li>Test</li>
</ol>
```

#### Tables

```html
<table>
    <thead>
        <tr>
            <th>Protocol</th>
            <th>Port</th>
            <th>Encrypted</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HTTP</td>
            <td>80</td>
            <td>No</td>
        </tr>
        <tr>
            <td>HTTPS</td>
            <td>443</td>
            <td>Yes</td>
        </tr>
    </tbody>
</table>
```

#### Forms

```html
<form action="/submit" method="post">
    <label for="username">Username:</label>
    <input type="text" id="username" name="username" required>

    <label for="password">Password:</label>
    <input type="password" id="password" name="password" required>

    <label for="level">Year level:</label>
    <select id="level" name="level">
        <option value="11">Year 11</option>
        <option value="12">Year 12</option>
    </select>

    <textarea id="comments" name="comments" rows="4" placeholder="Comments..."></textarea>

    <button type="submit">Submit</button>
</form>
```

### Semantic HTML Elements

Semantic elements give meaning to the structure of a page:

```html
<header>
    <nav>
        <a href="/">Home</a>
        <a href="/about">About</a>
    </nav>
</header>

<main>
    <article>
        <h1>Article Title</h1>
        <p>Article content...</p>
    </article>

    <aside>
        <p>Related links...</p>
    </aside>
</main>

<footer>
    <p>© 2025 My Website</p>
</footer>
```

| Element | Meaning |
|---|---|
| `<header>` | Page or section header |
| `<nav>` | Navigation links |
| `<main>` | Primary page content (one per page) |
| `<article>` | Self-contained content (blog post, news article) |
| `<section>` | Thematic grouping of content |
| `<aside>` | Related content, sidebars |
| `<footer>` | Page or section footer |
| `<figure>` | Image, diagram, or code with a caption |
| `<figcaption>` | Caption for a `<figure>` |

**Why semantic HTML matters:**
- Screen readers use it to navigate (jump to `<main>`, skip `<nav>`)
- Search engines use it to understand page content
- Makes code easier to read and maintain
- Required for WCAG compliance

### Nesting Rules

HTML elements must be properly nested — inner elements must close before outer elements:

```html
<!-- Correct nesting -->
<p>This is <strong>important</strong> text.</p>

<!-- Incorrect nesting (will cause rendering issues) -->
<p>This is <strong>important</p></strong>
```

---

## Objective Response Questions

**1.** A developer writes `<div class="nav">` to wrap navigation links instead of `<nav>`. What is the primary disadvantage of this approach?

- A) `<div>` elements cannot contain links
- B) The element has no semantic meaning, making the page less accessible to screen readers
- C) `<div>` elements cannot be styled with CSS
- D) The page will not render in Chrome

**Answer: B** — `<div>` is a generic container with no meaning. `<nav>` tells browsers and assistive technologies that the content is navigation.

---

**2.** Which HTML element is used to associate a text label with a form input, improving accessibility?

- A) `<caption>`
- B) `<legend>`
- C) `<label>`
- D) `<title>`

**Answer: C** — `<label for="id">` links a label to an input by matching the input's `id`. Screen readers announce the label when the input is focused.

---

**3.** A web page contains the following code: `<img src="graph.png">`. What accessibility requirement is missing?

- A) A `width` attribute specifying the image size
- B) A `title` attribute for the page
- C) An `alt` attribute describing the image content
- D) A `class` attribute for CSS styling

**Answer: C** — The `alt` attribute provides a text alternative for users who cannot see the image.

---

## Try It

In your Codespaces environment, create a file called `index.html` with the following:

1. A proper HTML5 document structure (DOCTYPE, head, body)
2. A `<header>` with a `<nav>` containing three links (Home, About, Contact)
3. A `<main>` section with:
   - An `<h1>` heading
   - Two paragraphs of text
   - An unordered list of at least four items
   - A table with at least three rows and three columns (use the protocol table from Day 3 if you like)
4. A `<footer>` with your name and the current year
5. All images (if any) must have `alt` attributes
6. Open it in the browser preview and check it renders correctly
