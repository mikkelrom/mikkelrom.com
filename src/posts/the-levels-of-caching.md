---
layout: layouts/post.njk
title: The levels of caching
date: 2020-07-31T09:02:28.278Z
tags:
  - caching
---
The different levels of caching - from client to server

CLIENT

* A JavaScript variable, scoped or global. This lives in memory, and will be cleared when you refresh the page.
* CSS variables aka. Custom Properties
* Local-storage
* Session-storage
* Cookies
* IndexedDB - <https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API>
* CacheStorage API - used in service-workers: <https://developer.mozilla.org/en-US/docs/Web/API/CacheStorage>
*