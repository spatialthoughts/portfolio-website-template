# Portfolio Website Template — Build Plan

## Goal

A MkDocs + Material theme portfolio template for geospatial professionals (GIS, remote sensing, data science, GeoAI). Designed for students to fork and customize. Beginner-friendly setup with conda, GitHub Actions auto-deploy to GitHub Pages.

---

## Site Structure

Multi-page site. Each page is a separate Markdown file students edit directly.

```
portfolio-website-template/
├── docs/
│   ├── index.md                  # Home / About
│   ├── projects/
│   │   ├── index.md              # Projects gallery (card grid)
│   │   └── sample-project.md    # Example project page with Leaflet map
│   ├── skills.md                 # Technical skills (simple lists)
│   ├── experience.md             # Work experience + education + certifications + publications
│   ├── contact.md                # Contact info, social links, CV download
│   └── assets/
│       ├── images/
│       │   ├── profile.jpg       # Placeholder profile photo (to be replaced)
│       │   └── sample-project.jpg
│       └── css/
│           └── extra.css         # Minor style tweaks (profile photo rounding, etc.)
├── overrides/
│   └── partials/
│       └── comments.html         # Giscus comment script (commented out by default)
├── mkdocs.yml
├── environment.yml               # Conda environment for local development
├── requirements.txt              # Pip packages (used by GitHub Actions)
├── .github/
│   └── workflows/
│       └── deploy.yml            # Auto-deploy on push to main
├── SETUP.md                      # Step-by-step setup guide for beginners
└── README.md                     # Overview + optional components instructions
```

---

## Page-by-Page Content Plan

### `docs/index.md` — Home / About

The landing page. Sections:

1. **Profile photo** — centered, rounded, using a simple HTML/CSS snippet students replace
2. **Name + tagline** — e.g. `[YOUR NAME] | GIS Analyst & Remote Sensing Specialist`
3. **Short bio** — 3–4 sentence paragraph placeholder
4. **Social links** — GitHub, LinkedIn, email (using Material icons in a simple list)
5. **Highlights row** — three small stat cards: `X Projects`, `Y Years Experience`, `Z Skills` (uses Material grid cards — students just edit the numbers)
6. **CV download button** — links to a PDF in `docs/assets/`

---

### `docs/projects/index.md` — Projects Gallery

A grid of project cards. Uses Material for MkDocs card grid syntax (works with the `md_in_html` extension — no plugins needed). Each card shows:

- Project thumbnail image
- Title
- One-line description
- Tool/skill tags (as inline code badges)
- Link to the project detail page

Template ships with **one filled-in sample card** and **two clearly marked placeholder cards** so students understand the pattern immediately.

---

### `docs/projects/sample-project.md` — Example Project Page

The main "copy this" template for individual projects. Includes:

1. **Hero image** — full-width project screenshot
2. **Overview** — what the project does, why it matters
3. **Leaflet map** — embedded via a raw HTML block in Markdown. Pattern:
   - Leaflet CSS/JS loaded via CDN entries in `mkdocs.yml` (`extra_css`, `extra_javascript`)
   - Map div + `<script>` block inline in the `.md` file
   - Example shows a simple tile layer + a few GeoJSON markers over a sample study area
   - Comment in the file explains exactly which lines to change (coordinates, zoom, markers)
4. **Methods & Tools** — bulleted list of software, libraries, data sources
5. **Key Findings** — brief results section
6. **Links** — GitHub repo, data source, live demo

---

### `docs/skills.md` — Technical Skills

Simple Markdown lists, grouped by category. No charts or bars — just clean readable lists students edit in place.

Categories:
- GIS Software (QGIS, ArcGIS Pro, Google Earth Engine, GDAL)
- Programming (Python, R, JavaScript)
- Remote Sensing (Google Earth Engine, Cloud Native Geospatial)
- Drone Data Processing (Pix4D, Agisoft, ODM)
- Machine Learning / GeoAI (scikit-learn, PyTorch, SAM)
- Data & Cloud (SQL, PostGIS, PostgreSQL, AWS S3, Google Cloud)
- Web Mapping (Leaflet, Folium, Maplibre)

Each category uses a Material `admonition` block (collapsible) so the page stays clean even with many skills.

---

### `docs/experience.md` — Experience & Education

Four sections on one page:

1. **Work Experience** — job entries with company, title, dates, 2–3 bullet points
2. **Education** — degree, institution, year, optionally a thesis title
3. **Certifications** — simple list (ESRI, Google, AWS, Coursera, etc.)
4. **Publications & Presentations** — simple numbered list; can be left empty

Each section header is a `##` heading so MkDocs auto-generates an in-page table of contents.

---

### `docs/contact.md` — Contact

Simple page with:

