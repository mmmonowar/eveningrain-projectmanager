# Features

---

## [FEAT-01] Done Hugo Project Config
- **Status**: Done
- **Description**: Hugo configuration file with base URL and theme settings
- **Details**:
  - Create `hugo.yaml` in `/eveningrain/`
  - Set base URL for GitHub Pages deployment
  - Configure title, description, and theme parameters
  - Set content directory to reference `/eveningrain-contents/10 Posts/`
  - Configure permalinks for clean URL structure
  - Enable page bundles for post folders with assets

---

## [FEAT-02] Done Base Template
- **Status**: Done
- **Description**: HTML5 base template with meta tags and responsive viewport
- **Details**:
  - Create `layouts/_default/baseof.html`
  - Include `<head>` with meta tags, CSS, fonts
  - Responsive viewport meta tag
  - Block structure for child templates

---

## [FEAT-03] Done Home Page
- **Status**: Done
- **Description**: Landing page with recent posts list, site title, and tagline
- **Details**:
  - Create `layouts/index.html`
  - Display site title and tagline
  - List recent posts with title, date, and excerpt
  - Link to individual posts

---

## [FEAT-04] Done Post Template
- **Status**: Done
- **Description**: Individual post layout with title, date, content, and reading time
- **Details**:
  - Create `layouts/_default/single.html`
  - Display post title and publication date
  - Render markdown content
  - Show estimated reading time
  - Navigation to previous/next posts
  - Parse frontmatter: `published`, `title`, `tags`, `author`

---

## [FEAT-05] Done Archive Page
- **Status**: Done
- **Description**: Posts grouped by month and year for browsing
- **Details**:
  - Create `layouts/_default/list.html`
  - Group posts by year and month
  - Display post title, date, and excerpt
  - Chronological ordering (newest first)

---

## [FEAT-06] Done Dark Theme CSS
- **Status**: Done
- **Description**: CSS implementing Evening Rain design system with dark theme and typography
- **Details**:
  - Create `static/css/style.css`
  - CSS custom properties for color palette
  - Source Serif 4 for headings
  - Source Sans 3 for body text
  - Dark background with light text

---

## [FEAT-07] Done Responsive Layout
- **Status**: Done
- **Description**: Mobile-first responsive design for clean reading experience
- **Details**:
  - Breakpoints for mobile, tablet, desktop
  - Fluid typography scaling
  - Readable line lengths (65-75 characters)
  - Proper spacing and margins

---

## [FEAT-08] Done GitHub Actions CI
- **Status**: Done
- **Description**: Automated build and deploy workflow for GitHub Pages
- **Details**:
  - Create `.github/workflows/hugo.yml`
  - Install Hugo on push to main
  - Build static site
  - Deploy to GitHub Pages

---

## [FEAT-09] Done Categories and Tags
- **Status**: Done
- **Description**: Taxonomy pages for organizing posts by category and tag
- **Details**:
  - Enable Hugo taxonomies in config
  - Create taxonomy templates
  - Add category/tag links to posts

---

## [FEAT-10] Done RSS Feed
- **Status**: Done
- **Description**: RSS feed for post syndication
- **Details**:
  - Hugo auto-generates RSS by default
  - Customize feed template if needed
  - Add RSS link to site header

---

## [FEAT-11] Done Search
- **Status**: Done
- **Description**: Client-side search functionality for posts
- **Details**:
  - Generate search index JSON at build time via Hugo output format (`layouts/index.json`)
  - Add search input to header in `baseof.html` with dropdown quick results
  - Dedicated search page at `/search/` with full results view
  - Vanilla JS scoring by title, tags, summary, and full content
  - Search styles in `static/css/style.css`
  - `buildFuture: true` in config to index all content regardless of dates
  - JS resolves search index URL from `data-search-index` attribute (supports sub-path deployment)
  - Debounced input, keyboard navigation, URL query parameter support

---

## [FEAT-12] Done Dark Light Toggle
- **Status**: Done
- **Description**: Theme switcher for dark and light modes
- **Details**:
  - Add toggle button to header in `baseof.html`
  - `[data-theme="light"]` CSS block redefining all 9 color variables to light values
  - FOUC-prevention via synchronous inline `&lt;script&gt;` in `<head>` reading localStorage then `prefers-color-scheme`
  - `theme.js` handles toggle click, updates `data-theme`, saves to localStorage
  - Smooth transitions on `color`, `background-color`, and `border-color` (respects `prefers-reduced-motion`)
  - Theme toggle button with sun/moon Unicode icons

---

## [FEAT-13] Done Content Structure Mapping
- **Status**: Done
- **Description**: Configure Hugo to read from content repository's directory structure
- **Details**:
  - Replace `contentDir` with `module.mounts` mapping `10 Posts/` → `content/posts` section
  - Rename post content files from `slug.md` → `index.md` for Hugo leaf bundle compatibility
  - Clean URLs via `slug` frontmatter + `permalinks: posts: "/:slug/"`
  - `published` frontmatter mapped to `.Date` via `frontmatter: { date: [published, date] }`
  - Date-nested sections (`YYYY/`, `YYYY-MM/`, `YYYY-MM-DD/`) suppressed via `build: { list: never, render: never }`
  - 5 type-specific post templates created: `blog`, `microblog`, `literary`, `media`, `resources` (all start identical to `_default/single.html`)
  - `post-type` taxonomy via `type` frontmatter field
  - Search page moved from content repo to `eveningrain/content/search/_index.md`
  - `75 Templates/blog_template.md` updated with `slug`, `type`, and ISO date format

---

## Content Structure Reference

### Source: `/eveningrain-contents/10 Posts/`

```
10 Posts/
├── YYYY/
│   ├── YYYY-MM/
│   │   ├── YYYY-MM-DD/
│   │   │   └── YYYY-MM-DD-hh-mm-ss_post-type_post-title/
│   │   │       ├── YYYY-MM-DD-hh-mm-ss_post-type_post-title.md
│   │   │       └── assettype_asset-title.img
```

### Post Frontmatter

```yaml
---
published: YYYY-MM-DD-hh-mm-ss
title: Post Title
tags: [tag-1, tag-2]
author: $author
---
```

### Site Metadata (`/eveningrain-contents/70 Metadata/metadata.yaml`)

```yaml
title: "Evening Rain"
author: "Muhammad Mustafa Monowar"
copyright: "© 2026 Muhammad Mustafa Monowar All rights reserved"
```

### Post Types

| Type | Directory Prefix | Description |
|------|------------------|-------------|
| blog | `_blog_` | Long-form blog posts |
| microblog | `_microblog_` | Short-form microblogs |
| literary | `_literary_` | Literary work (poetry, fiction) |
| media | `_media_` | Media posts (images, video) |
| resources | `_resources_` | Resource collections |

### Asset Naming

| Asset Type | Format | Example |
|------------|--------|---------|
| Image | `image_title.jpg` | `image_sunset.jpg` |
| Video | `video_title.mp4` | `video_rain.mp4` |
| Document | `document_title.pdf` | `document_notes.pdf` |
