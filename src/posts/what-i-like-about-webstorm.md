---
layout: layouts/post.njk
title: What I like about WebStorm
metaDesc: These awesome features in WebStorm are the reason why I can't "just
  use VS Code like everyone else"
socialImage: images/webstorm-logo.svg
date: 2022-04-21T20:22:49.151Z
tags:
  - webstorm
  - editors
---
![WebStorm logo](images/webstorm-logo.svg "WebStorm logo")

More than once in my career I have been talking to colleagues about code editors or IDE’s. Stuff like what’s the best one and why? What features are need-to-haves? What is it that makes the editor feel so good?

So now I decided that I actually wanted to document some of my thoughts and preferences around this very opinionated topic. I’ll try and keep this post updated as time goes by.

## A bit of background

I started out as a backend developer working with C#, so I’ve been using Visual Studio quite a bit over the years. Moving into doing fulltime frontend development, I decided to try out WebStorm, and I haven’t looked back since. It keeps on growing on me, and I just like it even more the better I get to know it. Yes, I have tried out VS Code a couple of times - and no, it didn’t last very long each time. There simply were some awesome need-to-have features (in my opinion) that was missing. So yeah, my favourite editor is WebStorm.

## Features

I’ve listed all my favourite features of WebStorm, and then split them into two categories: Expected features and Awesome features.

### Expected features

These are features that you would expect from a modern IDE (like VS Code), to be available by default or through a plugin/extension.

* [Emmet](https://emmet.io/) support
* [Integrated terminal](https://www.jetbrains.com/help/webstorm/terminal-emulator.html)
* [Git integration](https://www.jetbrains.com/help/webstorm/version-control-integration.html)
* [Prettier support](https://www.jetbrains.com/help/webstorm/prettier.html)
* [ESlint support](https://www.jetbrains.com/help/webstorm/eslint.html)
* [TypeScript support](https://www.jetbrains.com/help/webstorm/typescript-support.html)
* [Major JS framework support](https://www.jetbrains.com/help/webstorm/vue-js.html) (Vue, React, Angular, Svelte etc.)
* Common keyboard shortcuts

  * [Move current line up](https://www.jetbrains.com/idea/guide/tips/move-line/) **⌥⇧↑** (Mac) or **Alt+Shift+Up Arrow** (Windows)
  * [Move current line down](https://www.jetbrains.com/idea/guide/tips/move-line/) **⌥⇧↓** (Mac) or **Alt+Shift+Down Arrow** (Windows)
  * Duplicate line **⌘+D** (Mac) or **CTRL+D** (Windows)
  * Go to line **⌘+L** (Mac) or **CTRL+L** (Windows)
  * Close file **⌘+W** (Mac) or **CTRL+W** (Windows)

### Awesome features

These are features that I personally think are really awesome, and not necessarily something that every editor has (WebStorm does, of course)

* [Searching - Find in Files](https://www.jetbrains.com/help/webstorm/finding-and-replacing-text-in-project.html) **⌘⇧F** (Mac) **CTRL+F** (Windows) - this is by all means my most favourite feature of WebStorm. Compared to VS Code, this is to me 100 times better. 

  * I like that you have the search results in a modal window (instead of it being hidden away in a sidebar 🙄).
  * I like that you get a preview pane showing the selected file/search result. You can even select text and copy it, you can even edit the file in the preview pane.
  * In new projects, it's very common that the search results are polluted with too many results from dist-folders and minified files and so on. WebStorm makes it easy to exclude entire folders by right-clicking a folder and choosing "Mark Directory As", "Excluded". Easy peasy!
* Searching everywhere **Double ⇧** 
* [TODO tree](https://www.jetbrains.com/help/webstorm/using-todo.html)
* [Test runner](https://www.jetbrains.com/help/webstorm/test-runner-tab.html)
* [Auto import](https://www.jetbrains.com/help/webstorm/settings-auto-import.html)
* [Optimize imports](https://blog.jetbrains.com/webstorm/2018/05/optimize-imports-in-webstorm/)
* [Reformat file](https://www.jetbrains.com/help/webstorm/reformat-and-rearrange-code.html)
* [Resolve git conflicts](https://www.jetbrains.com/help/webstorm/resolve-conflicts.html)
* [Keymap](https://www.jetbrains.com/help/webstorm/settings-keymap.html) aka. shortcuts

## Extras

### **Font**

I use JetBrains own free [JetBrains Mono](https://www.jetbrains.com/lp/mono/) font in WebStorm, it’s really nice and it also supports [ligatures](https://www.jetbrains.com/lp/mono/#ligatures) which I like. [Heres how to enable ligatures in WebStorm](https://www.jetbrains.com/lp/mono/#ligatures).

### Theme

Currently I use the [Night Owl](https://plugins.jetbrains.com/plugin/10936-night-owl-theme) theme.

### My preferred custom settings

* I have enabled “[Mark modified (*)](https://www.jetbrains.com/help/idea/settings-editor-tabs.html)”, because I like to see which files have unsaved changes.
* For Prettier, I use this Run for files pattern: `{**/*,*}.{js,ts,jsx,tsx}`. I also enable both the “On ‘Reformat Code’ action” checkmark and the “On save” checkmark.
* I like to add “*.cshtml” to the HTML filetype, under Settings, Editor, File Types, Recognized File Types. This makes WebStorm work better with HTML in a cshtml file, which is a huge improvement. This tip should work for most filetypes that are close to being HTML.

### My WebStorm extensions

* [Dummy Text Generator](https://plugins.jetbrains.com/plugin/7216-dummy-text-generator)
* [Import Cost](https://plugins.jetbrains.com/plugin/9970-import-cost)
* [Material Theme UI](https://plugins.jetbrains.com/plugin/8006-material-theme-ui)
* [MDX](https://plugins.jetbrains.com/plugin/14944-mdx)
* [Night Owl Theme](https://plugins.jetbrains.com/plugin/10936-night-owl-theme)
* [PostCSS](https://plugins.jetbrains.com/plugin/8578-postcss)
* [Rainbow Brackets](https://plugins.jetbrains.com/plugin/10080-rainbow-brackets)

## The end

I hope you liked this post, if you did please share it with a friend 🙂

If you know of any tips I missed or have a question, feel free to hit me up at [@mikkelrom](https://twitter.com/mikkelrom)