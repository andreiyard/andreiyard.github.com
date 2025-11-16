# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a personal engineering blog built with Hugo static site generator, using the PaperMod theme. The site is automatically deployed to GitHub Pages via GitHub Actions on every push to main.

**Live URL**: https://andreiyard.github.com
**Theme**: PaperMod (installed as git submodule)
**Hugo Version (CI)**: 0.146.7 extended
**Deployment**: GitHub Actions → GitHub Pages

## Development Commands

### Creating New Posts
```bash
hugo new --kind post content/posts/your-post-name.md
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

### Post Front Matter
Posts use extensive front matter configuration (see `archetypes/post.md`):
- `title`, `date`, `summary`, `tags` - Basic metadata
- `showToc`, `TocOpen` - Table of contents settings
- `draft` - Set to false to publish
- `cover.image` - Post cover image (hidden in list by default with `hiddenInList: true`)
- `ShowReadingTime`, `ShowBreadCrumbs`, `ShowPostNavLinks`, `ShowWordCount` - Display options

All posts go in `content/posts/` directory.

### Site Configuration
Main configuration in `hugo.toml`:
- Site parameters, theme settings, menu configuration
- Search enabled with Fuse.js (outputs JSON index)
- Edit post links point to GitHub repository
- Social icons: LinkedIn, Email, GitHub

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

- `content/posts/` - Blog post markdown files
- `archetypes/` - Content templates (especially `post.md`)
- `static/` - Static assets (images, etc.)
- `themes/PaperMod/` - Theme submodule (do not edit directly)
- `public/` - Build output (git-ignored)

## Theme Customization

The site uses PaperMod theme with custom configuration in `hugo.toml`:
- Home page shows info section (not profile mode)
- Search functionality enabled
- Code copy buttons, reading time, word count all enabled
- Social icons configured for LinkedIn, Email, GitHub
- use brief commit messages without mentioning claude