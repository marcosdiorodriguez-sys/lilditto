# Deployment Prep Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Get the LIL DITTO static site live on Vercel with a GitHub-connected repo, correct SEO URLs, and crawler files.

**Architecture:** Pure static site (single `index.html` + assets). No build step. Vercel serves the repo root directly. GitHub is the source of truth; every push to `main` triggers an auto-deploy.

**Tech Stack:** HTML/CSS/JS, Git, GitHub CLI (`gh`), Vercel CLI (`vercel`)

---

### Task 1: Create `.gitignore`

**Files:**
- Create: `.gitignore`

- [ ] **Step 1: Create the file**

```
node_modules/
.DS_Store
Thumbs.db
*.log
```

Write this to `.gitignore` at the repo root.

- [ ] **Step 2: Verify**

```bash
cat .gitignore
```

Expected output:
```
node_modules/
.DS_Store
Thumbs.db
*.log
```

---

### Task 2: Create `robots.txt`

**Files:**
- Create: `robots.txt`

- [ ] **Step 1: Create the file**

Write to `robots.txt` at the repo root:
```
User-agent: *
Allow: /
Sitemap: https://PLACEHOLDER.vercel.app/sitemap.xml
```

- [ ] **Step 2: Verify**

```bash
cat robots.txt
```

Expected: file contains the three lines above with `PLACEHOLDER` in the sitemap URL.

---

### Task 3: Create `sitemap.xml`

**Files:**
- Create: `sitemap.xml`

- [ ] **Step 1: Create the file**

Write to `sitemap.xml` at the repo root:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/0.1">
  <url>
    <loc>https://PLACEHOLDER.vercel.app/</loc>
    <lastmod>2026-04-25</lastmod>
    <changefreq>monthly</changefreq>
    <priority>1.0</priority>
  </url>
