---
title: Day 1 — History of the Web
parent: Week 1 — Web Concepts and Data Transmission
nav_order: 1
---

# Day 1 — History of the Web

**Syllabus:** Explore the applications of web programming — interactive websites, e-commerce, progressive web apps (PWAs)
**Outcomes:** SE-12-03, SE-12-04
**Textbook:** Section 11.1

---

## Key Terms

**Internet** — The global network of interconnected computers that communicate using standardised protocols. The internet is the infrastructure — the physical cables, routers, and wireless connections.

**World Wide Web (Web)** — A system of interlinked documents and resources accessed via the internet using a browser. The web is a *service* that runs *on top of* the internet. You can use the internet without using the web (e.g. email, FTP).

**Web application** — Software that runs in a web browser, delivering functionality over the internet rather than being installed locally. Examples include Google Docs, online banking, and social media platforms.

**Static website** — A site where every page is a fixed HTML file. Content does not change unless a developer manually edits the file. Example: an early 1990s homepage.

**Dynamic website** — A site where content is generated on-the-fly, often from a database, in response to user requests. Example: a news site that pulls the latest articles each time a page loads.

---

## Theory

### A Brief History

The web was invented by **Tim Berners-Lee** in 1989 while working at CERN. He proposed a system of linked documents to help scientists share information. The first website went live in 1991.

Key milestones:

| Year | Event |
|---|---|
| 1989 | Tim Berners-Lee proposes the World Wide Web |
| 1991 | First website published at CERN |
| 1993 | Mosaic browser released — first to display images inline with text |
| 1995 | JavaScript invented by Brendan Eich at Netscape in 10 days |
| 1996 | CSS introduced — separating style from content |
| 2004 | Web 2.0 era — user-generated content, social media, AJAX |
| 2007 | iPhone launches — mobile web becomes critical |
| 2015 | Progressive Web Apps introduced by Google |

### Internet vs Web

A common source of confusion in exam responses:

- The **internet** is the network (infrastructure)
- The **web** is a service that uses the internet
- Email, online gaming, and video calls also use the internet but are not the web

Think of it this way: the internet is the road network; the web is one type of vehicle using those roads.

---

## Types of Web Applications

### Interactive Websites

An interactive website responds dynamically to user input without requiring a full page reload. Features include forms, live search, comment sections, and real-time updates.

**Example:** A news website that loads new articles as you scroll, or a flight search that updates prices as you change dates.

Key characteristics:
- Content changes based on user actions
- Uses JavaScript to manipulate the page without reloading
- May use AJAX to fetch data from a server in the background

### E-Commerce Applications

E-commerce platforms handle the buying and selling of goods and services online. They require additional complexity compared to standard websites.

**Example:** An online bookstore where users can browse products, add to a cart, enter payment details, and receive order confirmation.

Requirements that set e-commerce apart:
- Secure payment processing (must encrypt financial data)
- Inventory management (stock levels update in real time)
- User accounts and order history
- Compliance with payment security standards (PCI DSS)

### Progressive Web Apps (PWAs)

A PWA is a web application that behaves like a native mobile app. It can be installed on a device's home screen, works offline, and can send push notifications.

**Example:** Twitter Lite — a PWA version of Twitter that loads fast on slow connections and works offline.

PWA key features:
- **Offline capability** — stores data locally using a service worker so the app works without internet
- **Installable** — users can add it to their home screen like a native app
- **Push notifications** — can alert users even when the browser is closed
- **Responsive** — adapts to any screen size

| Feature | Traditional Website | PWA |
|---|---|---|
| Works offline | No | Yes |
| Installable | No | Yes |
| Push notifications | No | Yes |
| App store required | No | No |
| Updated instantly | Yes | Yes |

---

## Objective Response Questions

**1.** Which of the following best describes the difference between the internet and the World Wide Web?

- A) They are the same thing, used interchangeably
- B) The internet is a service that runs on top of the web
- C) The web is a service that runs on top of the internet
- D) The web only works on mobile devices

**Answer: C**

---

**2.** A developer is building an app for field scientists who need to collect data in remote areas with no mobile coverage. Which type of web application is most appropriate?

- A) Static website
- B) E-commerce platform
- C) Progressive Web App
- D) Interactive website with AJAX

**Answer: C** — PWAs support offline functionality via service workers, making them suitable for environments with no connectivity.

---

**3.** Which of the following is NOT a characteristic of a Progressive Web App?

- A) Can be installed on a home screen
- B) Requires an app store to distribute
- C) Can work without an internet connection
- D) Can send push notifications

**Answer: B** — PWAs are distributed via the web, not through an app store.

---

## Try It

1. Open your browser and go to [web.dev/explore/progressive-web-apps](https://web.dev/explore/progressive-web-apps). Read the overview.
2. Find one example of a real-world PWA and note: what problem does it solve? Why is offline capability useful for that specific application?
3. In your own words, write two or three sentences explaining the difference between the internet and the web. Avoid copying definitions — use an analogy of your own.
