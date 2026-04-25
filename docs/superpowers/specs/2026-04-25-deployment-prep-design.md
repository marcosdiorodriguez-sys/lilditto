# Deployment Prep Design

**Date:** 2026-04-25  
**Project:** LIL DITTO — Official Site  
**Approach:** Standard (B)

## Overview

Prepare the static LIL DITTO site for production deployment on Vercel via a GitHub-connected repo. The site is a single `index.html` with local assets — no build step, no framework.

## Phase 1 — Files to Create

Three files added to the repo root before the initial commit.

### `.gitignore`
Excludes OS and tooling artifacts:
```
node_modules/
.DS_Store
Thumbs.db
*.log
```

### `robots.txt`
Allows all crawlers and references the sitemap. URL is a placeholder until the Vercel subdomain is known:
```
User-agent: *
Allow: /
Sitemap: https://<vercel-subdomain>.vercel.app/sitemap.xml
```

### `sitemap.xml`
Single URL entry for the one-page site. URL is a placeholder until the Vercel subdomain is known:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/0.1">
  <url>
    <loc>https://<vercel-subdomain>.vercel.app/</loc>
    <lastmod>2026-04-25</lastmod>
    <changefreq>monthly</changefreq>
    <priority>1.0</priority>
  </url>
</urlset>
```

## Phase 2 — Git + GitHub Setup

1. Stage all project files: `index.html`, all asset directories (`album_covers/`, `band_images/`, `logo/`, `album_info/`), reference docs (`CLAUDE.md`, `PRD_LILDITTO.pdf`, `Engineering specifications_LILDITTO.pdf`, `band_info_LILDITTO.txt`), and the three new files.
2. Create initial commit.
3. Create public GitHub repo named `lilditto` via `gh repo create`.
4. Set as remote origin and push `main`.

**Visibility:** Public repo required for Vercel free-tier auto-deploy on push.

## Phase 3 — Vercel Connection + URL Update

1. Run `vercel` CLI in the project root. Configuration:
   - Framework: None (static)
   - Build command: None
   - Output directory: `.`
2. Note the assigned subdomain (e.g. `lilditto.vercel.app`).
3. Update the following placeholders in `index.html`:
   - `<link rel="canonical" href="...">`
   - `<meta property="og:url" content="...">`
   - `<meta property="og:image" content="...">`
   - `<meta name="twitter:image" content="...">`
4. Update placeholder URLs in `robots.txt` and `sitemap.xml`.
5. Commit and push — Vercel auto-deploys.

## Success Criteria

- Site is live at a `*.vercel.app` URL
- All canonical/OG/sitemap URLs point to the real subdomain
- `robots.txt` and `sitemap.xml` are accessible at the root
- Vercel auto-deploys on every push to `main`
