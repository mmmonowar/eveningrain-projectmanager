# Evening Rain — User Manual

## Overview

Evening Rain is a personal blog built with [Hugo](https://gohugo.io/) (static site generator) and deployed on GitHub Pages via GitHub Actions. It uses a two-repo architecture that separates site configuration from content, making it easy to manage posts independently from the presentation layer.

### Key Architecture Decisions

| Aspect | Choice |
|--------|--------|
| Static site generator | Hugo Extended v0.164+ |
| Hosting | GitHub Pages |
| CI/CD | GitHub Actions (auto-deploy on push to main) |
| Content approach | External repo mounted via `module.mounts` |
| Post types | blog, microblog, literary, media, resources |
| URLs | Clean slugs — `/dystopia/` instead of `/posts/2026/07/.../` |
| Dates | `published` frontmatter field mapped to Hugo's `.Date` |
| Theme | Dark-first with light toggle, persisted to localStorage |
| Search | Fully client-side vanilla JS (no external services) |
| Typography | Source Serif 4 (headings) + Source Sans 3 (body) |
| Responsive | Mobile-first, 3 breakpoints, fluid typography via `clamp()` |

---

## Project Structure

The repository splits into three top-level directories:

```
evening-rain/
├── eveningrain/                     # Hugo site root
│   ├── content/
│   │   └── search/
│   │       └── _index.md            # Search page content definition
│   ├── layouts/                     # All Hugo templates
│   │   ├── _default/
│   │   │   ├── baseof.html          # HTML shell (head, header, nav, footer)
│   │   │   ├── single.html          # Default post template
│   │   │   ├── list.html            # Archive page (posts grouped by year/month)
│   │   │   ├── taxonomy.html        # Tag/category filtered listing
│   │   │   └── terms.html           # Tag/category index pages
│   │   ├── blog/                    # Type-specific templates
│   │   ├── microblog/
│   │   ├── literary/
│   │   ├── media/
│   │   ├── resources/
│   │   ├── search/
│   │   │   └── list.html            # Dedicated search results page
│   │   ├── index.html               # Home page layout
│   │   └── index.json               # Search index JSON template
│   ├── static/
│   │   ├── css/
│   │   │   └── style.css            # All site styles (655 lines)
│   │   └── js/
│   │       ├── search.js            # Client-side search logic
│   │       └── theme.js             # Dark/light theme toggle
│   ├── public/                      # Build output (generated, not committed)
│   ├── hugo.yaml                    # Main Hugo configuration
│   └── .github/workflows/hugo.yml   # GitHub Actions deploy workflow
│
├── eveningrain-contents/            # Content repository
│   ├── 10 Posts/                    # All post content, organized by date
│   │   └── YYYY/
│   │       └── YYYY-MM/
│   │           └── YYYY-MM-DD/
│   │               └── YYYY-MM-DD-hh-mm-ss_post-type_post-title/
│   │                   ├── index.md              # Post content (frontmatter + body)
│   │                   └── assettype_asset-title.ext  # Optional media/assets
│   ├── 70 Metadata/
│   │   └── metadata.yaml            # Site-wide metadata (title, author, copyright)
│   └── 75 Templates/
│       └── blog_template.md         # Template for creating new posts
│
└── eveningrain-projectmanager/      # Project management meta-repo
    └── project-manager/
        ├── manual.md                # This document
        └── features.md              # Feature tracking
```

---

## Prerequisites

- **Hugo Extended v0.164+** — The Extended edition is required for Sass/SCSS processing and other features. Install via your package manager or download from [gohugo.io](https://gohugo.io/installation/).
- **Git** — For version control.
- **GitHub account** — With GitHub Pages enabled on the repository.

---

## Local Development

### Getting Started

```bash
# Clone the repository
git clone <repository-url>
cd evening-rain/eveningrain

# Start the Hugo development server with live reload
hugo server -D

# Open http://localhost:1313/eveningrain/ in your browser
```

The `-D` flag includes draft content. The content from `eveningrain-contents/10 Posts/` is automatically mounted into the site at build time — no symlinks or copies needed.

### Building for Production

```bash
hugo --minify
```

This generates the complete static site in the `public/` directory. The `--minify` flag compresses HTML, CSS, and JS. You can serve `public/` with any static file server to preview the production build locally.

---

## Content Creation Workflow

### Directory Structure for a New Post

Posts live under `eveningrain-contents/10 Posts/` organized by publication date:

```
10 Posts/
└── 2026/
    └── 2026-07/
        └── 2026-07-12/
            └── 2026-07-12-17-20-02_blog_dystopia/
                ├── index.md
                └── image_sunset.jpg   (optional asset)
```

The folder name encodes the timestamp and post type: `YYYY-MM-DD-hh-mm-ss_post-type_slug`. The file inside is always `index.md` (Hugo leaf bundle convention).

### Post Frontmatter

```yaml
---
published: 2026-07-12T17:20:02
title: Dystopia
slug: dystopia
type: blog
tags: [fiction, dystopia]
author: Muhammad Mustafa Monowar
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `published` | Yes | ISO 8601 datetime (`YYYY-MM-DDTHH:MM:SS`). Maps to Hugo's `.Date`. |
| `title` | Yes | Display title of the post. |
| `slug` | Yes | URL-friendly identifier. Becomes `/<slug>/`. Must be unique across all posts. |
| `type` | Yes | Post type: `blog`, `microblog`, `literary`, `media`, or `resources`. Determines which template renders the post. |
| `tags` | No | Array of tag strings, e.g. `[fiction, writing]`. Creates taxonomy pages at `/tags/<tag>/`. |
| `author` | No | Author name. Defaults to site-level author from metadata. |

### Post Content

Write the body in Markdown after the frontmatter. Any images or assets go in the same folder as `index.md` and are referenced by filename:

```markdown
---
published: 2026-07-12T17:20:02
title: Dystopia
slug: dystopia
type: blog
tags: [fiction, dystopia]
---

In the quiet corners of a rain-streaked afternoon...

![Sunset](image_sunset.jpg)
```

### Post Types

| Type | Directory Prefix | Purpose | Template |
|------|-----------------|---------|----------|
| `blog` | `_blog_` | Long-form articles and essays | `layouts/blog/single.html` |
| `microblog` | `_microblog_` | Short thoughts and updates | `layouts/microblog/single.html` |
| `literary` | `_literary_` | Poetry, short fiction, creative writing | `layouts/literary/single.html` |
| `media` | `_media_` | Photo essays, video posts | `layouts/media/single.html` |
| `resources` | `_resources_` | Curated links, tools, references | `layouts/resources/single.html` |

All five type-specific templates currently produce the same layout as the default post template. They exist as extension points — you can customize each type's template independently later.

### Creating a New Post (Step by Step)

1. Determine the current date and time for the folder name.
2. Create the directory tree: `10 Posts/YYYY/YYYY-MM/YYYY-MM-DD/YYYY-MM-DD-hh-mm-ss_post-type_post-title/`.
3. Create `index.md` inside with the frontmatter template (copy from `75 Templates/blog_template.md`).
4. Write the post content in Markdown.
5. Add any assets (images, videos, documents) to the same folder.
6. Build locally with `hugo server -D` to preview.
7. Commit and push — GitHub Actions will build and deploy automatically.

### Asset Naming Convention

| Asset Type | Format | Example |
|------------|--------|---------|
| Image | `image_title.ext` | `image_sunset.jpg` |
| Video | `video_title.ext` | `video_rain.mp4` |
| Document | `document_title.ext` | `document_notes.pdf` |

---

## Site Configuration

### `hugo.yaml` Key Settings

| Setting | Value | Purpose |
|---------|-------|---------|
| `baseURL` | `https://mmmonowar.github.io/eveningrain/` | Production URL. Critical for GitHub Pages sub-path deployment. |
| `buildFuture` | `true` | Include posts with future dates in builds. |
| `permalinks.posts` | `/:slug/` | Generates clean URLs from the `slug` frontmatter field. |
| `frontmatter.date` | `[published, date]` | Tells Hugo to use the `published` field first, falling back to `date`. |
| `module.mounts` | (see config) | Maps the external content repo and local search page into Hugo's content directory. |

### Content Mounts

Rather than using `contentDir` (which points to a single directory), the site uses Hugo's module mounts for flexibility:

```yaml
module:
  mounts:
    - source: content/search          # Local directory
      target: content/search          # Mounted to /search/
    - source: ../eveningrain-contents/10 Posts  # External content repo
      target: content/posts           # Mounted to /posts/
```

This means:
- The search page content lives in the site repo itself (under `eveningrain/content/search/`).
- All blog posts live in the separate `eveningrain-contents/10 Posts/` directory and appear under the `posts` section.

### Permalinks

The `/:slug/` pattern means every post gets a clean, readable URL:
- `https://mmmonowar.github.io/eveningrain/dystopia/`
- `https://mmmonowar.github.io/eveningrain/my-other-post/`

This is driven by the `slug` field in each post's frontmatter. Slugs must be unique across all posts.

### Date Handling

The `published` frontmatter field replaces Hugo's default `date` field. The mapping is set in `hugo.yaml`:

```yaml
frontmatter:
  date:
    - published
    - date
```

Hugo checks `published` first; if absent, it falls back to `date`. The format must be ISO 8601: `YYYY-MM-DDTHH:MM:SS`.

### Date-Nested Directory Sections

The nested year/month/day directories (`2026/`, `2026-07/`, `2026-07-13/`) each contain a `_index.md` file that suppresses them from appearing as standalone section pages:

```yaml
---
build:
  list: never
  render: never
---
```

This keeps the URL structure clean — only the post itself (via its slug) and the main `/posts/` archive page are rendered, not intermediate section pages.

---

## Features Reference

| Feature | Status | Summary |
|---------|--------|---------|
| FEAT-01 Hugo Project Config | Done | Site configuration with base URL, content mounts, permalinks, taxonomies, menus |
| FEAT-02 Base Template | Done | HTML5 shell with meta tags, responsive viewport, Google Fonts, SEO basics |
| FEAT-03 Home Page | Done | Landing page with recent posts, site title, tagline |
| FEAT-04 Post Template | Done | Single post layout: title, date, reading time, content, tags, prev/next navigation |
| FEAT-05 Archive Page | Done | All posts grouped by year and month, newest first |
| FEAT-06 Dark Theme CSS | Done | Full dark design system with CSS custom properties, typography, spacing |
| FEAT-07 Responsive Layout | Done | Mobile-first design, fluid typography, 3 breakpoints |
| FEAT-08 GitHub Actions CI | Done | Automated build and deploy to GitHub Pages on push to main |
| FEAT-09 Categories and Tags | Done | Taxonomy pages, tag badges on posts, terms index |
| FEAT-10 RSS Feed | Done | Auto-generated RSS, linked in menu and `<head>` |
| FEAT-11 Search | Done | Client-side vanilla JS search with JSON index, dropdown + dedicated page |
| FEAT-12 Dark Light Toggle | Done | Theme switcher with localStorage persistence and FOUC prevention |
| FEAT-13 Content Structure Mapping | Done | Mount-based content loading, clean URLs, date suppression, post-type system |

---

## Build and Deployment

### Manual Build

```bash
cd evening-rain/eveningrain
hugo --minify
```

The output goes to `public/`. You can preview it with:

```bash
# Using Python's built-in HTTP server
python -m http.server 8000 -d public

# Using npx serve
npx serve public
```

### Automatic Deployment (GitHub Actions)

Pushing to the `main` branch triggers the workflow defined in `.github/workflows/hugo.yml`:

1. Checks out the repository (including submodules if any).
2. Installs Hugo Extended.
3. Runs `hugo --minify`.
4. Deploys the `public/` directory to GitHub Pages.

The live site is at: **https://mmmonowar.github.io/eveningrain/**

---

## Troubleshooting

### "Page not found" for new posts

- Ensure the post is not in a `draft` or `future` state. The site uses `buildFuture: true`, so only `draft: true` blocks publication.
- Check that `slug` is set in the frontmatter and is unique.
- Verify the post is inside the mounted path (`eveningrain-contents/10 Posts/YYYY/...`).

### Build warnings about `languageCode`

```
WARN deprecated: project config key languageCode was deprecated...
```

Replace `languageCode` with `locale` in `hugo.yaml`. This is a benign deprecation warning in Hugo v0.158+.

### Search index not updating

- Verify `buildFuture: true` is set in `hugo.yaml`.
- Check that the post appears in the archive page at `/posts/` — if it's there, the search index should include it.
- Rebuild with `hugo --minify` and check `public/search.json` for the new entry.

### `public/search/index.html` missing

The search page content lives at `eveningrain/content/search/_index.md`. If the search page isn't rendering:

- Verify the file exists and has `layout: search` in its frontmatter.
- Check that the mount for `content/search` is present in `hugo.yaml`.
- Ensure `layouts/search/list.html` exists.

### CSS or JS not loading

- If deploying to a sub-path (like `eveningrain/`), ensure the `baseURL` in `hugo.yaml` includes the full path. The templates use `absURL` for all asset references, which resolves correctly relative to `baseURL`.
- Clear your browser cache or do a hard refresh (Ctrl+Shift+R).

### Theme toggle not working

- Check that `static/js/theme.js` exists and is loaded in `baseof.html`.
- Verify the FOUC-prevention inline script is present in `<head>` and comes before any CSS that depends on `data-theme`.
- Check browser console for errors.

---

## SEO & Metadata

### Site-Level Metadata

Defined in `eveningrain-contents/70 Metadata/metadata.yaml`:

```yaml
title: "Evening Rain"
author: "Muhammad Mustafa Monowar"
copyright: "© 2026 Muhammad Mustafa Monowar All rights reserved"
```

### Per-Post Metadata

Each post's frontmatter provides:

- **Title**: Used in `<title>` tags and Open Graph / Twitter Card metadata.
- **Description**: Falls back to the excerpt; the first paragraph is used if no explicit description is set.
- **Tags**: Each tag generates a feed at `/tags/<tag>/index.xml` for topic-specific RSS subscriptions.

### RSS Feed

The RSS feed is auto-generated by Hugo at `/index.xml` and `/tags/<tag>/index.xml`. It is linked in the site navigation menu and in the `<head>` of every page for auto-discovery.

---

## CSS Architecture

The stylesheet (`static/css/style.css`, ~655 lines) uses:

- **CSS custom properties** for the full color palette (9 colors for dark, all 9 redefined for light).
- **No preprocessor** — plain CSS only.
- **System font stack** as fallback; Google Fonts (Source Serif 4 + Source Sans 3) loaded via `<link>`.
- **Mobile-first** with breakpoints at 641px (tablet) and 1025px (desktop).
- **Fluid typography** using `clamp()` for headings.
- **Transitions** on `background-color`, `color`, and `border-color` with 0.3s ease. Respects `prefers-reduced-motion`.

### CSS Color Variables

| Variable | Dark Value | Light Value | Usage |
|----------|-----------|-------------|-------|
| `--black` | `#010101` | `#FFFFFF` | Page background |
| `--carbon-black` | `#1C1C1C` | `#F9F9F9` | Cards / secondary background |
| `--jet-black` | `#2B2B2B` | `#EBEBEB` | Borders / tertiary background |
| `--iron-grey` | `#3A3A3A` | `#D4D4D4` | Muted borders |
| `--charcoal` | `#4A4A4A` | `#B0B0B0` | Placeholder text |
| `--grey` | `#6F6F6F` | `#808080` | Secondary text |
| `--grey-olive` | `#8C8C8C` | `#5C5C5C` | Meta text |
| `--cool-steel` | `#B3B3B3` | `#3A3A3A` | Body text |
| `--platinum` | `#E0E0E0` | `#1A1A1A` | Headings |

---

## JavaScript Modules

### `search.js` (281 lines)

Vanilla JS, no dependencies. Runs as an IIFE.

**How it works:**
1. Reads the search index URL from the `data-search-index` attribute on `.search-container`.
2. Fetches the JSON index and caches it.
3. On input: tokenizes the query, scores each post (title=10pts, tags=8pts, summary=4pts, content=1pt), ranks results.
4. Renders results with highlighted `<mark>` tags around matched tokens.
5. Extracts excerpts near the first match position.

**Two modes:**
- **Dropdown** (header): Top 8 results in an absolute-positioned container. Keyboard accessible (Escape to close, Enter to navigate). Click-outside dismisses.
- **Search page** (`/search/`): Full results list with tag links. Reads `?q=` query parameter from URL for deep linking.

### `theme.js` (26 lines)

Simple theme toggle:
1. Reads `#theme-toggle` button.
2. Toggles `data-theme` attribute on `<html>` between `"dark"` and `"light"`.
3. Persists to `localStorage`.
4. Updates button icon: sun (☀) for dark mode, moon (☾) for light.

FOUC prevention is handled by a synchronous inline `<script>` in `<head>` that reads `localStorage` (or `prefers-color-scheme`) before any CSS renders.
