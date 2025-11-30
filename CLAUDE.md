# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a personal engineering blog built with Hugo static site generator, using the PaperMod theme. The site is automatically deployed to GitHub Pages via GitHub Actions on every push to main.

**Live URL**: https://andreiyard.github.com
**Theme**: PaperMod (installed as git submodule)
**Hugo Version (CI)**: 0.146.7 extended
**Deployment**: GitHub Actions → GitHub Pages

## Multilingual Support

The site supports three languages:
- **English (default)** - `content/en/`
- **Russian** - `content/ru/`
- **Serbian (Latin script)** - `content/sr/`

Language configuration is in `hugo.toml` under `[languages]` section. Each language has its own:
- Content directory (`contentDir`)
- Translated home page intro (`params.homeInfoParams`)
- Translated menu items (Search, Tags)

Serbian uses Latin alphabet (latinica) throughout the site. Custom i18n file at `i18n/sr.yaml`.

### URL Structure for Multilingual Content

**IMPORTANT**: Hugo generates different URL paths for the default language vs other languages:

- **English (default language)**: URLs are at the **root level WITHOUT `/en/` prefix**
  - Posts: `/posts/article-name/`
  - Series: `/series/series-name/`
  - Tags: `/tags/tag-name/`
  - Search: `/search/`

- **Russian**: URLs include `/ru/` prefix
  - Posts: `/ru/posts/article-name/`
  - Series: `/ru/series/series-name/`
  - Tags: `/ru/tags/tag-name/`
  - Search: `/ru/search/`

- **Serbian**: URLs include `/sr/` prefix
  - Posts: `/sr/posts/article-name/`
  - Series: `/sr/series/series-name/`
  - Tags: `/sr/tags/tag-name/`
  - Search: `/sr/search/`

**When writing internal links in content:**
- ✅ English articles: Use `/series/homelab-setup/` (NO /en/ prefix)
- ✅ Russian articles: Use `/ru/series/homelab-setup/` (WITH /ru/ prefix)
- ✅ Serbian articles: Use `/sr/series/homelab-setup/` (WITH /sr/ prefix)
- ❌ **NEVER** use `/en/` prefix - it will result in 404 errors

This behavior is controlled by `defaultContentLanguage = 'en'` in `hugo.toml`. To change this and put English under `/en/`, you would need to set `defaultContentLanguageInSubdir = true`, but this would break existing URLs.

## Development Commands

### Creating New Posts
```bash
# English (default)
hugo new --kind post content/en/posts/your-post-name.md

# Russian
hugo new --kind post content/ru/posts/your-post-name.md

# Serbian
hugo new --kind post content/sr/posts/your-post-name.md
```
This uses the post archetype template from `archetypes/post.md` which includes all necessary front matter fields.

### Local Development
```bash
# Start local development server with drafts
hugo server -D

# Start without drafts (production-like)
hugo server

# Build the site locally
hugo --gc --minify
```

The built site outputs to `./public` directory.

### Git Submodules
The PaperMod theme is installed as a git submodule. When cloning fresh:
```bash
git submodule update --init --recursive
```

## Content Structure

### Post Organization
Posts are stored as individual markdown files (not page bundles):
- `content/en/posts/article-name.md`
- `content/ru/posts/article-name.md`
- `content/sr/posts/article-name.md`

Posts are organized by language in their respective directories.

### Images and Static Assets
Images are stored once in `/static/posts/` and shared across all language versions:
- Post images: `/static/posts/article-slug/images/`
- Image references in markdown use absolute paths from root: `/posts/article-slug/images/filename.jpg`
- Benefits: No duplication, easier maintenance, single source of truth for media

Example in front matter:
```yaml
cover:
    image: "/posts/homelab-part1-minipc-proxmox/images/cover.jpg"
```

In markdown body:
```markdown
{{< figure src="/posts/article-slug/images/photo.jpg" alt="Description" caption="Caption text" >}}
```

### Post Front Matter
Posts use extensive front matter configuration (see `archetypes/post.md`):
- `title`, `date`, `summary`, `tags` - Basic metadata
- `showToc`, `TocOpen` - Table of contents settings
- `draft` - Set to false to publish
- `cover.image` - Post cover image (use absolute path: `/posts/article-slug/images/cover.jpg`)
- `ShowReadingTime`, `ShowBreadCrumbs`, `ShowPostNavLinks`, `ShowWordCount` - Display options

