# Anthony Pearson

Website to know about me and other things.

Live at [anthonypearsonvu.github.io](https://anthonypearsonvu.github.io).

## Project structure

```
docs/              published site (this folder is what gets deployed)
  index.html       landing page
  about.html       about page
  skills.html      skills page
  site.css         shared stylesheet for all pages
.github/workflows/
  deploy.yml       GitHub Pages deployment
```

## Setup

There is nothing to install. This is a set of hand-written static HTML pages with
no build system, no framework, no package manager, and no dependencies.

```bash
git clone https://github.com/AnthonyPearsonVU/AnthonyPearsonVU.github.io.git
cd AnthonyPearsonVU.github.io
```

## Run locally

The simplest option is to open `docs/index.html` directly in a browser.

To preview with working relative links (recommended), serve the `docs/` folder with
any static web server, then visit <http://localhost:8000>:

```bash
# Python 3
python -m http.server --directory docs 8000
```

```bash
# Node.js
npx serve docs
```

Edits to the HTML or CSS show up on refresh — no restart or rebuild needed.

## Theming (light / dark)

The site ships both a light and a dark theme and picks one automatically from the
visitor's OS or browser setting. There is no toggle and no JavaScript.

Every color is a CSS custom property defined on `:root` in
[docs/site.css](docs/site.css). The dark GitHub palette is the default; a
`@media (prefers-color-scheme: light)` block redefines the same properties with
light values. `color-scheme: dark light` on `:root` also makes browser-rendered
chrome (scrollbars, form controls, caret) match the active theme.

| Property          | Dark      | Light     | Used for                       |
| ----------------- | --------- | --------- | ------------------------------ |
| `--bg`            | `#0d1117` | `#ffffff` | page background                |
| `--text`          | `#c9d1d9` | `#24292f` | body text, nav active, tags    |
| `--text-muted`    | `#8b949e` | `#57606a` | secondary/body paragraph text  |
| `--text-strong`   | `#e6edf3` | `#1f2328` | page and hero headings         |
| `--text-faint`    | `#484f58` | `#6e7781` | footer                         |
| `--accent`        | `#58a6ff` | `#0969da` | links, headings, hover borders |
| `--border`        | `#21262d` | `#d0d7de` | header/footer rules, outlines  |
| `--surface`       | `#161b22` | `#f6f8fa` | cards and tags                 |
| `--surface-hover` | `#1c2128` | `#eaeef2` | card hover background          |

To add a new color, define it in both blocks and reference it as
`var(--name)` — never hardcode a hex value in a rule, or that rule will only be
correct in one theme.

To test the other theme without changing your OS setting, use your browser
devtools: Chrome/Edge DevTools → Rendering → "Emulate CSS media feature
prefers-color-scheme", or Firefox DevTools → Inspector → the sun/moon toggle.

## Build

There is no build step. The files in `docs/` are served as-is.

## Deploy

Deployment is automatic. Any push to `main` triggers
[.github/workflows/deploy.yml](.github/workflows/deploy.yml), which uploads the
`docs/` folder as the GitHub Pages artifact and publishes it. There is no test,
lint, or build stage in CI.

Files outside `docs/` (including this README) are not published.

## Adding a page

1. Copy an existing page in `docs/` to keep the header, nav, and footer markup consistent.
2. Link the shared stylesheet in the `<head>`: `<link rel="stylesheet" href="site.css" />`.
3. Add a nav link to the new page in every other page — the nav markup is duplicated
   per file and must be kept in sync by hand.
4. Mark the current page's nav link with `class="active"`.
