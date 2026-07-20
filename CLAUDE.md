# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A Jekyll-based cinematography portfolio site for Nick Payne (dp.nickpayne.com). Posts represent individual film/video projects. Asset pipeline is managed by Gulp.

## Commands

**Install dependencies (first time):**
```sh
bundle install   # Jekyll and Ruby gems
yarn             # Node/Gulp dev tools + client-side deps (Bootstrap, jQuery)
```

**Develop (build + serve + watch, livereload at http://127.0.0.1:4000):**
```sh
gulp
```

**Build + serve (no file watching):**
```sh
gulp build
```

**Watch CSS/JS only (Jekyll already running):**
```sh
gulp liveReload
```

The dev server runs with `--livereload`, `--drafts`, and `--future` flags.

There is no separate lint or test command — ESLint runs on `__js/**/*.js` as part of the `lintJS` Gulp task and fails the build on errors.

## Asset pipeline

The Gulp pipeline runs in this order: **clean → css → lintJS → js → jekyllServe**. `gulp liveReload` watches `__sass/` and `__js/` and re-runs the relevant tasks on change.

**Never edit files in `css/`, `js/`, or `_includes/head.html` / `_includes/foot.html` directly** — they are build artefacts / hand-authored (no longer Gulp-generated).

| Edit here | Gets built to |
|---|---|
| `__sass/*.scss` | `css/main.css`, `css/main.min.css` |
| `__sass/vendor/*.scss` | `css/vendor/*.min.css` |
| `__js/main.js` | `js/app.min.js` |
| `node_modules/jquery` | `js/vendor.min.js` |
| `node_modules/bootstrap` | `js/bootstrap.min.js` |

Bootstrap and jQuery are installed as Yarn dependencies (`package.json`) and bundled directly from `node_modules` — there is no Bower step. ESLint config lives in `eslint.config.js` (flat config format).

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

## Layouts

| Layout | Use for |
|---|---|
| `default` | Base wrapper (nav + footer). Rarely used directly. |
| `home` | Index page — bio + 3-column post grid. |
| `post` | Individual project — Bootstrap carousel + thumbnail strip. |
| `category` | Filtered post grid. Requires `displayCategory: Music Video` (or similar) in front matter. |
| `page` | Plain Markdown content. |
| `two-col` | Two-column layout. |

## Jekyll config notes

- Site is served from `https://dp.nickpayne.com` with no `baseurl`.
- Permalink format: `/:year/:month/:day/:slug`.
- Jekyll is run with `--future` in development so future-dated posts are visible locally before they go live.
- `docs/**` pages are excluded from the sitemap.
- Contact form uses Formspree (`formid: xnqwgkzg` in `_config.yml`).

## Other directories

- `_email/` — standalone Bootstrap Email templates; not part of the Jekyll build.
- `docs/` — excluded from the sitemap; used for internal/reference pages.
