---
title: Day 28 — PWA Manifest and Service Workers
parent: Week 5 — Security, PWA and Assessment
nav_order: 4
---

# Day 28 — PWA: Manifest and Service Workers

**Syllabus:** Design, develop and implement a progressive web app (PWA) — offline capability, service workers
**Outcomes:** SE-12-02
**Textbook:** Section 13.3

---

## Key Terms

**PWA (Progressive Web App)** — A web application that uses modern browser features to deliver app-like experiences including offline use, installation, and push notifications.

**Web App Manifest** — A JSON file that describes the PWA to the browser — name, icons, colours, display mode. Required for installation.

**Service Worker** — A JavaScript file that runs in the background, separate from the main page. Intercepts network requests, enabling offline capability and caching.

**Cache API** — A browser storage mechanism used by service workers to store request/response pairs for offline use.

**Install prompt** — The browser's prompt asking the user if they want to install the PWA to their home screen.

**Offline-first** — A design approach where the application works fully offline and syncs when connectivity returns.

---

## Theory

### PWA Requirements

For a web app to be installable as a PWA it needs:
1. Served over **HTTPS** (or localhost for development)
2. A valid **Web App Manifest**
3. A registered **Service Worker**

### Web App Manifest

Create `static/manifest.json`:

```json
{
  "name": "HSC Study App",
  "short_name": "StudyApp",
  "description": "A study tool for HSC Software Engineering",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#0077cc",
  "icons": [
    {
      "src": "/static/icons/icon-192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/static/icons/icon-512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

Link in your base template `<head>`:

```html
<link rel="manifest" href="/static/manifest.json">
<meta name="theme-color" content="#0077cc">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-title" content="StudyApp">
```

Serve from Flask:

```python
@app.route('/static/manifest.json')
def manifest():
    return send_from_directory('static', 'manifest.json',
                               mimetype='application/manifest+json')
```

### Service Worker

Create `static/sw.js`:

```javascript
const CACHE_NAME = 'study-app-v1';

// Files to cache immediately on install
const PRECACHE_URLS = [
  '/',
  '/static/css/style.css',
  '/static/js/script.js',
  '/static/manifest.json',
  '/static/icons/icon-192.png'
];

// Install event — cache core files
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then((cache) => cache.addAll(PRECACHE_URLS))
      .then(() => self.skipWaiting())
  );
});

// Activate event — remove old caches
self.addEventListener('activate', (event) => {
  event.waitUntil(
    caches.keys().then((cacheNames) => {
      return Promise.all(
        cacheNames
          .filter((name) => name !== CACHE_NAME)
          .map((name) => caches.delete(name))
      );
    })
  );
});

// Fetch event — serve from cache, fall back to network
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request)
      .then((cached) => {
        if (cached) return cached;           // serve from cache

        return fetch(event.request)          // fetch from network
          .then((response) => {
            // Cache successful GET responses
            if (event.request.method === 'GET' && response.status === 200) {
              const copy = response.clone();
              caches.open(CACHE_NAME)
                .then((cache) => cache.put(event.request, copy));
            }
            return response;
          })
          .catch(() => {
            // Network failed — return offline page if available
            if (event.request.destination === 'document') {
              return caches.match('/offline.html');
            }
          });
      })
  );
});
```

### Registering the Service Worker

Add to your main JavaScript file or a `<script>` in `base.html`:

```javascript
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('/static/sw.js')
      .then((reg) => console.log('SW registered:', reg.scope))
      .catch((err) => console.error('SW failed:', err));
  });
}
```

Serve the service worker from Flask — it must be at the root scope:

```python
@app.route('/sw.js')
def service_worker():
    return send_from_directory('static', 'sw.js',
                               mimetype='application/javascript')
```

### Offline Page

Create `templates/offline.html` and a route:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Offline</title>
</head>
<body>
    <h1>You're offline</h1>
    <p>Please check your connection and try again.</p>
</body>
</html>
```

```python
@app.route('/offline.html')
def offline():
    return render_template('offline.html')
```

### Caching Strategies

| Strategy | How it works | Best for |
|---|---|---|
| Cache first | Serve from cache; update in background | Static assets (CSS, JS, images) |
| Network first | Try network; fall back to cache | Dynamic content (pages, API data) |
| Stale while revalidate | Serve cache immediately; fetch update | Frequently updated content |
| Cache only | Serve from cache only | Pre-cached app shell |
| Network only | Always fetch from network | Real-time data, POST requests |

The service worker above uses **cache first** for already-cached resources and **network first** for new resources.

### Testing the PWA

In Chrome DevTools:
1. **Application** tab → **Manifest** — verify manifest loads correctly
2. **Application** tab → **Service Workers** — verify SW is registered and active
3. **Application** tab → **Cache Storage** — view cached files
4. **Network** tab → check **Offline** checkbox → reload page — verify offline mode works
5. **Lighthouse** tab → run audit → check PWA score

---

## Objective Response Questions

**1.** A service worker's fetch event handler returns a cached response when available and falls back to the network. This strategy is called:

- A) Network first
- B) Cache only
- C) Cache first
- D) Stale while revalidate

**Answer: C**

---

**2.** A PWA manifest sets `"display": "standalone"`. What does this mean?

- A) The app runs in a separate sandboxed process
- B) The app launches in a window without browser UI (address bar, navigation buttons)
- C) The app can only run when offline
- D) The app is available in the system app store

**Answer: B** — Standalone mode makes the PWA look and feel like a native app — no browser chrome visible.

---

**3.** Why must a service worker script be served from the root path (e.g. `/sw.js`) rather than `/static/sw.js`?

- A) JavaScript files cannot be served from the static directory
- B) A service worker's scope is limited to its own path and below — a worker at `/static/sw.js` could only intercept requests to `/static/`
- C) Flask does not support service workers in subdirectories
- D) The manifest requires the service worker to be at the root

**Answer: B** — Service worker scope is determined by its URL. To intercept all requests (including `/`, `/students`, etc.), the SW must be at `/sw.js`.

---

## Try It

Convert your Flask app to a PWA:

1. Create `static/manifest.json` with name, icons, theme colour, display standalone
2. Create placeholder icons (192×192 and 512×512 PNG — can be simple coloured squares)
3. Create `static/sw.js` with install, activate, and fetch handlers
4. Register the service worker in your base template
5. Create an `offline.html` template
6. Open DevTools → Application → verify manifest and service worker are detected
7. Check Offline in DevTools Network tab, reload — verify the offline page appears