</urlset>
```

- [ ] **Step 2: Verify**

```bash
cat sitemap.xml
```

Expected: valid XML with `PLACEHOLDER` in the `<loc>` URL.

---

### Task 4: Initial commit of all project files

**Files:**
- All untracked files in the repo

- [ ] **Step 1: Check what will be staged**

```bash
git status
```

Expected: untracked files including `index.html`, `album_covers/`, `band_images/`, `logo/`, `album_info/`, `CLAUDE.md`, `band_info_LILDITTO.txt`, `PRD_LILDITTO.pdf`, `Engineering specifications_LILDITTO.pdf`, `.gitignore`, `robots.txt`, `sitemap.xml`.

- [ ] **Step 2: Stage everything**

```bash
git add .
```

- [ ] **Step 3: Verify staging**

```bash
git status
```

Expected: all files listed under "Changes to be committed". No untracked files remaining.

- [ ] **Step 4: Commit**

```bash
git commit -m "Initial commit — LIL DITTO static site"
```

Expected output includes: `main ... N files changed`

- [ ] **Step 5: Verify commit**

```bash
git log --oneline
```

Expected: 2 commits — the spec doc commit and this new one at the top.

---

### Task 5: Create GitHub repo and push

**Files:**
- No file changes — git remote configuration only

- [ ] **Step 1: Check GitHub auth**

```bash
gh auth status
```

Expected: `Logged in to github.com as <username>`. If not logged in, run `gh auth login` and follow the prompts.

- [ ] **Step 2: Create public repo and push**

```bash
gh repo create lilditto --public --source=. --remote=origin --push
```

Expected output includes:
```
✓ Created repository <username>/lilditto on GitHub
✓ Added remote https://github.com/<username>/lilditto.git
✓ Pushed commits to https://github.com/<username>/lilditto.git
```

- [ ] **Step 3: Verify remote**

```bash
git remote -v
```

Expected:
```
origin  https://github.com/<username>/lilditto.git (fetch)
origin  https://github.com/<username>/lilditto.git (push)
```

- [ ] **Step 4: Confirm on GitHub**

```bash
gh repo view lilditto --web
```

Expected: browser opens to the repo page showing all committed files.

---

### Task 6: Connect to Vercel and note subdomain

**Files:**
- No file changes — Vercel project configuration only

- [ ] **Step 1: Install Vercel CLI if needed**

```bash
npm install -g vercel
```

Expected: installs silently. If already installed, outputs current version.

- [ ] **Step 2: Log in to Vercel**

```bash
vercel login
```

Follow the browser-based auth flow. Expected on completion: `Success! Email <address> confirmed.`

- [ ] **Step 3: Deploy**

```bash
vercel
```

When prompted:
- **Set up and deploy?** → `Y`
- **Which scope?** → select your personal account
- **Link to existing project?** → `N`
- **Project name?** → `lilditto` (or accept default)
- **In which directory is your code located?** → `.` (current directory)
- **Want to modify these settings?** → `N`

Expected on completion: a line like:
```
✅ Production: https://lilditto-xxxx.vercel.app
```

- [ ] **Step 4: Record the subdomain**

Copy the full URL from the output (e.g. `lilditto-xxxx.vercel.app`). You will need this in Task 7.

- [ ] **Step 5: Connect GitHub repo for auto-deploy**

Go to https://vercel.com/dashboard → open the `lilditto` project → Settings → Git → Connect Git Repository → select `<username>/lilditto`. From this point every push to `main` triggers a redeploy.

---

### Task 7: Replace placeholder URLs with real subdomain

**Files:**
- Modify: `index.html`
- Modify: `robots.txt`
- Modify: `sitemap.xml`

Replace `PLACEHOLDER` (in `robots.txt` and `sitemap.xml`) and `lilditto.com` (in `index.html`) with the real Vercel subdomain noted in Task 6. Use the full hostname only — e.g. `lilditto-xxxx.vercel.app`.

- [ ] **Step 1: Update `index.html` — canonical**

Find:
```html
<link rel="canonical" href="https://lilditto.com/">
```
Replace with:
```html
<link rel="canonical" href="https://lilditto-xxxx.vercel.app/">
```

- [ ] **Step 2: Update `index.html` — og:url**

Find:
```html
<meta property="og:url"         content="https://lilditto.com/">
```
Replace with:
```html
<meta property="og:url"         content="https://lilditto-xxxx.vercel.app/">
```

- [ ] **Step 3: Update `index.html` — og:image**

Find:
```html
<meta property="og:image"       content="https://lilditto.com/band_images/hero1.png">
```
Replace with:
```html
<meta property="og:image"       content="https://lilditto-xxxx.vercel.app/band_images/hero1.png">
```

- [ ] **Step 4: Update `index.html` — twitter:image**

Find:
```html
<meta name="twitter:image"       content="https://lilditto.com/band_images/hero1.png">
```
Replace with:
```html
<meta name="twitter:image"       content="https://lilditto-xxxx.vercel.app/band_images/hero1.png">
```

- [ ] **Step 5: Update `robots.txt`**

Replace:
```
Sitemap: https://PLACEHOLDER.vercel.app/sitemap.xml
```
With:
```
Sitemap: https://lilditto-xxxx.vercel.app/sitemap.xml
```

- [ ] **Step 6: Update `sitemap.xml`**

Replace:
```xml
<loc>https://PLACEHOLDER.vercel.app/</loc>
```
With:
```xml
<loc>https://lilditto-xxxx.vercel.app/</loc>
```

- [ ] **Step 7: Update `index.html` — JSON-LD structured data**

Find:
```json
"url": "https://lilditto.com",
```
Replace with:
```json
"url": "https://lilditto-xxxx.vercel.app",
```

- [ ] **Step 8: Also update the YouTube embed origin in `index.html`**

Find:
```html
src="https://www.youtube.com/embed/DFD9elv1JUs?origin=https://lilditto.com"
```
Replace with:
```html
src="https://www.youtube.com/embed/DFD9elv1JUs?origin=https://lilditto-xxxx.vercel.app"
```

- [ ] **Step 9: Commit and push**

```bash
git add index.html robots.txt sitemap.xml
git commit -m "Update URLs to Vercel subdomain"
git push
```

Expected: push succeeds and Vercel dashboard shows a new deployment triggered.

---

### Task 8: Verify live deployment

- [ ] **Step 1: Confirm site loads**

Open `https://lilditto-xxxx.vercel.app` in a browser. Expected: LIL DITTO site loads with hero image, all sections visible.

- [ ] **Step 2: Confirm `robots.txt` is accessible**

```bash
curl https://lilditto-xxxx.vercel.app/robots.txt
```

Expected:
```
User-agent: *
Allow: /
Sitemap: https://lilditto-xxxx.vercel.app/sitemap.xml
```

- [ ] **Step 3: Confirm `sitemap.xml` is accessible**

```bash
curl https://lilditto-xxxx.vercel.app/sitemap.xml
```

Expected: the XML file with the correct `<loc>` URL.

- [ ] **Step 4: Confirm canonical URL in page source**

```bash
curl -s https://lilditto-xxxx.vercel.app | grep canonical
```

Expected:
```html
<link rel="canonical" href="https://lilditto-xxxx.vercel.app/">
```

- [ ] **Step 5: Confirm auto-deploy is wired**

Make a trivial change (e.g. add a space and remove it in `index.html`), commit and push. Check the Vercel dashboard — a new deployment should appear within 30 seconds.

```bash
git push
```

Then visit https://vercel.com/dashboard and confirm a new deployment is in progress.
