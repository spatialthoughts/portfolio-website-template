# Geospatial Portfolio Template

A ready-to-use portfolio website template for geospatial professionals — GIS analysts, remote
sensing specialists, spatial data scientists, and GeoAI practitioners. Built with
[MkDocs](https://www.mkdocs.org/) and the [Material theme](https://squidfunk.github.io/mkdocs-material/).

Build a fully responsive personalized portfolio website - no Git or coding expertise required.

**[TEMPLATE PREVIEW](https://spatialthoughts.github.io/portfolio-website-template/)**

**[LIVE PORTFOLIO](https://spatialthoughts.github.io/)**

This template is part of our [Building Your Geospatial Portfolio Website](https://courses.spatialthoughts.com/geospatial-portfolio-workshop.html) workshop. Visit the workshop page for step-by-step instructions.

---

## What's Included

- Professional theme with a mobile-responsive layout
- Homepage with profile photo, skill cards, about section and a CV download button.
- Pre-configured pages - publications, experience and contact. With the flexibility to add/remove pages as needed.
- Project gallery with individual project pages with example projects as both
  a Markdown page and a Jupyter notebook.
- Pre-configured GitHub Action to auto-deploy site GitHub Pages on every push to `main`
- Light/dark mode toggle
- Optional support for blog and comments.

See **[SETUP.md](SETUP.md)** for the full step-by-step guide

## Optional Components

The template is pre-wired for two optional features that are disabled by default.
Enable either one by following the steps below.

### Adding a Blog with Giscus Comments

Adds a `/blog` section for writing about projects, tutorials, or field notes.
Comments on posts are powered by [Giscus](https://giscus.app/) (GitHub Discussions — free,
no third-party tracking).

**Step 1 — Enable GitHub Discussions in your repo**

Go to your repo on GitHub → Settings → General → Features → check **Discussions**.

**Step 2 — Enable the blog plugin in `mkdocs.yml`**

Open `mkdocs.yml` and uncomment these two lines:

```yaml
plugins:
  - search
  - blog        # <-- uncomment this line

nav:
  ...
  - Blog: blog/index.md    # <-- uncomment this line
```

**Step 3 — Create the blog directory**

Create the folder `docs/blog/` and add a file `docs/blog/.authors.yml`:

```yaml
authors:
  [YOUR-GITHUB-USERNAME]:
    name: [YOUR NAME]
    description: Author
```

Create your first post at `docs/blog/posts/my-first-post.md`:

```markdown
---
date: 2024-01-01
authors:
  - [YOUR-GITHUB-USERNAME]
comments: true
---

# My First Post

Content goes here.
```

**Step 4 — Configure Giscus comments**

1. Go to [https://giscus.app](https://giscus.app), fill in your GitHub repo details,
   and copy the generated `<script>` tag.
2. Open `overrides/partials/comments.html`.
3. Replace the placeholder `<script>` tag with your actual one from giscus.app.
4. Remove the `<!--` and `-->` comment markers around the script block.

---

### Adding Jupyter Notebook Projects

Jupyter notebook support is included by default via `mkdocs-jupyter`. Drop your `.ipynb`
file into `docs/projects/` and add it to the nav in `mkdocs.yml` exactly like any other
page:

```yaml
nav:
  - Projects:
    - "All Projects": projects/index.md
    - "My Notebook Analysis": projects/my-analysis.ipynb
```

Cell outputs (maps, charts, tables) render inline on the published site.

---

## License

[MIT License](LICENSE)
