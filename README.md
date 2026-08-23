# mikkelrom.com

Personal blog about web tech, architecture, performance, design systems and programming.
Built with [Eleventy](https://www.11ty.dev/), deployed on Netlify.

Originally started from the [Hylia](https://github.com/hankchizljaw/hylia) starter kit,
since stripped back to a plain Eleventy site.

## Requirements

Node 24 or newer, as pinned in `.nvmrc` and `engines`.

## Getting started

```sh
npm install
npm start
```

`npm start` compiles Sass in watch mode and runs the Eleventy dev server in parallel.

## Scripts

`npm run production` runs the full build that Netlify uses. It cleans `dist`,
compiles the stylesheet and runs Eleventy.

`npm run sass:process` regenerates `src/scss/_tokens.scss` from
`src/_data/tokens.json` and compiles `src/scss/global.scss` into
`src/_includes/assets/css/global.css`. Both generated files are gitignored, and the
compiled CSS is inlined into every page by `src/_includes/layouts/base.njk`.

`npm run mermaid` regenerates `src/images/caching-layers.svg` from the `.mmd` source.
The SVG is committed, so this only needs running when the diagram changes. It is
deliberately not part of the production build, because mermaid-cli pulls down a
headless Chromium.

## Structure

```
src/
  _data/        Site config, navigation and design tokens
  _includes/    Nunjucks layouts and partials, plus the compiled CSS
  filters/      Eleventy filters (dates, markdown)
  transforms/   Output transforms (HTML minify, heading permalinks)
  scss/         Styles, see below
  posts/        Blog posts in markdown
  pages/        Standalone pages
```

## Styles

The stylesheet is written in Sass using the module system (`@use`, not `@import`).

`src/_data/tokens.json` is the single source of truth for the size scale, colours and
font stacks. It feeds both `_tokens.scss` (via `json-to-scss`) and the styleguide page.

`_config.scss` turns those tokens into a utility config. `_utilities-generated.scss`
walks that config and emits a class per entry, plus a breakpoint-prefixed variant for
entries marked `responsive`, for example `.md\:box-flex`. `_core.scss` forwards the
config, the `get-size` and `get-color` functions, and the `media-query`,
`apply-utility` and `dark-mode` mixins. Component partials pull those in with
`@use '../core' as *`.

Load order in `global.scss` is deliberate. Global element styles must come before the
utility and component layers, because selectors such as `a:not([class])` and
`.site-foot a` have the same specificity, so whichever lands last wins.

## Content

Posts live in `src/posts` as markdown. Frontmatter drives the title, tags and social
image. Set `draft: true` to keep a post out of the listings and the feed.

There is no CMS. Posts are written and committed directly.

## Deployment

Netlify builds with `npm run production` and publishes `dist`, as configured in
`netlify.toml`, which also sets the security and caching headers.

## Licence

MIT, see `LICENSE.txt`.
