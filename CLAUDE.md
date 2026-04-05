# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

Arjun Gupta's personal academic website hosted on GitHub Pages at `arjung93.github.io`. Built on the [Academic Pages](https://academicpages.github.io/) template, which is a fork of the Minimal Mistakes Jekyll theme.

## Running Locally

```bash
bundle install          # install Ruby dependencies (delete Gemfile.lock and retry if errors)
jekyll serve -l -H localhost  # serve at localhost:4000 with live reload
```

Or with Docker:
```bash
docker build -t jekyll-site .
docker run -p 4000:4000 --rm -v $(pwd):/usr/src/app jekyll-site
```

## Site Architecture

Content is organized as Jekyll collections. Each collection maps to a directory of markdown files:

| Directory | URL path | Purpose |
|-----------|----------|---------|
| `_pages/` | various | Static pages (about, cv, sitemap, etc.) |
| `_posts/` | `/year/month/day/title/` | Blog posts |
| `_publications/` | `/publications/:path/` | Academic papers |
| `_talks/` | `/talks/:path/` | Conference talks |
| `_portfolio/` | `/portfolio/:path/` | Projects |
| `_teaching/` | `/teaching/:path/` | Teaching entries |

**Key config files:**
- `_config.yml` — site-wide settings: author info, plugins, collection definitions, default frontmatter
- `_data/navigation.yml` — controls header nav links (currently: Portfolio, Blog Posts, CV)
- `_data/ui-text.yml` — UI string overrides by locale

**Layouts** (`_layouts/`): `single`, `archive`, `archive-taxonomy`, `talk`, `splash`, `default`, `compress`

**Templates** (`_includes/`): `author-profile.html`, `masthead.html`, `sidebar.html`, `head.html`, and collection-specific partials like `archive-single-cv.html`

**Styles** (`_sass/`): SCSS source; compiled output goes to `assets/css/`

## Content Editing Patterns

### Adding a blog post
Create `_posts/YYYY-MM-DD-title.md` with frontmatter:
```yaml
---
title: "Post Title"
date: YYYY-MM-DD
permalink: /posts/YYYY/MM/slug/
tags: [tag1, tag2]
---
```

### Adding a publication
Create `_publications/YYYY-MM-DD-paper-slug.md`. The CV page auto-populates publications via `{% for post in site.publications reversed %}`.

### CV page (`_pages/cv.md`)
Manually maintained markdown with Liquid loops at the bottom that auto-append Publications, Talks, and Teaching entries from their respective collections. The `files/` directory holds the actual CV PDF.

### Author sidebar
Edit `_config.yml` under `author:` for bio, avatar, social links. Avatar image goes in `images/`.

## Deployment

Pushing to `master` triggers GitHub Pages automatic build and deploy. No CI configuration needed — GitHub Pages runs Jekyll with `--safe` (whitelisted plugins only). The plugins in use (`jekyll-feed`, `jekyll-sitemap`, `jemoji`, `jekyll-paginate`) are all GitHub Pages compatible.
