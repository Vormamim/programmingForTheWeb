# Day 6 — W3C, WAI and Web Standards

**Syllabus:** Investigate and explain the role of the World Wide Web Consortium (W3C) — Web Accessibility Initiative (WAI), internationalisation, web security, privacy, machine-readable data
**Outcomes:** SE-12-06
**Textbook:** Section 12.1

---

## Key Terms

**W3C (World Wide Web Consortium)** — The international organisation founded by Tim Berners-Lee in 1994 that develops and maintains open web standards. W3C standards ensure the web works consistently across all browsers, devices, and countries.

**Web standard** — A set of formal rules and specifications that define how web technologies should behave. Standards ensure interoperability — that a webpage built to standard works in any browser.

**WAI (Web Accessibility Initiative)** — A W3C initiative that develops guidelines and resources to make the web accessible to people with disabilities.

**WCAG (Web Content Accessibility Guidelines)** — The W3C standard for web accessibility. Currently at version 2.2. Defines four principles: Perceivable, Operable, Understandable, and Robust (POUR).

**Internationalisation (i18n)** — Designing software so it can be adapted for different languages, regions, and cultures without requiring code changes. The "18" in i18n refers to the 18 letters between the i and n in "internationalisation".

**Localisation (l10n)** — The process of actually adapting an internationalised application for a specific locale — translating text, formatting dates/currency, adjusting layout for right-to-left languages.

**Machine-readable data** — Data formatted so computers can automatically parse and process it, without human interpretation. Examples: JSON, XML, RDF.

**JSON (JavaScript Object Notation)** — A lightweight, human-readable data format widely used for exchanging data between web services.

**Privacy by design** — An approach where privacy protections are built into a system from the start rather than added later as an afterthought.

---

## Theory

### The Role of the W3C

Before web standards existed, browser manufacturers (Microsoft, Netscape) implemented features differently. A website that worked in one browser could break completely in another. This was the "browser wars" era of the 1990s.

The W3C solves this by:
- Publishing open, vendor-neutral specifications for HTML, CSS, and web APIs
- Requiring member organisations (Google, Apple, Mozilla, Microsoft) to implement standards consistently
- Maintaining backwards compatibility — old websites should still work

**Key W3C standards:**
- HTML5 — structure of web pages
- CSS3 — styling and layout
- ARIA — accessibility roles for interactive elements
- WebRTC — real-time communication in the browser
- SVG — scalable vector graphics

### Web Accessibility Initiative (WAI)

Approximately 15% of the world's population lives with some form of disability. The WAI ensures the web is usable for everyone, including people who are:
- **Visually impaired** — may use screen readers, braille displays, or high contrast modes
- **Hearing impaired** — require captions for audio and video content
- **Motor impaired** — may use keyboard-only navigation, switch controls, or eye tracking
- **Cognitively impaired** — benefit from plain language, consistent layouts, and clear navigation

**WCAG four principles (POUR):**

| Principle | Meaning | Example |
|---|---|---|
| Perceivable | Content must be presentable to users in ways they can perceive | Alt text on images for screen readers |
| Operable | Interface components must be operable | All functionality available via keyboard |
| Understandable | Content and operation must be understandable | Clear error messages, consistent navigation |
| Robust | Content must be interpreted reliably by assistive technologies | Valid, semantic HTML |

**WCAG conformance levels:**
- **Level A** — minimum accessibility (must do)
- **Level AA** — standard requirement for most government and commercial sites
- **Level AAA** — highest level, not always achievable for all content

In Australia, government websites are legally required to meet WCAG 2.1 Level AA.

**Practical accessibility examples:**

```html
<!-- Bad: image with no alt text -->
<img src="chart.png">

<!-- Good: descriptive alt text for screen readers -->
<img src="chart.png" alt="Bar chart showing sales growth of 40% from 2023 to 2024">

<!-- Bad: button with no label -->
<button>X</button>

<!-- Good: button with accessible label -->
<button aria-label="Close dialog">X</button>
```

### Internationalisation (i18n)

A web application built for one language and region will fail in others if internationalisation is not considered from the start.

**Key i18n considerations:**

