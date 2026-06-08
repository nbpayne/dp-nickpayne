# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A Jekyll-based cinematography portfolio site for Nick Payne (dp.nickpayne.com). Posts represent individual film/video projects. Asset pipeline is managed by Gulp.

## Commands

```bash
# Install dependencies (first time)
bundle install
yarn install
bower install

# Develop (compiles assets, starts Jekyll with livereload at http://127.0.0.1:4000)
gulp

# Jekyll only (no asset compilation)
bundle exec jekyll serve --livereload --drafts --future
```

There is no lint or test command — CSS is linted as part of the Gulp `css` task via `gulp-csslint`, and JS is linted via `gulp-jshint`.

## Asset pipeline

Gulp manages a two-layer source → output system. **Never edit files in `css/`, `js/`, or `_includes/head.html` / `_includes/foot.html` directly** — they are build artefacts.

| Edit here | Gets built to |
|---|---|
| `__sass/*.scss` | `css/*.min.css` |
| `__sass/vendor/*.scss` | `css/vendor/*.min.css` |
| `__js/main.js` | `js/` (via useref in `__includes/`) |
| `__includes/head.html`, `foot.html` | `_includes/head.html`, `foot.html` |

Bower dependencies are injected into `__includes/` by `gulp wiredep`, then the built result is copied to `_includes/`.

## Adding a new post

1. Create `_posts/YYYY-MM-DD-slug.md` with this front matter:

```yaml
---
layout: post
categories: ['Music Video']   # or 'Short Film', 'Documentary', etc.
title: >
  "Title" by Artist Name
role: Cinematographer / Colourist
homeImg: slug.00.jpg           # used on the home page grid
imgs:
  - slug.01.jpg
  - slug.02.jpg
link: https://...              # omit or leave blank if not yet released
published: true
---
```

2. Place full-resolution images in `img/YYYY-MM-DD-slug/` (matching the post filename without `.md`).
3. Place thumbnails in `img/YYYY-MM-DD-slug/th/` — the home page grid and carousel thumbnails are served from `th/`.
4. The `homeImg` thumbnail (`th/slug.00.jpg`) is shown on the home page card; it does **not** need to be in the `imgs` list.

## Jekyll config notes

- Site is served from `https://dp.nickpayne.com` with no `baseurl`.
- Permalink format: `/:year/:month/:day/:slug`.
- Jekyll is run with `--future` in development so future-dated posts are visible locally before they go live.
- `docs/**` pages are excluded from the sitemap.
