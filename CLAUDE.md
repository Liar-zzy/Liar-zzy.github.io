# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

An academic personal website for Zhenyuan Zhang (Ph.D. student at HKUST), built with the [al-folio](https://github.com/alshedivat/al-folio) Jekyll theme. The site includes publications, CV, projects, blog, news, and teaching pages.

## Build & Development

### Docker (recommended)

```bash
docker compose pull && docker compose up    # Start dev server at http://localhost:8080
docker compose up --build                   # Rebuild after dependency changes
docker compose down                         # Stop and free port 8080
```

### Local (legacy)

```bash
bundle install
pip install jupyter
bundle exec jekyll serve --port 4000
```

### Pre-Commit

```bash
npm install --save-dev prettier @shopify/prettier-plugin-liquid  # first time only
npx prettier . --write                                           # format all files
```

Prettier formatting is enforced by CI — PRs will fail if not formatted.

## Architecture

**Jekyll static site** using Liquid templates, SCSS, and Kramdown markdown.

- `_config.yml` — Central configuration: site metadata, feature flags (`enable_*`), plugin settings, Jekyll Scholar config, third-party library versions
- `_pages/` — Top-level pages (about, cv, publications, projects, teaching, blog)
- `_layouts/` — Liquid layout templates (about, post, bib, cv, distill, page, etc.)
- `_includes/` — Reusable Liquid partials (header, footer, head, scripts, figure, video, etc.)
- `_sass/` — SCSS stylesheets organized by component (`_themes.scss` for dark/light mode, `_variables.scss` for design tokens)
- `_scripts/` — JavaScript for optional features (analytics, comments, search, cookie consent)
- `_bibliography/papers.bib` — BibTeX file for publications, processed by jekyll-scholar. Supports custom fields: `pdf`, `code`, `preview`, `selected`, `arxiv`, `blog`, `slides`, `video`, `website`
- `_data/` — YAML data files: `socials.yml` (social links), `cv.yml` (CV data), `coauthors.yml`, `citations.yml`, `repositories.yml`, `venues.yml`
- `_posts/` — Blog posts (format: `YYYY-MM-DD-title.md`)
- `_news/`, `_projects/`, `_teachings/`, `_books/` — Collection content
- `assets/json/resume.json` — JSON Resume data fetched by `jekyll-get-json`

### Key Plugin: Jekyll Scholar

Configured in `_config.yml` under `scholar:`. Highlights author names matching `last_name: [Zhang]`, `first_name: [Zhenyuan]`. Bibliography rendered via `_layouts/bib.liquid`.

### Configuration Gotchas

- `url` and `baseurl` in `_config.yml` must be set correctly together (personal site: `url: https://username.github.io` + empty `baseurl`)
- YAML values with special characters (`:`, `&`, `#`) must be quoted
- Feature flags in `_config.yml` control optional functionality (math, dark mode, masonry, medium zoom, etc.)

## File-Type Specific Instructions

Detailed instructions for each file type exist at `.github/instructions/`:
- `markdown-content.instructions.md` — for `_posts/`, `_pages/`
- `yaml-configuration.instructions.md` — for `_config.yml`, `_data/`
- `bibtex-bibliography.instructions.md` — for `_bibliography/`
- `liquid-templates.instructions.md` — for `_includes/`, `_layouts/`
- `javascript-scripts.instructions.md` — for `_scripts/`

## CI/CD

GitHub Actions workflows in `.github/workflows/`:
- `deploy.yml` — Builds with Jekyll, runs purgecss, deploys to gh-pages branch
- `prettier.yml` — Mandatory formatting check
- `broken-links.yml` / `broken-links-site.yml` — Link validation
- `axe.yml` — Accessibility testing
- `update-citations.yml` — Auto-updates citation counts
- `render-cv.yml` — Renders CV from RenderCV format