- Email (using a `mailto:` link — no form, no server required)
- LinkedIn URL
- GitHub URL
- Twitter/X URL (optional, clearly marked)
- CV download button (same as home page — links to PDF in `docs/assets/`)

Short note to students: "Replace the placeholder links. Delete any rows you don't use."

---

## Optional Components

Both optional features are pre-wired in the template but disabled by default. Enabling each one requires only small, documented changes — no restructuring.

### Option A: Blog with Giscus Comments

**What it adds:** A `/blog` section where students can write posts about their work, with GitHub Discussions-powered comments at the bottom of each post.

**How it works:**
- The Material blog plugin is built into `mkdocs-material` — no extra package to install
- Giscus uses GitHub Discussions as a comment backend. It requires the portfolio repo to have Discussions enabled (one checkbox in repo Settings)
- The comment injection point is `overrides/partials/comments.html`, which Material automatically appends to blog posts when the blog plugin is active

**What ships in the template (disabled):**
- `overrides/partials/comments.html` — contains the giscus `<script>` tag, fully commented out with instructions
- `mkdocs.yml` — has a commented-out `blog` plugin entry and a commented-out `Blog` nav item
- `custom_dir: overrides` is always active in `mkdocs.yml` so the directory is registered from day one

**Enabling steps (3 steps, documented in README):**
1. Enable GitHub Discussions in the repo (Settings → General → Features → Discussions)
2. In `mkdocs.yml`: uncomment the `blog` plugin and the `Blog` nav entry
3. In `overrides/partials/comments.html`: uncomment the script block and fill in your GitHub username and repo name

---

### Option B: Jupyter Notebook Projects

**What it adds:** Students can add `.ipynb` notebook files as project pages directly — no manual conversion needed.

**How it works:**
- `mkdocs-jupyter` plugin converts notebooks to MkDocs pages at build time
- A notebook at `docs/projects/my-analysis.ipynb` works exactly like `docs/projects/my-analysis.md`
- Cell outputs (maps, charts, tables) render inline

**What ships in the template (disabled):**
- `requirements.txt` — has `mkdocs-jupyter` as a commented-out line
- `environment.yml` — same
- `mkdocs.yml` — has a commented-out `mkdocs-jupyter` plugin entry

**Enabling steps (3 steps, documented in README):**
1. In `requirements.txt` and `environment.yml`: uncomment the `mkdocs-jupyter` line
2. Run `pip install mkdocs-jupyter` (or `conda env update -f environment.yml`)
3. In `mkdocs.yml`: uncomment the `mkdocs-jupyter` plugin entry
4. Drop `.ipynb` files into `docs/projects/` and add them to the nav in `mkdocs.yml` like any other page

---

## MkDocs Configuration (`mkdocs.yml`)

Key settings:

```yaml
theme:
  name: material
  custom_dir: overrides      # needed for giscus comments override
  palette:
    - scheme: default        # light mode
      primary: teal
      accent: deep orange
    - scheme: slate          # dark mode toggle
      primary: teal
      accent: deep orange
  features:
    - navigation.tabs        # top-level nav as tabs
    - navigation.top         # back-to-top button
    - content.code.copy      # copy button on code blocks
  font:
    text: Roboto
    code: Roboto Mono

extra_css:
  - assets/css/extra.css
  - https://unpkg.com/leaflet@1.9.4/dist/leaflet.css

extra_javascript:
  - https://unpkg.com/leaflet@1.9.4/dist/leaflet.js

markdown_extensions:
  - md_in_html               # needed for card grids and HTML blocks in markdown
  - attr_list
  - admonition
  - pymdownx.details         # collapsible admonitions
  - pymdownx.superfences
  - tables

plugins:
  - search
  # OPTIONAL: Uncomment to enable blog (see README for full steps)
  # - blog
  # OPTIONAL: Uncomment to enable Jupyter notebook pages (see README for full steps)
  # - mkdocs-jupyter
```

Color palette is easy to change — one line in `mkdocs.yml`. Comment in the file lists the Material color names.

---

## Conda Environment (`environment.yml`)

```yaml
name: portfolio
channels:
  - conda-forge
  - defaults
dependencies:
  - python=3.11
  - pip
  - pip:
    - mkdocs-material>=9.5
```

Kept minimal. Only one real dependency.

---

## Requirements File (`requirements.txt`)

```
mkdocs-material>=9.5
```

Used by GitHub Actions (pip install is faster in CI than conda).

---

## GitHub Actions Workflow (`.github/workflows/deploy.yml`)

Trigger: every push to `main`.

Steps:
1. Checkout repo
2. Set up Python 3.11
3. `pip install -r requirements.txt`
4. `mkdocs gh-deploy --force`

