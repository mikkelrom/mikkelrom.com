---
layout: layouts/post.njk
title: The levels of caching
date: 2020-07-31T09:02:28.278Z
tags:
  - caching
---
The different levels of caching - from client to server.

It's essential in web development to be able to save state, so that a user can return to the same place in the checkout-process or so that it's possible to share specific content with a friend or on social media.

Columns: 

* Caching mechanism
* Persistence
* Docs



CLIENT

* A JavaScript variable, scoped or global. CSS variables aka. Custom Properties. This lives in memory, and will be cleared when you refresh the page.
* Local-storage
* Session-storage
* Cookies
* [History.pushState()](https://developer.mozilla.org/en-US/docs/Web/API/History/pushState)
* IndexedDB - <https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API>



CLIENT-PROXY

* ServiceWorkers - CacheStorage API: <https://developer.mozilla.org/en-US/docs/Web/API/CacheStorage>



BROWSER

* Built-in browser cache (HTTP cache) that we as developers can't control directly.



EDGE/CDN

* Edge-workers - logic on the CDN
* CDN cache



SERVER

* Memory cache. Just like the JS variable on the client, just on the server instead.
* Filesystem
* Database



What about Redux, NGRX, VueX or other JavaScript store-libraries for state mangement? These all live in the "JS variable" category.

Link: <https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Client-side_storage>