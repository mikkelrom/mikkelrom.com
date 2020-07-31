---
layout: layouts/post.njk
title: The levels of caching
date: 2020-07-31T09:02:28.278Z
tags:
  - caching
---
Let's say you are building a website/webapp and suddenly you need to persist some information. That could be anything really, like "this user is logged in" or "this user has signed up for the newsletter". It could even be something like "the user has chosen to see the condensed product-list view, instead of the default gallery-view". All these informations needs to be persisted, or cached. There are many ways to do this, and each method has it's own pros and cons. I thought it would be fun to try and map those out, and hopefully it can be helpful to other than myself.

Columns: 

* Caching mechanism
* Persistence
* Docs



# Client
Let's start with the caching technologies that are available on the client.

## Runtime variables
* A **JavaScript variable**, either scoped to a specific context or global, is a runtime variable. You can store data in a variable in runtime, for example when a user clicks a button, but when you reload the page, that data will be gone.
* **CSS variables aka. Custom Properties** is also a runtime variable.

### Persistence
This lives in memory, and will be cleared (or reset to it's initial state) when you reload the page.

### Use-case
A common scenario for this could be to save whether the user has opened the burger-menu or not. It could also be used to save whether the user is currently hovering a specific element or not. These types of informations doesn't need to be "survive" a page-refresh.

### Examples
```javascript
// JavaScript
let color = 'red';
```

```css
/* CSS */
::root {
    --color: red;
}
```


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