The `gh-deploy` command builds the site and pushes to the `gh-pages` branch automatically. No extra configuration needed after the one-time GitHub Pages setup.

---

## Setup Guide (`SETUP.md`)

Written for someone who has never used git or the command line. Sections:

### Part 1 — Fork the template
- Step-by-step screenshots description for forking on GitHub
- How to rename the repo to `your-username.github.io` for a root GitHub Pages URL, or keep any name for a project page URL

### Part 2 — Enable GitHub Pages
- Settings → Pages → Source: Deploy from branch → `gh-pages`
- Note: the `gh-pages` branch is created automatically on first push — students don't need to create it

### Part 3 — Edit your content (GitHub web editor)
- Explains that for simple edits (text changes), students can edit `.md` files directly in the GitHub web UI without installing anything
- Lists which files to edit and in what order

### Part 4 — Local development (optional but recommended)
- Install conda (links to Anaconda installer for Mac/Windows/Linux)
- Install git and authenticate
- `git clone` the repo
- `conda env create -f environment.yml`
- `conda activate portfolio`
- `mkdocs serve` — opens live preview at `http://127.0.0.1:8000`
- Edit a file, browser auto-reloads

### Part 5 — Adding images
- Drop image files into `docs/assets/images/`
- Reference them in Markdown as `![Alt text](../assets/images/your-image.jpg)`
- For profile photo: replace `docs/assets/images/profile.jpg` with your photo (keep the same filename)

### Part 6 — Adding a new project
- Copy `docs/projects/sample-project.md` → rename to `docs/projects/your-project-name.md`
- Edit the content
- Add a card to `docs/projects/index.md`
- Add the page to the `nav:` section of `mkdocs.yml`

### Part 7 — Customizing colors and site name
- Edit `mkdocs.yml`: change `site_name`, `site_author`, `site_url`
- Change color: edit the `primary` value (list of valid color names provided inline)

### Part 8 — Deploying
- `git add .` → `git commit -m "update portfolio"` → `git push`
- GitHub Actions runs automatically, site is live within ~2 minutes
- Link to find the live URL (Settings → Pages)

---

## Placeholder Strategy

Every piece of content that must be replaced is marked with `[YOUR ...]` in all caps. Examples:

- `[YOUR NAME]`
- `[YOUR JOB TITLE]`
- `[YOUR LINKEDIN URL]`
- `[YOUR PROJECT TITLE]`

At the top of each file, a commented-out checklist reminds the student what to replace on that page:

```markdown
<!--
CHECKLIST FOR THIS PAGE:
- [ ] Replace [YOUR NAME] with your name
- [ ] Replace profile.jpg with your photo
- [ ] Update the bio paragraph
- [ ] Update the social links
-->
```

---

## README Content Plan

`README.md` is the first thing students see on GitHub. Sections:

1. **What this is** — one paragraph: MkDocs Material portfolio template for geospatial professionals
2. **Live demo link** — placeholder URL for the instructor's deployed example
3. **Quick start** — 3-line summary pointing to SETUP.md
4. **What's included** — bullet list of pages and features
5. **Optional Components** — two subsections:

   **Adding a Blog with Comments (Giscus)**
   - Enable GitHub Discussions in repo settings
   - Uncomment `blog` plugin and nav entry in `mkdocs.yml`
   - Uncomment and configure giscus script in `overrides/partials/comments.html`
   - Link to the giscus.app config generator for getting the script tag

   **Adding Jupyter Notebook Projects**
   - Uncomment `mkdocs-jupyter` in `requirements.txt` and `environment.yml`
   - Run `pip install mkdocs-jupyter` or `conda env update -f environment.yml`
   - Uncomment the plugin in `mkdocs.yml`
   - Drop `.ipynb` files into `docs/projects/`, add to nav in `mkdocs.yml`

6. **License** — link to LICENSE file

---

## Implementation Steps

1. Create `mkdocs.yml` with full configuration
2. Create `docs/assets/css/extra.css` (profile photo styling, minor tweaks)
3. Create `docs/index.md` — Home page with all placeholders
4. Create `docs/projects/index.md` — Projects gallery with card grid
5. Create `docs/projects/sample-project.md` — Example project with Leaflet map
6. Create `docs/skills.md`
7. Create `docs/experience.md`
8. Create `docs/contact.md`
9. Add placeholder images to `docs/assets/images/`
10. Create `environment.yml` and `requirements.txt`
11. Create `.github/workflows/deploy.yml`
12. Create `SETUP.md` — beginner-friendly setup guide
13. Update `README.md` — overview, link to SETUP.md, and a dedicated "Optional Components" section covering blog+giscus and Jupyter notebook steps
14. Test locally with `mkdocs serve`
15. Verify GitHub Actions workflow YAML is valid
