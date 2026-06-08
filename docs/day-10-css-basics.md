# Day 10 — CSS — Selectors, Properties and Linking

**Syllabus:** Investigate cascading style sheets (CSS) and its impact on the design of a web application — consistency of appearance, flexibility with browsers or display devices, CSS maintenance tools
**Outcomes:** SE-12-02, SE-12-06
**Textbook:** Section 12.4

---

## Key Terms

**CSS (Cascading Style Sheets)** — A language for describing the visual presentation of HTML documents. CSS controls layout, colours, fonts, spacing, and responsiveness.

**Selector** — The part of a CSS rule that identifies which HTML element(s) to style. Examples: `h1`, `.card`, `#header`.

**Property** — The aspect of an element being styled. Examples: `color`, `font-size`, `margin`.

**Value** — The setting applied to a property. Examples: `red`, `16px`, `auto`.

**Declaration** — A single property-value pair. Example: `color: red;`

**Rule** — A complete CSS block: selector + declarations. Example:
```css
p {
    color: red;
    font-size: 16px;
}
```

**Cascade** — The mechanism by which CSS resolves conflicts when multiple rules apply to the same element. Rules are applied in order of specificity and source order.

**Specificity** — A score that determines which CSS rule takes priority when multiple rules target the same element. ID selectors beat class selectors beat element selectors.

**Inheritance** — Some CSS properties (like `color` and `font-family`) are automatically passed from parent elements to their children.

**Box model** — The CSS model that describes every element as a rectangular box with content, padding, border, and margin layers.

**Responsive design** — Designing web pages that adapt their layout and appearance to different screen sizes and devices.

**Media query** — A CSS technique for applying styles only when certain conditions are met, such as a minimum screen width.

---

## Theory

### Three Ways to Add CSS

**1. External stylesheet (recommended)**

Create a separate `.css` file and link it in the HTML `<head>`:

```html
<link rel="stylesheet" href="styles.css">
```

```css
/* styles.css */
body {
    font-family: Arial, sans-serif;
    background-color: #f5f5f5;
}
```

Best for: all production sites. Keeps style separate from structure. One stylesheet can control an entire site — change one file, update every page.

**2. Internal styles**

CSS written inside a `<style>` tag in the `<head>`:

```html
<head>
    <style>
        h1 { color: navy; }
    </style>
</head>
```

Best for: single-page prototypes. Not recommended for multi-page sites.

**3. Inline styles**

CSS written directly on an element using the `style` attribute:

```html
<p style="color: red; font-size: 18px;">This text is red.</p>
```

Best for: overrides, dynamic styles applied by JavaScript. Avoid for general styling — hard to maintain.

### Selectors

```css
/* Element selector — targets all <p> elements */
p {
    color: #333;
}

/* Class selector — targets elements with class="card" */
.card {
    border: 1px solid #ccc;
    padding: 1rem;
}

/* ID selector — targets element with id="header" */
#header {
    background-color: navy;
    color: white;
}

/* Descendant selector — targets <a> inside <nav> */
nav a {
    text-decoration: none;
    color: white;
}

/* Multiple selectors — apply same rule to h1 and h2 */
h1, h2 {
    font-family: Georgia, serif;
}

/* Pseudo-class — targets a link when hovered */
a:hover {
    color: orange;
}
```

### Specificity

When two rules target the same element, the more specific rule wins:

| Selector type | Specificity score |
|---|---|
| Inline style | 1000 |
| ID selector (`#id`) | 100 |
| Class selector (`.class`) | 10 |
| Element selector (`p`) | 1 |

```css
p { color: black; }          /* specificity: 1 */
.intro { color: blue; }      /* specificity: 10 */
#main p { color: green; }    /* specificity: 101 */
```

If `<p class="intro" id="main">` exists, the text is green (highest specificity wins).

### The Box Model

Every HTML element is a rectangular box with four layers:

