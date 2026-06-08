---
title: Day 24 — Web Performance
parent: Week 4 — Back-End and Databases
nav_order: 6
---

# Day 24 — Web Performance

**Syllabus:** Investigate methods to support and manage the load times of web pages and applications
**Outcomes:** SE-12-04
**Textbook:** Section 13.3

---

## Key Terms

**Page load time** — The time from a user requesting a page to it being fully usable. A key measure of web performance.

**Render-blocking resource** — A CSS or JavaScript file that prevents the browser from rendering the page until it has downloaded and processed.

**Minification** — Removing whitespace, comments, and unnecessary characters from CSS and JS files to reduce file size without changing functionality.

**Caching** — Storing copies of resources locally (browser cache) or on intermediate servers (CDN) so they do not need to be re-downloaded on repeat visits.

**Lazy loading** — Deferring the loading of non-critical resources (images, scripts) until they are needed — typically when scrolled into view.

**CDN (Content Delivery Network)** — A distributed network of servers that serve static assets from the location nearest to the user.

**GZIP compression** — Server-side compression of text-based files (HTML, CSS, JS) before transmission. Reduces transfer size by 60–80%.

**Core Web Vitals** — Google's set of performance metrics: LCP (Largest Contentful Paint), FID (First Input Delay), CLS (Cumulative Layout Shift).

---

## Theory

### Why Performance Matters

- **53%** of mobile users abandon a site that takes more than 3 seconds to load (Google, 2018)
- Google uses page speed as a ranking factor in search results
- Each 100ms improvement in load time increases conversion rates by ~1%
- Poor performance disproportionately affects users on slow connections and older devices

### What Affects Load Time

```
User types URL → DNS lookup → TCP connection → TLS handshake
→ Server processes request → Server sends HTML
→ Browser parses HTML → Downloads CSS, JS, images
→ Browser renders page → Page is usable
```

Every step has a cost. Performance optimisation targets the biggest bottlenecks.

### Image Optimisation

Images are typically the largest resources on a page.

```html
<!-- Specify dimensions to prevent layout shift -->
<img src="hero.jpg" alt="Hero" width="1200" height="600">

<!-- Lazy load images below the fold -->
<img src="photo.jpg" alt="Photo" loading="lazy">

<!-- Modern formats: WebP is 25-35% smaller than JPEG -->
<picture>
    <source srcset="image.webp" type="image/webp">
    <img src="image.jpg" alt="Fallback for older browsers">
</picture>

<!-- Responsive images — serve appropriate size -->
<img src="small.jpg"
     srcset="small.jpg 480w, medium.jpg 800w, large.jpg 1200w"
     sizes="(max-width: 600px) 480px, (max-width: 900px) 800px, 1200px"
     alt="Responsive image">
```

### CSS and JavaScript Loading

**CSS** blocks rendering — the browser will not display content until CSS is loaded:

```html
<!-- In <head> — blocks rendering but must be there for correct display -->
<link rel="stylesheet" href="styles.css">

<!-- Preload critical CSS -->
<link rel="preload" href="critical.css" as="style">
```

**JavaScript** blocks parsing — place at end of `<body>` or use `defer`:

```html
<!-- Blocks parsing — avoid for non-critical scripts -->
<script src="script.js"></script>

<!-- defer: download in parallel, execute after HTML parsed -->
<script src="script.js" defer></script>

<!-- async: download in parallel, execute immediately when ready -->
<script src="analytics.js" async></script>
```

Use `defer` for scripts that need the DOM. Use `async` for independent scripts (analytics, ads).

### Caching

```python
# Flask — set cache headers for static files
from flask import Flask, send_from_directory

@app.route('/static/<path:filename>')
def static_files(filename):
    response = send_from_directory('static', filename)
    response.headers['Cache-Control'] = 'public, max-age=31536000'  # 1 year
    return response
```

**Browser caching:** resources with long `max-age` are stored locally and not re-downloaded on subsequent visits. Only change the filename when updating (cache busting: `style.v2.css`).

### GZIP Compression

Enable in Flask with the `flask-compress` library:

```bash
pip install flask-compress
```

```python
from flask_compress import Compress

app = Flask(__name__)
Compress(app)  # automatically compresses responses
```

Typical results: 80KB HTML → 20KB compressed.

### Minification

Minified CSS and JS remove whitespace and comments:

```css
/* Original: 450 bytes */
body {
    font-family: Arial, sans-serif;
    background-color: #ffffff;
    margin: 0;
    padding: 0;
}

/* Minified: 65 bytes */
body{font-family:Arial,sans-serif;background-color:#fff;margin:0;padding:0}
```

Tools: use build tools (Webpack, Vite) for production. In development, use readable source files.

### Measuring Performance

**Browser DevTools — Network panel:**
- Total page weight (KB)
- Number of requests
- Load time breakdown

**Browser DevTools — Lighthouse:**
- Performance score (0–100)
- Core Web Vitals
- Specific recommendations

**Online tools:**
- [PageSpeed Insights](https://pagespeed.web.dev) — Google's tool, tests real-world performance
- [WebPageTest](https://www.webpagetest.org) — detailed waterfall analysis

### Flask Performance Checklist

```python
# 1. Use a production WSGI server (not Flask's built-in)
# pip install gunicorn
# gunicorn app:app

# 2. Enable compression
from flask_compress import Compress
Compress(app)

# 3. Set cache headers on static files

# 4. Use a CDN for static assets in production

# 5. Index database columns used in WHERE and ORDER BY
# In SQL:
# CREATE INDEX idx_students_year ON students(year);
```

---

## Objective Response Questions

**1.** A web page has a large JavaScript file loaded in the `<head>` without `defer` or `async`. What performance problem does this cause?

- A) The script executes before CSS is applied
- B) The browser stops parsing HTML until the script is downloaded and executed, delaying visible content
- C) The script cannot access DOM elements
- D) The script is cached for too long

**Answer: B** — Render-blocking scripts in `<head>` delay the display of page content. Use `defer` or move to end of `<body>`.

---

**2.** A developer adds `loading="lazy"` to images below the fold. What is the benefit?

- A) Images are compressed automatically
- B) Images are loaded only when the user scrolls near them, reducing initial page load time
- C) Images are cached permanently in the browser
- D) Images are converted to WebP format

**Answer: B**

---

**3.** A web page scores 45/100 on Lighthouse performance. Which of the following changes is likely to have the greatest impact?

- A) Adding more CSS comments for readability
- B) Changing the page background colour
- C) Compressing and resizing large images
- D) Adding more HTML semantic elements

**Answer: C** — Images are typically the largest resources. Optimising them has the greatest impact on load time.

---

## Try It

1. Run a Lighthouse audit on your Flask application (open in browser → DevTools → Lighthouse → Generate report)
2. Record your initial scores for Performance and Accessibility
3. Install `flask-compress` and add it to your app — re-run Lighthouse and compare
4. Add `loading="lazy"` to any images on your page
5. Move all `<script>` tags to the bottom of `<body>` with `defer`
6. Check the Network panel — identify the three largest resources and note their sizes
