---
title: Day 17 — Open-Source Software and CMS
parent: Week 3 — Scripting and Frameworks
nav_order: 5
---

# Day 17 — Open-Source Software and Content Management Systems

**Syllabus:** Explain the use and development of open-source software in relation to web development; research, experiment with and evaluate the prevalence and use of web content management systems (CMS)
**Outcomes:** SE-12-04
**Textbook:** Section 12.6

---

## Key Terms

**Open-source software (OSS)** — Software whose source code is publicly available. Anyone can view, modify, and distribute it under the terms of its licence.

**Proprietary software** — Software whose source code is not publicly available. Owned and controlled by a company. Example: Adobe Photoshop.

**Licence** — Legal terms under which software can be used, modified, and distributed. Common OSS licences: MIT, Apache 2.0, GPL.

**Repository** — A storage location for source code, typically version controlled with Git. Example: GitHub repository.

**Fork** — Creating a personal copy of someone else's repository to modify independently.

**CMS (Content Management System)** — Software that allows users to create, manage, and publish digital content without writing code. Examples: WordPress, Drupal, Joomla.

**Headless CMS** — A CMS that manages content without a built-in front end. Content is delivered via API and displayed by a separately built front end. Examples: Contentful, Strapi.

---

## Theory

### Open-Source Software in Web Development

The web runs largely on open-source software. Nearly every tool used in this course is open-source:

| Tool | Licence | What it does |
|---|---|---|
| Python | PSF Licence | Programming language |
| Flask | BSD | Web framework |
| Linux | GPL | Server operating system |
| Git | GPL | Version control |
| Bootstrap | MIT | CSS framework |
| SQLite | Public domain | Database engine |

**Why open-source dominates web development:**
- Free to use — reduces development cost
- Transparent — security vulnerabilities can be found and fixed by anyone
- Community-driven — bugs fixed and features added by contributors worldwide
- No vendor lock-in — not dependent on one company's decisions

### Open-Source Licences

Licences define what you can and cannot do with open-source code:

| Licence | Can use commercially | Must share modifications | Must credit author |
|---|---|---|---|
| MIT | Yes | No | Yes |
| Apache 2.0 | Yes | No | Yes |
| GPL | Yes | Yes (under GPL) | Yes |
| BSD | Yes | No | Yes |

**MIT** is the most permissive and most common in web development. Flask, Bootstrap, and React all use MIT.

**GPL** is "copyleft" — if you modify GPL code and distribute it, your modifications must also be open-source under GPL.

### Contributing to Open Source

Open-source projects on GitHub accept contributions via **pull requests**:

1. Fork the repository
2. Make changes in your fork
3. Submit a pull request — proposing your changes to the original project
4. Maintainers review and (if accepted) merge the changes

This model is how Flask, Bootstrap, and thousands of other tools are maintained by volunteer communities.

### Content Management Systems (CMS)

A CMS allows non-technical users to manage website content through a web interface — no coding required.

**Traditional CMS (coupled):** The CMS manages both content and presentation.

**WordPress** — powers approximately 43% of all websites on the internet. Open-source, written in PHP.

Features:
- Admin dashboard for creating and editing pages and posts
- Themes control appearance (thousands available, free and paid)
- Plugins extend functionality (e.g. contact forms, SEO tools, e-commerce)

**Headless CMS:** Content is stored and managed in the CMS but delivered via API to any front end (website, mobile app, etc.).

### CMS Comparison

| | WordPress | Drupal | Squarespace | Headless CMS |
|---|---|---|---|---|
| Technical skill needed | Low–Medium | High | Very low | High |
| Customisation | High | Very high | Low | Very high |
| Cost | Free (hosting extra) | Free (hosting extra) | Subscription | Varies |
| Open-source | Yes | Yes | No | Varies |
| Best for | Blogs, small business | Enterprise, government | Simple sites | Complex multi-platform |

### When to Use a CMS vs Custom Development

**Use a CMS when:**
- Content is updated frequently by non-technical staff
- Standard features (blog, pages, forms) are sufficient
- Time to market is critical
- Budget is limited

**Use custom development when:**
- Complex, unique business logic is required
- Tight performance requirements
- Integration with specific systems is needed
- Full control over the technology stack is required

### Open-Source Security Considerations

Using open-source software introduces responsibilities:
- **Keep dependencies updated** — security vulnerabilities in libraries are publicly known and actively exploited
- **Check licences** — commercial projects must comply with OSS licence terms
- **Audit dependencies** — tools like `pip audit` check for known vulnerabilities in Python packages

```bash
# Check for vulnerabilities in installed Python packages
pip audit

# List all installed packages and versions
pip list

# Check a specific package
pip show flask
```

---

## Objective Response Questions

**1.** A developer uses a library licensed under the GPL in their commercial web application and distributes the application to customers. What obligation does the GPL licence impose?

- A) They must pay a licensing fee to the original authors
- B) They must make their application's source code available under the GPL
- C) They cannot use GPL software in commercial products
- D) They must credit the original authors in the application's UI

**Answer: B** — GPL is copyleft: distributing software that includes GPL code requires the entire distributed work to be available under GPL.

---

**2.** A school wants to build a website where teachers can update news and events without any coding knowledge. Which solution is most appropriate?

- A) A static HTML site updated manually by a developer
- B) A content management system with an admin dashboard
- C) A headless CMS delivering content via API
- D) A Flask application with a Jinja2 template

**Answer: B** — A traditional CMS provides a non-technical interface for managing content.

---

**3.** Which of the following best describes the difference between a traditional CMS and a headless CMS?

- A) A headless CMS has no admin interface
- B) A traditional CMS is always open-source; a headless CMS is always proprietary
- C) A headless CMS delivers content via API, allowing any front end to consume it; a traditional CMS manages both content and presentation
- D) A headless CMS cannot store images or media files

**Answer: C**

---

## Try It

1. Go to [wordpress.com](https://wordpress.com) and create a free account and site. Explore the dashboard — create one page and one post.
2. Find the Flask repository on GitHub (`github.com/pallets/flask`). Record:
   - How many stars it has
   - What licence it uses
   - When the last commit was made
   - How many open issues exist
3. Run `pip list` in your Codespaces terminal. Identify three packages and find their licences on PyPI (`pypi.org`).
4. Write a 150-word evaluation: for a school sports team website updated weekly by a non-technical coach, would you recommend WordPress or a custom Flask application? Justify your answer referencing at least two criteria (cost, ease of use, customisation, maintenance).