```
┌─────────────────────────────────┐
│           MARGIN                │  (space outside the border)
│  ┌───────────────────────────┐  │
│  │         BORDER            │  │  (visible edge)
│  │  ┌─────────────────────┐  │  │
│  │  │      PADDING        │  │  │  (space between content and border)
│  │  │  ┌───────────────┐  │  │  │
│  │  │  │    CONTENT    │  │  │  │  (text, image, etc.)
│  │  │  └───────────────┘  │  │  │
│  │  └─────────────────────┘  │  │
│  └───────────────────────────┘  │
└─────────────────────────────────┘
```

```css
.box {
    width: 300px;           /* content width */
    padding: 20px;          /* space inside border */
    border: 2px solid #333; /* visible border */
    margin: 10px;           /* space outside border */

    /* By default, padding and border ADD to the width.
       box-sizing: border-box includes them IN the width — much easier to work with */
    box-sizing: border-box;
}
```

### Common CSS Properties

```css
/* Typography */
font-family: Arial, Helvetica, sans-serif;
font-size: 16px;
font-weight: bold;        /* or: normal, 100-900 */
line-height: 1.5;
text-align: center;       /* left, right, center, justify */
text-decoration: none;    /* removes underline from links */
color: #333333;           /* text colour */

/* Background */
background-color: #f0f0f0;
background-image: url('bg.jpg');

/* Spacing */
margin: 10px;             /* all sides */
margin: 10px 20px;        /* top/bottom, left/right */
margin: 10px 20px 15px 5px; /* top right bottom left */
padding: 1rem;

/* Border */
border: 1px solid #ccc;
border-radius: 8px;       /* rounded corners */

/* Size */
width: 100%;
max-width: 800px;
height: auto;
```

### CSS and Consistency of Appearance

A single external stylesheet applied to all pages ensures visual consistency:
- All headings use the same font and colour
- Buttons look the same everywhere
- Changes made in one file update the entire site instantly

This is a core advantage of CSS over inline styles — **one change, site-wide effect**.

### CSS Maintenance Tools

**CSS variables (custom properties)** — define values once, reuse everywhere:

```css
:root {
    --primary-colour: #0077cc;
    --font-size-base: 16px;
    --spacing-unit: 1rem;
}

h1 { color: var(--primary-colour); }
button { background-color: var(--primary-colour); }
```

Change `--primary-colour` once → updates every element using it.

**CSS preprocessors (Sass, Less)** — extend CSS with variables, nesting, and functions. Compiled to standard CSS before deployment.

**Browser DevTools** — allow live editing of CSS to test changes before writing them into the file.

---

## Objective Response Questions

**1.** A developer wants to apply the same font style to every page of a 50-page website. Which CSS approach is most appropriate for maintaining consistency?

- A) Inline styles on each element
- B) A `<style>` block in each page's `<head>`
- C) A single external stylesheet linked to all pages
- D) CSS written directly in each HTML attribute

**Answer: C** — An external stylesheet applies to all linked pages. Changes made to the stylesheet automatically apply site-wide.

---

**2.** Consider the following CSS:
```css
p { color: black; }
.highlight { color: yellow; }
#intro { color: red; }
```
An element `<p class="highlight" id="intro">` will display in which colour?

- A) Black
- B) Yellow
- C) Red
- D) The browser default

**Answer: C** — The ID selector (`#intro`) has the highest specificity (100), overriding the class (10) and element (1) selectors.

---

**3.** Which of the following best describes a benefit of using CSS custom properties (variables)?

- A) They allow CSS to run server-side logic
- B) They enable a single value to be updated in one place and applied consistently throughout the stylesheet
- C) They replace the need for a separate CSS file
- D) They automatically adjust colours for accessibility

**Answer: B**

---

## Try It

In your Codespaces environment, create `styles.css` and link it to your `index.html` from Day 9.

Apply the following styling:

1. Set a `font-family` for the whole page using the `body` selector
2. Give the `<header>` a background colour and white text
3. Style the `<nav>` links — remove underlines, add spacing between them
4. Style your table with borders, padding, and alternating row colours using:
   ```css
   tr:nth-child(even) { background-color: #f2f2f2; }
   ```
5. Define at least two CSS variables in `:root` and use them in your stylesheet
6. Add a `:hover` effect to your navigation links

View the result in the browser preview and verify it looks as expected.
