---
title: Day 29 — PWA UI/UX and Accessibility
parent: Week 5 — Security, PWA and Assessment
nav_order: 5
---

# Day 29 — PWA: UI/UX, Accessibility and Version Control

**Syllabus:** UI/UX principles — font, colour, audio, video, navigation; accessibility and inclusivity; version control
**Outcomes:** SE-12-02, SE-12-06
**Textbook:** Section 13.3 + 12.1

---

## Key Terms

**UI (User Interface)** — The visual and interactive elements a user sees and uses. Concerned with appearance.

**UX (User Experience)** — The overall experience of using a product. Concerned with usability, efficiency, and satisfaction.

**Accessibility** — Designing so people with disabilities can use the application effectively.

**ARIA (Accessible Rich Internet Applications)** — A set of HTML attributes that enhance accessibility for dynamic content.

**Version control** — A system for tracking changes to code over time. Enables rollback, collaboration, and history.

**Git** — The most widely used version control system. Tracks changes to files in a repository.

**Branch** — A parallel version of the codebase. Used to develop features without affecting the main code.

**Commit** — A saved snapshot of changes with a descriptive message.

**Pull request** — A proposal to merge changes from one branch into another, typically reviewed by teammates.

---

## Theory

### UI/UX Principles for PWAs

PWAs must deliver a native app-like experience. Key principles:

**Typography:**
- Use a legible base font size (minimum 16px body text)
- Limit to 2 font families maximum
- Maintain sufficient line height (1.4–1.6) for readability
- Ensure adequate contrast between text and background

```css
:root {
    --font-base: 'Segoe UI', system-ui, sans-serif;
    --font-size-base: 16px;
    --line-height: 1.5;
    --colour-text: #1a1a1a;
    --colour-bg: #ffffff;
    --colour-primary: #0077cc;
}

body {
    font-family: var(--font-base);
    font-size: var(--font-size-base);
    line-height: var(--line-height);
    color: var(--colour-text);
    background: var(--colour-bg);
}
```

**Colour:**
- WCAG AA requires contrast ratio of at least **4.5:1** for normal text
- Never use colour as the only way to convey information (colour-blind users)
- Provide a focus indicator for keyboard navigation

```css
/* Visible focus indicator — do not remove outline */
:focus-visible {
    outline: 3px solid var(--colour-primary);
    outline-offset: 2px;
}

/* Colour contrast example */
/* White (#fff) on navy (#003366) = 12.6:1 — excellent */
/* Light grey (#999) on white (#fff) = 2.85:1 — fails WCAG AA */
```

