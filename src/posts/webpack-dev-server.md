---
layout: layouts/post.njk
title: Accessing localhost externally when using webpack-dev-server
date: 2020-06-11T19:37:37.642Z
tags:
  - webpack
  - webpack-dev-server
  - browserstack
  - localhost
  - protip
---
When doing web development, it's essential to be able to test and verify your work on different screen-sizes and devices. For this we can use various tools like: [BrowserStack](https://www.browserstack.com), [ngrok](https://ngrok.com/) or a vitual machine running a different OS. 

I have been doing this for years and years, but then something happened a few years ago... It's been a while since it happened, so some of the details are a bit blurry. But what happened was that I suddenly couldn't test my site running on localhost on BrowserStack. It just wouldn't connect... 😲 I thought that BrowserStack had messed something up or something, so I tried testing my MacOS localhost on my virtual machine ([Parallels Desktop](https://www.parallels.com/eu/)) running Windows 10, but also this was impossible. I couldn't connect to the IP. At this point, I started getting a little frustrated. I started Googling around for answers. I blamed it on Parallels for not letting the connection go through.
And then... suddenly in some weird Parallels forum thread, I found the answer. It wasn't related to Parallels at all, instead the issue was related to webpack-dev-server 🤯 Apparently webpack-dev-server had stopped allowing remote connections.
The fix was to set the `host` option in the `devServer` object in the config.  Like this:

```javascript
// webpack.config.js

module.exports = {
  //...
  devServer: {
    host: '0.0.0.0' // Allows external connections to localhost
  }
};
```

Or via CLI like this:

```javascript
--host 0.0.0.0
```

[Read more in the docs](https://webpack.js.org/configuration/dev-server/#devserverhost)

Happy testing on localhost 🎉