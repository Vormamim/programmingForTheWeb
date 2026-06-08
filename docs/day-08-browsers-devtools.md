---
title: Day 8 — Browsers and DevTools
parent: Week 2 — Design and Front-End Foundations
nav_order: 3
---

# Day 8 — Web Browsers and Developer Tools

**Syllabus:** Explore and explain the influence of a web browser on web development, including the use of developer (dev) tools
**Outcomes:** SE-12-04
**Textbook:** Section 12.3

---

## Key Terms

**Web browser** — An application that retrieves, renders, and displays web content. Examples: Chrome, Firefox, Safari, Edge.

**Rendering engine** — The component of a browser responsible for parsing HTML and CSS and displaying the visual result on screen. Also called a layout engine.

**Blink** — The rendering engine used by Chrome, Edge, Opera, and most modern browsers. Developed by Google.

**WebKit** — The rendering engine used by Safari. Developed by Apple. iOS requires all browsers to use WebKit.

**Gecko** — The rendering engine used by Firefox. Developed by Mozilla.

**DOM (Document Object Model)** — A tree-like representation of an HTML document that the browser creates in memory. JavaScript uses the DOM to read and modify page content dynamically.

**DevTools** — Built-in browser tools for inspecting, debugging, and profiling web pages. Accessed via F12 or right-click → Inspect.

**Console** — The DevTools panel for viewing JavaScript errors, log messages, and running JavaScript interactively.

**Network panel** — The DevTools panel that records all HTTP requests and responses made by a page, including timing information.

**Elements panel** — The DevTools panel showing the live DOM tree and applied CSS styles.

**Browser compatibility** — Whether a web feature works consistently across different browsers. Not all browsers support all features equally.

**Polyfill** — JavaScript code that provides modern functionality to older browsers that do not natively support it.

---

## Theory

### How a Browser Renders a Web Page

When you type a URL and press Enter, the browser follows these steps:

1. **DNS lookup** — resolve the domain name to an IP address
2. **TCP connection** — establish connection to the server
3. **TLS handshake** — if HTTPS, negotiate encryption
4. **HTTP request** — send GET request for the page
5. **Receive response** — server sends back HTML
6. **Parse HTML** — browser reads HTML and builds the DOM tree
7. **Fetch resources** — browser requests CSS, JS, images referenced in the HTML
8. **Parse CSS** — build the CSSOM (CSS Object Model)
9. **Render tree** — combine DOM + CSSOM to determine what to display
10. **Layout** — calculate the position and size of every element
11. **Paint** — draw pixels to screen

Steps 6–11 are the job of the **rendering engine**.

### Rendering Engines and Browser Compatibility

Different rendering engines implement web standards slightly differently, or at different times. This means a feature might work in Chrome but not in an older version of Safari.

| Browser | Engine | Maintained by |
|---|---|---|
| Chrome | Blink | Google |
| Edge | Blink | Microsoft |
| Firefox | Gecko | Mozilla |
| Safari | WebKit | Apple |
| Opera | Blink | Opera Software |

**Practical implication:** Always test web applications in multiple browsers. Use [caniuse.com](https://caniuse.com) to check which browsers support a given CSS or JavaScript feature before using it.

**Example:** CSS Grid was not supported in IE11. Developers who used Grid without a fallback broke their sites for IE11 users.

### Browser Influence on Web Development

Browsers do not just display pages — they actively shape how web development works:

- **Browser vendors propose and implement new features** — often before W3C standardisation. Chrome implemented many features (push notifications, service workers) that later became standards.
- **Developer tools accelerate development** — DevTools make it possible to debug, profile, and inspect pages without touching the source code.
- **Browser market share influences decisions** — if 60% of users use Chrome, developers prioritise Chrome compatibility.
- **Mobile browsers** — Safari on iOS has significant restrictions (e.g. all iOS browsers must use WebKit), which affects what web features are available on iPhones.

### Using Developer Tools

DevTools is one of the most important tools in a web developer's toolkit. Every major browser has its own version.

**Open DevTools:**
- Windows/Linux: `F12` or `Ctrl+Shift+I`
- Mac: `Cmd+Option+I`

**Key panels:**

#### Elements Panel
Shows the live DOM tree. You can click any element to see its HTML and applied CSS.
- Hover over an element in the panel — it highlights on the page
- Double-click to edit HTML directly (changes are temporary — refresh to reset)
- The Styles pane on the right shows all CSS rules applied to the selected element

#### Console Panel
A JavaScript REPL (Read-Eval-Print Loop). Use it to:
- View errors and warnings from the page
- Log values for debugging
- Run JavaScript directly against the live page

```javascript
// Try these in the console on any page:
document.title                          // Get the page title
document.querySelectorAll('a').length   // Count all links
document.body.style.background = 'red'  // Change the background
```

#### Network Panel
Records every HTTP request the page makes. Click any request to see:
- Request headers (what the browser sent)
- Response headers (what the server sent back)
- Response body (the actual data returned)
- Timing breakdown (DNS, connection, waiting, download)

Useful for:
- Checking what API calls a page makes
- Debugging slow load times
- Verifying HTTPS is being used

#### Application Panel
Shows storage used by the page:
- Cookies
- Local Storage and Session Storage
- Cache Storage (service workers)
- IndexedDB

#### Lighthouse Panel
Audits a page for performance, accessibility, SEO, and PWA compliance. Generates a scored report with specific recommendations.

---

## Objective Response Questions

**1.** A developer is using DevTools and notices a CSS rule is crossed out (struck through) in the Styles pane. What does this indicate?

- A) The rule contains a syntax error and will not apply
- B) The rule has been overridden by a more specific CSS rule
- C) The rule applies only to mobile devices
- D) The browser does not support this CSS property

**Answer: B** — Struck-through rules in DevTools indicate they have been overridden by a higher-specificity rule.

---

**2.** A web developer wants to check why a page is loading slowly. Which DevTools panel is most useful?

- A) Elements
- B) Console
- C) Network
- D) Application

**Answer: C** — The Network panel shows the timing of every request, identifying which resources are slowest to load.

---

**3.** A developer uses a CSS feature that works in Chrome but not in Safari. Which resource should they check to understand browser support for this feature?

- A) The W3C HTML specification
- B) The browser's Application panel
- C) caniuse.com
- D) The DNS lookup tool

**Answer: C** — caniuse.com provides detailed browser compatibility data for CSS and JavaScript features.

---

## Try It

Open DevTools on any website and complete the following:

1. **Elements panel:** Find the `<h1>` element. What font size is applied to it? What CSS rule sets it — is it from a stylesheet or inline?

2. **Console panel:** Run these two lines and record what they return:
   ```javascript
   document.querySelectorAll('img').length
   document.querySelectorAll('a').length
   ```

3. **Network panel:** Reload the page with the Network panel open. Find the main HTML document request (usually the first one). Record:
   - The HTTP status code
   - The response time in milliseconds
   - The size of the response

4. **Lighthouse:** Run a Lighthouse audit on a site of your choice. Record the scores for Performance and Accessibility. Note one specific recommendation it makes.