Check contrast at [webaim.org/resources/contrastchecker](https://webaim.org/resources/contrastchecker)

**Navigation:**
- Consistent placement across all pages
- Current page indicated visually
- All navigation accessible via keyboard (Tab key)
- Skip navigation link for screen reader users

```html
<!-- Skip link — visually hidden but accessible to keyboard/screen reader -->
<a href="#main-content" class="skip-link">Skip to main content</a>

<nav aria-label="Main navigation">
    <a href="/" aria-current="page">Home</a>
    <a href="/about">About</a>
</nav>

<main id="main-content">
    <!-- page content -->
</main>
```

```css
.skip-link {
    position: absolute;
    top: -40px;
    left: 0;
}
.skip-link:focus {
    top: 0;
}
```

**Audio and Video:**
- All videos must have captions
- Audio must have a transcript
- No autoplay audio
- Provide controls for all media

```html
<video controls>
    <source src="demo.mp4" type="video/mp4">
    <track kind="subtitles" src="demo.vtt" srclang="en" label="English" default>
    Your browser does not support video.
</video>
```

### Accessibility in PWAs

PWAs have additional accessibility considerations:

**Dynamic content updates** — when content changes without a page reload, screen readers need to be notified:

```html
<!-- Live region — screen reader announces changes -->
<div aria-live="polite" id="status-message"></div>
```

```javascript
document.getElementById('status-message').textContent = 'Form submitted successfully';
```

**Touch targets** — minimum 44×44px for interactive elements on mobile:

```css
button, a, input[type="checkbox"] {
    min-height: 44px;
    min-width: 44px;
    padding: 12px;
}
```

**Keyboard navigation** — all interactive elements reachable via Tab, activated via Enter/Space:

```javascript
// Custom interactive element — handle keyboard events
element.addEventListener('keydown', (e) => {
    if (e.key === 'Enter' || e.key === ' ') {
        e.preventDefault();
        handleAction();
    }
});
```

### Version Control with Git

Version control is essential for any software project:

**Core workflow:**

```bash
# Check status — what has changed?
git status

# Stage changes
git add .                         # stage all changes
git add app.py templates/         # stage specific files

# Commit with a meaningful message
git commit -m "Add CSRF protection to login form"

# Push to GitHub
git push origin main
```

**Meaningful commit messages:**

```bash
# ❌ Bad
git commit -m "fix"
git commit -m "changes"
git commit -m "wip"

# ✅ Good
git commit -m "Fix SQL injection in student search route"
git commit -m "Add service worker for offline support"
git commit -m "Implement WCAG AA colour contrast on navigation"
```

**Branching for features:**

```bash
# Create and switch to a new branch
git checkout -b feature/pwa-offline

# Work on the feature...
git add .
git commit -m "Add service worker and manifest for PWA"

# Push branch to GitHub
git push origin feature/pwa-offline

# On GitHub: create a pull request to merge into main
```

**Reverting mistakes:**

```bash
# View commit history
git log --oneline

# Undo last commit (keep changes staged)
git reset --soft HEAD~1

# Discard all uncommitted changes (careful — cannot undo)
git checkout -- .

# Revert a specific commit (creates a new undo commit)
git revert abc1234
```

**Why version control matters:**
- **Safety net** — mistakes can be undone
- **History** — understand why a change was made
- **Collaboration** — multiple developers work without overwriting each other
- **Deployment** — production code is tagged and traceable
- **Assessment** — demonstrates a professional development process

---

## Objective Response Questions

**1.** A PWA displays error messages using red text only, with no other indicator. A colour-blind user may not notice the error. Which design principle does this violate?

- A) Principle of least privilege
- B) WCAG requirement that colour is not the sole means of conveying information
- C) The mobile-first design principle
- D) The cache-first service worker strategy

**Answer: B** — Colour alone cannot be the only indicator. Add an icon, bold text, or a border to convey the same information non-visually.

---

**2.** A developer makes 10 changes over 2 hours and commits them all at once with the message "done". What is the problem with this approach?

- A) Git does not allow commits with more than 5 changed files
- B) The commit history provides no useful information about what changed or why, making debugging and rollback difficult
- C) GitHub will reject the push due to the large diff
- D) The commit message must be at least 20 characters

**Answer: B** — Frequent, focused commits with descriptive messages make it possible to identify exactly when and why a bug was introduced, and to revert specific changes.

---

**3.** A keyboard user presses Tab to navigate a web page but the currently focused element has no visible indicator. Which CSS issue causes this?

- A) Missing `aria-label` attributes
- B) `outline: none` applied to `:focus` — removing the browser's default focus ring
- C) The element does not have `tabindex="0"`
- D) JavaScript is blocking keyboard events

**Answer: B** — Removing the focus outline (`outline: none`) is a common mistake that breaks keyboard navigation for sighted keyboard users.

---

## Try It

1. Run a WAVE accessibility audit on your app ([wave.webaim.org](https://wave.webaim.org)) — fix at least two issues
2. Check all colour contrast ratios using the WebAIM contrast checker — fix any that fail WCAG AA
3. Add a skip navigation link to your `base.html`
4. Add `aria-current="page"` to the active nav link (use Jinja2 to detect the current route)
5. Navigate your entire app using only the keyboard (Tab, Enter, Space) — document any elements you cannot reach
6. Review your Git commit history (`git log --oneline`) — if you have any vague messages, document what each commit should have been called