### Site Configuration
Main configuration in `hugo.toml`:
- Site parameters, theme settings, menu configuration
- Search enabled with Fuse.js for all three languages
- Edit post links point to GitHub repository
- Social icons: LinkedIn, Email, GitHub
- Custom taxonomies: `tags`, `categories`, `series` (for organizing article series)

### Search Configuration
Search is enabled for all three languages with dedicated search pages:
- English: `/en/search/`
- Russian: `/ru/search/`
- Serbian: `/sr/search/`

Each language outputs its own JSON index for Fuse.js search functionality.
Configuration in `hugo.toml` includes per-language outputs:
```toml
[languages.en.outputs]
  home = [ "HTML", "RSS", "JSON" ]
[languages.ru.outputs]
  home = [ "HTML", "RSS", "JSON" ]
[languages.sr.outputs]
  home = [ "HTML", "RSS", "JSON" ]
```

Search configuration (applies to all languages):
- Case-insensitive fuzzy search
- Searches: title, permalink, summary, content
- Configurable via `[params.fuseOpts]` in hugo.toml

### RSS Feeds
RSS is automatically generated for all languages:
- English: `/en/index.xml`
- Russian: `/ru/index.xml`
- Serbian: `/sr/index.xml`

### Organizing Article Series
For multi-part article series, use the `series` front matter field:
```yaml
---
title: "Homelab Part 1: Setup"
tags: ["homelab", "proxmox"]
series: ["homelab-setup"]
---
```

This creates a dedicated page at `/series/homelab-setup/` listing all articles in the series.

### Translation Workflow
When creating multilingual content:
1. Write original version in preferred language
2. Translate to other languages (maintain consistent structure)
3. Use same slug for all language versions (`article-name.md`)
4. Reference shared images from `/static/posts/` using absolute paths
5. Keep `series` and `tags` identical across translations
6. Translate: `title`, `summary`, image `alt` and `caption` text
7. All versions share the same images (no duplication needed)

## Deployment Architecture

### GitHub Actions Workflow
Located at `.github/workflows/hugo.yaml`:
- Triggers on push to main or manual dispatch
- Installs Hugo extended v0.146.7 and Dart Sass
- Checks out repo with submodules
- Builds with `--gc --minify` flags
- Uses Hugo cache to speed up builds
- Deploys artifact to GitHub Pages

**Important**: The workflow configures Git with `core.quotepath false` to handle non-ASCII filenames correctly.

## Key Directories

- `content/en/` - English content (posts, search page)
- `content/ru/` - Russian content (posts, search page)
- `content/sr/` - Serbian content (Latin script, posts, search page)
- `archetypes/` - Content templates (especially `post.md`)
- `i18n/` - Custom i18n translations (sr.yaml for Serbian)
- `static/posts/` - Centralized images shared across all languages
- `static/` - Other static assets
- `themes/PaperMod/` - Theme submodule (do not edit directly)
- `public/` - Build output (git-ignored)
- `.notes/` - Project notes and session documentation (git-ignored, not committed)

## Theme Customization

The site uses PaperMod theme with custom configuration in `hugo.toml`:
- Home page shows info section (not profile mode)
- Search functionality enabled
- Code copy buttons, reading time, word count all enabled
- Social icons configured for LinkedIn, Email, GitHub
- use brief commit messages without mentioning claude

## Active Article Series

### Homelab Series
- **Series slug:** `homelab-setup`
- **Status:** Part 1 published, Part 2 draft (awaiting screenshots)
- **Planning doc:** `.notes/homelab-series.md`
- **Topics:** Mini PC setup, Proxmox, services, remote access, VyOS networking

### 3D Printing Series
- **Series slug:** `3d-printing`
- **Status:** Part 1 draft in progress
- **Planning doc:** `.notes/3d-printing-series.md`
- **Screenshots checklist:** `.notes/print3d-part1-screenshots.md`
- **Hardware:** Creality Hi printer
- **Topics:** Printer selection, first prints, slicer comparison, multi-color printing, PETG experience
- **Planned articles:**
  1. `print3d-part1-creality-hi` - Printer selection and first prints (draft)
  2. `print3d-part2-calibration-slicers` - Calibration fix and slicer comparison (planned)
  3. `print3d-part3-multicolor` - Multi-color printing with pause/swap (planned)
  4. `print3d-part4-labrax-petg` - Labrax project with PETG (planned)