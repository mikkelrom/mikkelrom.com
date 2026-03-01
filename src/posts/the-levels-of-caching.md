---
layout: layouts/post.njk
title: The levels of caching
date: 2026-03-01T09:00:00.000Z
tags:
  - caching
---
Let's say you are building a website/webapp and suddenly you need to persist some information. That could be anything really, like "this user is logged in" or "this user has signed up for the newsletter". It could even be something more UI-related like "the user has chosen to see the condensed product-list view, instead of the default gallery-view". All this information needs to be persisted, or in other words, cached.
There are many ways to do this, and each method has its own pros and cons. I thought it would be fun to try and map those out, and hopefully it can be helpful to others as well.
Alright, let's go!

<a href="/images/caching-layers.svg" target="_blank" style="border-bottom:none;display:block;">
  <img src="/images/caching-layers.svg" alt="The levels of caching — from client to server" width="1380" height="908" style="width:100%;height:auto;">
</a>

**Overview**

Client
- [Runtime variables](#heading-runtime-variables)
- [History state](#heading-history-state)
- [Session storage](#heading-session-storage)
- [Local storage](#heading-local-storage)
- [Cookies](#heading-cookies)
- [IndexedDB](#heading-indexeddb)

Client-proxy
- [Service Workers (CacheStorage API)](#heading-service-workers-(cachestorage-api))

Browser
- [HTTP cache](#heading-http-cache)

Edge / CDN
- [CDN cache](#heading-cdn-cache)

Server
- [Memory cache](#heading-memory-cache)
- [Filesystem](#heading-filesystem)
- [Database](#heading-database)

# Client

Let's start with the caching technologies that are available on the client.

## Runtime variables

* A **JavaScript variable**, either scoped to a specific context or global, is a runtime variable. You can store data in a variable in runtime, for example when a user clicks a button, but when you reload the page, that data will be gone.
* **CSS variables aka. Custom Properties** is also a runtime variable.

### Persistence

This lives in memory, and will be cleared (or reset to its initial state) when you reload the page.

### Use-case

A common scenario for this could be to save whether the user has opened the burger-menu or not. It could also be used to save whether the user is currently hovering a specific element or not. These types of information don't need to survive a page-refresh.

### Example

```javascript
// JavaScript
let color = 'red';
```

```css
/* CSS */
:root {
    --color: red;
}
```

### Read more

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)

> **Note:** What about Redux, NGRX, VueX, Zustand, or other JavaScript store-libraries for state management? These all fall under "Runtime variables" — they store state in JavaScript memory, so the data is gone when you reload the page.

## History state

You might not think of the browser's history as a cache, but `history.pushState()` lets you attach a state object to a history entry. This is useful for storing UI state that should survive back/forward navigation.

### Persistence

The state object lives in the browser's session history. It survives back/forward navigation but is lost on page refresh or when the tab is closed. However, the URL itself (including query parameters) does survive a refresh — so you get the best of both worlds by combining `pushState` with URL search params.

### Use-case

This is great for storing UI state tied to navigation — like search filters, pagination position, or scroll position. When the user hits "back", the state is still there, so you can restore the page exactly as they left it.

### Example

```javascript
// JavaScript
// Save state when the user applies a filter
history.pushState({ filter: 'recent', page: 2 }, '', '?filter=recent&page=2');

// Restore state when the user navigates back
window.addEventListener('popstate', (event) => {
  if (event.state) {
    applyFilter(event.state.filter);
    goToPage(event.state.page);
  }
});
```

### Read more
[https://developer.mozilla.org/en-US/docs/Web/API/History/pushState](https://developer.mozilla.org/en-US/docs/Web/API/History/pushState)


## Session-storage

Session-storage is great for storing small amounts of data on the client, that is only relevant in the current session.

### Persistence

Session-storage survives a page-refresh. It is cleared when the session ends, which is when the tab/window is closed.

### Use-case

This could be used for a feature that autosaves the contents of a text-field, since this data is specific to the current session.

### Example

```javascript
// JavaScript
sessionStorage.setItem('key', 'value');

sessionStorage.getItem('key'); // "value"
```

### Read more
[https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage)

## Local-storage

Local-storage is great for storing small amounts of data on the client.

### Persistence

The data is persisted, so it can "survive" a page-refresh. It will be deleted when the user chooses to ["Clear browsing data"](https://support.google.com/accounts/answer/32050). It can also be deleted programmatically in JS or manually through browser devtools.

### Use-case

This could be used for saving an anonymized user preference such as "gallery list view".

### Example

```javascript
// JavaScript
localStorage.setItem('view-mode', 'gallery');

localStorage.getItem('view-mode'); // "gallery"
```

### Read more

[https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)


## Cookies
Cookies are kind of old-school, but they actually have a super-power that for example local storage doesn't have: Cookies are sent from the client to the server, so a value written on client-side can be read on server-side and vice versa, which can be very useful.

### Persistence

A cookie can be a "session cookie". Session cookies are only persisted in the current session, similar to session storage.
If a cookie is not a session cookie, then it is persisted, so it can "survive" a page-refresh. It will be deleted when the user chooses to ["Clear browsing data"](https://support.google.com/accounts/answer/32050). It can also be deleted programmatically in JS or manually through browser devtools.
A cookie can also have an optional `expirationDate` set, which will automatically invalidate the cookie at that time.

### Use-case

A cookie has been used for many years to save whether the user is logged in or not. This has also shown to be risky though, since cookies can be stolen. A cookie could also be used for saving the view-mode, or other user-preferences.

### Example

```javascript
// JavaScript
document.cookie = 'key=value';

document.cookie; // "key=value"
```

### Read more
[https://developer.mozilla.org/en-US/docs/Web/API/document/cookie](https://developer.mozilla.org/en-US/docs/Web/API/document/cookie)


## IndexedDB

IndexedDB is a full-blown database that runs in the browser. Unlike localStorage, it can handle large amounts of structured data — think megabytes rather than a few kilobytes. It's asynchronous too, so it won't block the main thread.

### Persistence

The data is persisted across sessions and survives page refreshes. Like localStorage, it will be deleted when the user clears browsing data. Storage capacity is much larger though — browsers typically allow hundreds of megabytes or more.

### Use-case

This is your go-to when you need to store large or complex data on the client. Common examples include caching API responses for offline use, storing files or blobs, or keeping a local copy of data that syncs with the server.

### Example

```javascript
// JavaScript
const request = indexedDB.open('myDatabase', 1);

request.onupgradeneeded = (event) => {
  const db = event.target.result;
  db.createObjectStore('items', { keyPath: 'id' });
};

request.onsuccess = (event) => {
  const db = event.target.result;
  const tx = db.transaction('items', 'readwrite');
  tx.objectStore('items').add({ id: 1, name: 'cached item' });
};
```

### Read more
[https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)


# Client-proxy

Between the client and the network sits a layer that can intercept and cache requests before they ever hit the server.

## Service Workers (CacheStorage API)

A Service Worker is a script that runs in the background, separate from your web page. It can intercept network requests and serve cached responses — which means your site can work offline or load instantly on repeat visits.

### Persistence

Cached data is persisted until you explicitly delete it or the Service Worker is unregistered. It survives page refreshes, tab closures, and even browser restarts.

### Use-case

This is essential for building offline-capable web apps (PWAs). It's also great for caching static assets like HTML, CSS, and images so they load instantly on repeat visits, without hitting the network at all.

### Example

```javascript
// In your Service Worker (sw.js)
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then((cached) => {
      // Return cached response, or fetch from network and cache it
      return cached || fetch(event.request).then((response) => {
        return caches.open('my-cache').then((cache) => {
          cache.put(event.request, response.clone());
          return response;
        });
      });
    })
  );
});
```

### Read more
[https://developer.mozilla.org/en-US/docs/Web/API/CacheStorage](https://developer.mozilla.org/en-US/docs/Web/API/CacheStorage)


# Browser

The browser itself has a built-in caching layer that we as developers don't control directly through JavaScript — instead, we control it through HTTP headers.

## HTTP cache

Every browser has a built-in HTTP cache. When you request a resource (a CSS file, an image, a script), the browser checks its cache first. If the resource is there and still "fresh", the browser skips the network request entirely. This is the caching most people never think about, because it just works.

### Persistence

Controlled by HTTP headers sent from the server. `Cache-Control` sets how long a resource is considered fresh. `ETag` and `Last-Modified` allow the browser to ask the server "has this changed?" before re-downloading. The browser manages the actual storage and eviction.

### Use-case

This is how static assets like CSS, JavaScript, images, and fonts get cached automatically. You don't write any client-side code for this — you configure it on your server or CDN.

### Example

```
# HTTP response headers
Cache-Control: public, max-age=31536000, immutable
ETag: "abc123"

# On subsequent requests, the browser sends:
If-None-Match: "abc123"
# Server responds with 304 Not Modified if unchanged
```

### Read more
[https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

> **Note:** Browsers also have a **Back/Forward Cache (bfcache)** that stores a complete snapshot of the page in memory when you navigate away. When the user hits "back", the browser restores the snapshot instantly instead of re-fetching and re-rendering. This is fully automatic — you can't opt in — but you can accidentally break it with things like `unload` event listeners or `Cache-Control: no-store`. If you've ever wondered why your page looks stale after pressing "back", bfcache is likely the reason. [Read more on web.dev](https://web.dev/articles/bfcache).


# Edge / CDN

Moving further away from the client, we reach the edge — servers distributed around the world that sit between the user and your origin server.

## CDN cache

A CDN (Content Delivery Network) caches your content on servers geographically close to your users. When someone in Tokyo requests your site, they get the response from a nearby edge server instead of your origin server in Europe. This dramatically reduces latency.

### Persistence

Controlled by cache headers (`Cache-Control`, `Surrogate-Control`) and CDN-specific configuration. Cached content can also be purged manually or programmatically when you deploy new versions.

### Use-case

This is ideal for serving static assets (JS, CSS, images, fonts) and even full HTML pages from edge locations. If your content doesn't change on every request, a CDN cache can save your origin server from a ton of traffic and give users a much faster experience.

### Example

```
# HTTP response headers for CDN caching
Cache-Control: public, max-age=86400, s-maxage=604800
Surrogate-Control: max-age=604800

# s-maxage targets shared caches (like CDNs) specifically.
# The CDN caches for 7 days, while the browser caches for 1 day.
```

### Read more
[https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching#shared_caches](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching#shared_caches)


# Server

Finally, we reach the server. Caching on the server is all about avoiding redundant work — whether that's re-computing expensive results, re-reading files, or re-querying the database.

## Memory cache

Just like a JavaScript variable on the client, a server can store data in memory. This is the fastest type of server-side cache because there's no I/O involved — the data is right there in the process's memory.

### Persistence

Lives in the process memory. The data is lost when the server restarts or the process crashes. If you're running multiple instances, each one has its own separate memory cache.

### Use-case

This is great for hot data that's expensive to compute or fetch — things like parsed configuration, compiled templates, or frequently accessed database results. A common pattern is to check the memory cache first, and only hit the database if the data isn't there.

### Example

```javascript
// Node.js
const cache = new Map();

function getUser(id) {
  if (cache.has(id)) {
    return cache.get(id);
  }
  const user = db.query('SELECT * FROM users WHERE id = ?', [id]);
  cache.set(id, user);
  return user;
}
```

### Read more
[https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Server-side/First_steps/Introduction](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Server-side/First_steps/Introduction)

> **Note:** Tools like [Redis](https://redis.io/) and [Memcached](https://memcached.org/) also fall under memory cache, but they run as separate services. This means they can be shared across multiple server instances and can optionally persist data to disk. If you've outgrown a simple in-process Map, these are the typical next step.


## Filesystem

The filesystem is a simple and reliable way to cache data on the server. Write something to disk, read it back later. It's slower than memory but survives restarts.

### Persistence

Persisted to disk, so it survives server restarts and process crashes. The data stays until it's explicitly deleted or the disk is wiped.

### Use-case

This is commonly used for build artifacts, generated files (like resized images or compiled templates), and file-based caching strategies. Many frameworks use the filesystem to cache rendered pages as static HTML files.

### Example

```javascript
// Node.js
const fs = require('fs');
const path = require('path');

const cachePath = path.join(__dirname, '.cache', 'data.json');

// Write to cache
fs.writeFileSync(cachePath, JSON.stringify({ key: 'value' }));

// Read from cache
const data = JSON.parse(fs.readFileSync(cachePath, 'utf-8'));
```

### Read more
[https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction)


## Database

At the bottom of the stack sits the database. It's the slowest cache on this list, but it's also the most durable and the most powerful. If you need data to survive restarts, scale across multiple servers, and be queryable in complex ways — this is where it goes.

### Persistence

Fully persisted to disk (and often replicated across multiple machines). Survives restarts, crashes, and deployments. The data lives until it's explicitly deleted.

### Use-case

This is where you store your core application data — users, orders, posts, settings. Anything that needs to be permanent and shared across all server instances. It's also used for caching computed results that are expensive to regenerate, like aggregated analytics or search indexes.

### Example

```sql
-- SQL
INSERT INTO cache (key, value, expires_at)
VALUES ('homepage_stats', '{"visits": 1234}', '2024-01-02 00:00:00');

SELECT value FROM cache
WHERE key = 'homepage_stats' AND expires_at > NOW();
```

### Read more
[https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Server-side/First_steps/Introduction](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Server-side/First_steps/Introduction)

# Wrapping up
Phew, that was all of them 😅 If you think I have missed something in this list, let me know. Otherwise I hope you enjoyed it and thanks for reading this far.
