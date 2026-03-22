# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is François Lanusse's academic personal website, built with Jekyll using the [al-folio](https://github.com/alshedivat/al-folio) theme. It is deployed to GitHub Pages at https://eiffl.github.io.

## Build & Serve Commands

```bash
# Local development (standard)
bundle install
bundle exec jekyll serve

# Local development (Docker, recommended on Windows)
docker-compose up              # uses pre-built image, serves on port 8080

# Build a custom Docker image
docker-compose -f docker-local.yml build
docker-compose -f docker-local.yml up

# Production build
JEKYLL_ENV=production bundle exec jekyll build

# Manual deploy to gh-pages branch
./bin/deploy
```

## Regenerating Publications

Publications are auto-generated from NASA ADS via `scripts/bibgen.py`. This script requires the `ADS_API_KEY` environment variable and fetches all papers by "Lanusse, F", enriches them with Altmetric data, applies customizations from `_data/custom_bib.yml`, and writes to `_bibliography/papers.bib`.

## Architecture

### Content Structure
- `_pages/` — Main site pages (about, publications, CV, projects, tutorials, repositories). The about page (`about.md`) is the homepage (`permalink: /`).
- `_news/` — News items displayed on the homepage (collection with `output: true`)
- `_projects/` — Project pages displayed on a responsive grid (collection with `output: true`)
- `_bibliography/papers.bib` — BibTeX file powering the publications page via jekyll-scholar

### Data Files (`_data/`)
- `coauthors.yml` — Co-author metadata (names + URLs) for auto-linking in publications
- `custom_bib.yml` — Per-entry overrides applied by `bibgen.py` (selected, preview, code, etc.)
- `cv.yml` — Structured CV data
- `repositories.yml` — GitHub repos/users displayed on the repositories page
- `venues.yml` — Venue abbreviation links for publication badges

### Theme & Layout
- `_layouts/` — Page templates (about, page, post, bib, cv, distill, etc.)
- `_includes/` — Reusable partials (header, footer, news, social, repository cards, etc.)
- `_sass/` — Stylesheets. Theme color is set via `--global-theme-color` in `_sass/_themes.scss`.
- `_plugins/` — Custom Ruby plugins: `external-posts.rb` (RSS feed integration), `hideCustomBibtex.rb` (filters internal bib keywords from output)

### Jekyll Scholar Configuration
Author highlighting matches entries where last name is "Lanusse" and first name matches "François", "Francois", "F", or "F." (configured in `_config.yml` under `scholar:`). The `max_author_limit` is 3.

### Key BibTeX Keywords
Custom bibtex fields that control publication display: `abbr`, `abstract`, `arxiv`, `bibtex_show`, `html`, `pdf`, `supp`, `blog`, `code`, `poster`, `slides`, `website`, `preview`, `altmetric`, `selected`. These are filtered from rendered bib output via `filtered_bibtex_keywords` in `_config.yml`.
