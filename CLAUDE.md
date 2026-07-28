# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A personal website for Anthony Pearson, published as a GitHub Pages site at
`AnthonyPearsonVU.github.io`. It is a set of hand-written static HTML pages — there is
**no build system, no framework, no package manager, and no dependencies**.

## Architecture

- All published content lives in [docs/](docs/): `index.html` (landing/hero), `about.html`, and
  `skills.html`. These are plain static HTML files. There is no JavaScript.
- All styling lives in a single shared stylesheet, [docs/site.css](docs/site.css), which every
  page links via `<link rel="stylesheet" href="site.css" />` in the `<head>`. There are no
  inline `<style>` blocks.
- `site.css` is organized into the theme definition (`:root` custom properties plus the
  `prefers-color-scheme: light` override), then shared rules (reset, `body`, `header`/`nav`,
  `main`, `footer`, `.page-title`), then page-specific sections (home hero/cards, about
  sections, skills tags). Class names are unique per page, so page-specific rules do not collide.
- The three pages share a common design system:
  - Header + `<nav>` (links to About, Skills, and the GitHub repo). The current page's nav
    link carries `class="active"`.
  - A GitHub-derived palette with both a dark and a light theme (see below).
  - A `<footer>` copyright line.

### Theming

Colors are **never hardcoded in rules** — every one is a CSS custom property (`--bg`, `--text`,
`--text-muted`, `--text-strong`, `--text-faint`, `--accent`, `--border`, `--surface`,
`--surface-hover`) declared on `:root` and referenced as `var(--name)`.

The dark GitHub palette is the default; a `@media (prefers-color-scheme: light)` block
redefines the same properties with light values, so the active theme follows the visitor's
OS/browser setting. `color-scheme: dark light` on `:root` keeps browser-rendered chrome
(scrollbars, form controls, caret) in sync. There is no toggle and still no JavaScript.

When adding a color, add it to **both** blocks. A literal hex value in a rule is a bug — it
will only be correct in one of the two themes.

### Important consequence

Styling is centralized in `site.css`, so a change to the color palette, layout, or any shared
component applies to all pages at once — and because colors go through the custom properties,
both themes stay consistent for free. However, the **header/nav markup is still duplicated**
in each HTML file by hand and must be kept in sync. Adding a new page means copying the header/nav
markup, linking `site.css`, and adding a nav link to the other pages.

## Deployment

- Deploys via GitHub Actions: [.github/workflows/deploy.yml](.github/workflows/deploy.yml).
- Trigger: any push to `main`. The workflow uploads the **`docs/` folder** (not the repo root)
  as the Pages artifact and deploys it. Files outside `docs/` (e.g. `README.md`) are not published.
- There is no test, lint, or build step in CI — the workflow only publishes the static files.

## Local preview

No server or tooling is required. Open a file directly, e.g.
`docs/index.html`, in a browser. To preview with working relative links, serve the folder with
any static server, for example:

```
python -m http.server --directory docs 8000
```

To check the theme you are not currently in, emulate the media feature in devtools
(Chrome/Edge: Rendering → "Emulate CSS media feature prefers-color-scheme"; Firefox:
Inspector → sun/moon toggle) rather than changing the OS setting.
