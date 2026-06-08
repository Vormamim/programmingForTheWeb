---
title: Day 11 — CSS Layout and Flexbox
parent: Week 2 — Front-End Foundations
nav_order: 5
---

# Day 11 — CSS Layout: Flexbox and Responsive Design

**Syllabus:** CSS flexibility with browsers or display devices
**Outcomes:** SE-12-02, SE-12-06
**Textbook:** Section 12.4

---

## Key Terms

**Flexbox** — A CSS layout model for arranging elements in a row or column, distributing space and aligning items efficiently.

**Grid** — A CSS layout model for two-dimensional layouts (rows and columns simultaneously).

**Responsive design** — Layouts that adapt to different screen sizes automatically.

**Media query** — CSS syntax that applies styles only when screen conditions are met (e.g. minimum width).

**Viewport** — The visible area of a web page in the browser window.

---

## Theory

### Flexbox

Apply `display: flex` to a parent element to control how its children are arranged.

```css
.container {
    display: flex;
    flex-direction: row;        /* row (default) or column */
    justify-content: space-between; /* horizontal alignment */
    align-items: center;        /* vertical alignment */
    gap: 1rem;                  /* space between children */
}
```

| Property | Values | Effect |
|---|---|---|
| `flex-direction` | `row`, `column` | Direction of children |
| `justify-content` | `flex-start`, `center`, `space-between`, `space-around` | Alignment along main axis |
| `align-items` | `flex-start`, `center`, `flex-end`, `stretch` | Alignment along cross axis |
| `flex-wrap` | `wrap`, `nowrap` | Whether children wrap to next line |
| `gap` | any size | Space between children |

**Common patterns:**

```css
/* Nav bar — logo left, links right */
nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

/* Centred content */
.hero {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
}

/* Card row that wraps on small screens */
.cards {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
}
.card {
    flex: 1 1 250px; /* grow, shrink, min-width */
}
```

### CSS Grid

For two-dimensional layouts:

```css
.grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr); /* 3 equal columns */
    gap: 1rem;
}

/* Sidebar + main layout */
.layout {
    display: grid;
    grid-template-columns: 250px 1fr;
    gap: 2rem;
}
```

### Responsive Design with Media Queries

```css
/* Default: mobile first */
.cards {
    display: flex;
    flex-direction: column;
}

/* Tablet and up */
@media (min-width: 600px) {
    .cards {
        flex-direction: row;
        flex-wrap: wrap;
    }
}

/* Desktop */
@media (min-width: 900px) {
    .cards {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
    }
}
```

Always include in `<head>`:
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

Without this, mobile browsers zoom out to fit a desktop layout.

---

## Objective Response Questions

**1.** A developer wants navigation links to appear side-by-side on desktop but stack vertically on mobile. Which CSS feature achieves this?

- A) CSS variables
- B) Flexbox with media queries
- C) CSS specificity rules
- D) The box model

**Answer: B**

---

**2.** Which CSS property controls spacing between flex children?

- A) `margin-between`
- B) `padding`
- C) `gap`
- D) `flex-wrap`

**Answer: C**

---

**3.** A page displays correctly on desktop but text is tiny on mobile. What is the most likely missing element?

- A) `box-sizing: border-box`
- B) `display: flex` on the body
- C) The viewport `<meta>` tag
- D) A media query for large screens

**Answer: C**

---

## Try It

Add to your `index.html` and `styles.css` from Day 10:

1. Wrap your `<nav>` links in a flex container — logo/title on the left, links on the right
2. Create a `.cards` section with at least three cards using flexbox
3. Add a media query so cards stack vertically on screens narrower than 600px
4. Use DevTools device toolbar (Ctrl+Shift+M) to preview at iPhone and iPad sizes