**Character encoding:** Use UTF-8. It supports virtually every character in every language — Arabic, Chinese, Hindi, Greek, emoji.

```html
<meta charset="UTF-8">
```

**Text direction:** English reads left-to-right (LTR). Arabic and Hebrew read right-to-left (RTL). CSS supports both:

```css
/* Right-to-left layout for Arabic */
[lang="ar"] {
    direction: rtl;
    text-align: right;
}
```

**Date and number formats:** Formats vary significantly by locale:

| Locale | Date | Number | Currency |
|---|---|---|---|
| Australia | 25/12/2024 | 1,000.50 | $1,000.50 |
| Germany | 25.12.2024 | 1.000,50 | 1.000,50 € |
| USA | 12/25/2024 | 1,000.50 | $1,000.50 |

**Text expansion:** When text is translated, it often gets longer. German text is typically 30% longer than English. Layouts must accommodate this.

### Web Security Standards

The W3C develops security-related specifications including:

- **Content Security Policy (CSP)** — limits which scripts, styles, and resources a page can load, reducing XSS risk
- **Subresource Integrity (SRI)** — verifies that external files (CDN scripts) have not been tampered with
- **HTTPS enforcement** — W3C recommends all web features require HTTPS

### Privacy

W3C develops privacy standards including:

- **Privacy by Design** — build privacy into systems from the start
- **Do Not Track** — a browser header allowing users to opt out of tracking (largely ignored in practice)
- **Web payments** — standards for privacy-preserving payment APIs

Privacy legislation (Australia's Privacy Act, EU's GDPR) sets legal requirements that web applications must meet. Key principles:
- Collect only data that is necessary
- Tell users what data is collected and why
- Allow users to access and delete their data
- Store data securely

### Machine-Readable Data

Machine-readable data allows programs to automatically extract and use information without human intervention.

**JSON** — the dominant format for web APIs:

```json
{
  "student": {
    "name": "Alex Chen",
    "year": 12,
    "subjects": ["Software Engineering", "Maths", "Physics"],
    "enrolled": true
  }
}
```

**XML** — older format, still used in some industries:

```xml
<student>
  <name>Alex Chen</name>
  <year>12</year>
  <subjects>
    <subject>Software Engineering</subject>
    <subject>Maths</subject>
  </subjects>
</student>
```

**Why it matters:** Government data portals, weather APIs, payment gateways, and mapping services all expose data as JSON or XML. Web applications consume this data programmatically — no human reads it directly.

---

## Objective Response Questions

**1.** A government website displays important public health information but has no alt text on its charts and graphs. Which WCAG principle does this violate?

- A) Operable
- B) Understandable
- C) Perceivable
- D) Robust

**Answer: C** — Perceivable means content must be presentable in ways users can perceive. Without alt text, visually impaired users using screen readers cannot access the chart content.

---

**2.** A development team is building a web application that will be used in Australia, Japan, and Saudi Arabia. Which of the following is the most important internationalisation consideration for the Saudi Arabian market?

- A) Using HTTPS for all connections
- B) Supporting right-to-left text direction
- C) Adding alt text to all images
- D) Using JSON for data exchange

**Answer: B** — Arabic is written right-to-left. Without RTL support, the interface will be unusable for Arabic speakers.

---

**3.** Which organisation is responsible for developing and maintaining open web standards such as HTML and CSS?

- A) IETF (Internet Engineering Task Force)
- B) ISO (International Organisation for Standardisation)
- C) W3C (World Wide Web Consortium)
- D) ICANN (Internet Corporation for Assigned Names and Numbers)

**Answer: C**

---

## Try It

1. Go to [wave.webaim.org](https://wave.webaim.org) and enter the URL of any website you use regularly. Record:
   - How many errors were found
   - What the most common accessibility issue is
   - One specific fix that would improve it

2. Open the DevTools console on any webpage and run:
   ```javascript
   document.documentElement.lang
   ```
   This returns the page's declared language. Check three different websites — do they all declare a language?

3. Find a public JSON API (try `https://api.github.com/users/octocat`) and open it in your browser. Identify three pieces of data it returns and explain what each one represents.
