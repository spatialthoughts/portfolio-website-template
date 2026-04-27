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

See **[SETUP.md](SETUP.md)** for the full step-by-step guide

## Optional Components

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
