---
layout: layouts/post.njk
socialImage: /images/pankaj-patel-6jvlsdgmace-unsplash-1-.jpg
date: 2020-07-29T22:37:33.082Z
title: Does a media-query affect specificity?
metaTitle: Does a media-query affect specificity? Let's find out...
metaDesc: A while back I had a nice and healthy discussion with my colleague,
  about whether having a rule of CSS, wrapped in a media-query would affect
  specificity or not.
tags:
  - css
  - specificity
  - protip
---
{% first-letter "A while back I had a nice and healthy discussion with my colleague, about whether having a rule of CSS, wrapped in a media-query would affect specificity or not. I was pretty sure that a media-query didn't affect the specificity, but when we talked about it back and forth, I started doubting myself a tiny bit. So I decided to make a simple test, and I want to share the findings here, hoping someone can use it." %}

So let's have a look at a super simple example, that also gives away the answer quite clearly:

<p class="codepen" data-height="491" data-theme-id="dark" data-default-tab="css,result" data-user="mikkelrom" data-slug-hash="GRobrqx" style="height: 491px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="media-query specificity">
  <span>See the Pen <a href="https://codepen.io/mikkelrom/pen/GRobrqx">
  media-query specificity</a> by Mikkel Rom Engholm (<a href="https://codepen.io/mikkelrom">@mikkelrom</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

If we take a closer look at the CSS in Chrome dev-tools, we can also see that the media-query doesn't affect it at all.
 
![image](/images/media-query-specificity.jpg)

### Conclusion
So there you have it, media-queries **does not** affect specificity. A media-query only decides whether the code inside it should be enabled or not.

In comparison, it's the same mechanism as for the media attribute for the [the link-tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link#attr-media), [the source-tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/source#attr-media) and [the style-tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/style#attr-media): 

```html
<!-- The link tag media attribute -->
<link href="style.css" media="screen and (max-width: 600px)" />

<!-- The source tag media attribute -->
<picture>
    <source srcset="/media/examples/surfer-240-200.jpg"
            media="(min-width: 800px)">
    <img src="/media/examples/painted-hand-298-332.jpg" />
</picture>

<!-- The style tag media attribute -->
<style media="all and (max-width: 500px)">
    p {
      color: blue;
      background-color: yellow;
    }
</style>
```

Read more: [MDN - Media queries](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_queries)
