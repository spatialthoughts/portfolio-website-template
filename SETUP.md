# Setting Up Your Portfolio

This guide walks you through setting up your portfolio from scratch.
No prior experience with git or the command line is required.

---

## Part 1 — Fork the Template

Forking creates your own copy of the template that you can edit freely.

1. Make sure you are logged in to [GitHub](https://github.com).
2. Go to the template repository page.
3. Click the **Fork** button in the top-right corner.
4. On the "Create a new fork" page:
   - **Repository name:** To use GitHub Pages for free at `https://your-username.github.io`,
     name the repo exactly `your-username.github.io` (replace `your-username` with your actual
     GitHub username). Otherwise, any name works and the site will be at
     `https://your-username.github.io/your-repo-name`.
   - Leave all other options as-is.
5. Click **Create fork**.

You now have your own copy of the template at `https://github.com/your-username/your-repo-name`.

---

## Part 2 — Enable GitHub Pages

GitHub Pages hosts your site for free. It needs to be turned on once.

1. Go to your forked repository on GitHub.
2. Click **Settings** (in the top menu of your repo).
3. In the left sidebar, click **Pages**.
4. Under **Build and deployment**, set the Source to **Deploy from a branch**.
5. Set the Branch to **gh-pages** and the folder to **/ (root)**.
6. Click **Save**.

> Note: The `gh-pages` branch does not exist yet — it will be created automatically
> the first time you push a change to `main`. After that, your site will be live at the URL
> shown on the Pages settings page.

---

## Part 3 — Edit Your Content (No Local Install Needed)

For simple text changes, you can edit files directly in the GitHub web interface —
no software installation required.

**Which files to edit, in order:**

| File | What to change |
|------|---------------|
| `mkdocs.yml` | Site name, your name, GitHub repo URL, colors |
| `docs/index.md` | Profile photo, bio, social links, highlights |
| `docs/projects/index.md` | Project cards (titles, descriptions, images) |
| `docs/projects/sample-project.md` | The example project (or copy it for your own) |
| `docs/skills.md` | Your actual skills |
| `docs/experience.md` | Your work history, education, certifications |
| `docs/contact.md` | Your email, GitHub, LinkedIn links |

**How to edit a file on GitHub:**

1. Navigate to the file in your repo.
2. Click the pencil icon (**Edit this file**) in the top-right of the file view.
3. Make your changes.
4. Click **Commit changes** at the top-right, add a short message, and click **Commit changes** again.
5. GitHub Actions will automatically build and deploy your site within about 2 minutes.

---

## Part 4 — Local Development (Optional but Recommended)

Local development lets you preview changes instantly before pushing to GitHub.
This requires installing a few tools.

### 4a — Install Git

- **Windows:** Download from [git-scm.com](https://git-scm.com/download/win).
  During install, accept all defaults.
- **macOS:** Open Terminal and run `git --version`. If not installed, macOS will prompt you to install it.
- **Linux:** `sudo apt install git` (Ubuntu/Debian) or `sudo dnf install git` (Fedora).

After installing, set your name and email (run these in Terminal / Git Bash):

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

### 4b — Install Conda (Miniforge)

Conda manages the Python environment. We recommend Miniforge (smaller, faster than Anaconda).

1. Download the installer for your system from [conda-forge.org/miniforge](https://conda-forge.org/miniforge/).
2. Run the installer. Accept all defaults.
3. Open a new Terminal (or Anaconda Prompt on Windows) to make sure conda is available.

### 4c — Clone Your Repository

Replace `your-username` and `your-repo-name` with your actual values:

```bash
git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name
```

### 4d — Create the Environment

```bash
conda env create -f environment.yml
conda activate portfolio
```

### 4e — Start the Local Preview Server

```bash
mkdocs serve
```

Open your browser and go to [http://127.0.0.1:8000](http://127.0.0.1:8000).
The page reloads automatically whenever you save a file.

Press `Ctrl+C` in the terminal to stop the server.

---

## Part 5 — Adding and Replacing Images

### Replace the profile photo

1. Prepare your photo (square crop works best, at least 300×300 pixels).
2. Name the file `profile.png` — or keep your existing filename and update the reference in
   `docs/index.md` to match.
3. Upload it to `docs/assets/images/` by navigating there on GitHub and clicking
   **Add file → Upload files**.

### Add a project image

1. Prepare your image (landscape orientation, at least 800×300 pixels recommended).
2. Upload it to `docs/assets/images/`.
3. Reference it in your project page:
   ```markdown
   ![Description of image](../assets/images/your-image-name.jpg)
   ```
   Note the `../` — project pages are in a subfolder, so the path goes up one level first.

### Add your CV/Resume PDF

1. Save your CV as a PDF (name it something like `Jane-Doe-CV.pdf`).
2. Upload it to `docs/assets/`.
3. In `docs/index.md` and `docs/contact.md`, update the CV download button link to match:
   ```markdown
   [Download CV](assets/Jane-Doe-CV.pdf){ .md-button }
   ```

---

## Part 6 — Adding a New Project

1. **Copy the sample project file:**
   Duplicate `docs/projects/sample-project.md` and rename it
   (e.g., `docs/projects/flood-risk-mapping.md`).

2. **Edit the new file:**
   Work through the checklist at the top of the file and fill in your project content.
   Update the Leaflet map coordinates to your actual study area.

3. **Add a card to the Projects gallery:**
   Open `docs/projects/index.md` and copy one of the existing `<div class="project-card">` blocks.
   Update the image path, title, description, tools, and link.

4. **Add the page to the navigation:**
   Open `mkdocs.yml` and add a line under the Projects section:
   ```yaml
   nav:
     - Projects:
       - "All Projects": projects/index.md
       - "Land Cover Classification": projects/sample-project.md
       - "Flood Risk Mapping": projects/flood-risk-mapping.md   # <-- add this
   ```

---

## Part 7 — Customizing Colors and Site Name

Open `mkdocs.yml` and edit:

- `site_name` — shown in the browser tab and site header
- `site_author` — used in page metadata
- `site_url` — your GitHub Pages URL (update after deploying)
- `repo_url` and `repo_name` — your GitHub repository

To change the color scheme, edit the `primary` value under `palette`:

```yaml
palette:
  - scheme: default
    primary: indigo     # change this
    accent: amber       # and this
```

Available colors: `red`, `pink`, `purple`, `deep purple`, `indigo`, `blue`, `light blue`,
`cyan`, `teal`, `green`, `light green`, `lime`, `yellow`, `amber`, `orange`, `deep orange`.

---

## Part 8 — Deploying Your Changes

Every time you push a change to the `main` branch, GitHub Actions automatically builds
and deploys your site. You do not need to do anything extra.

**Using the GitHub web editor** (no local setup needed):

Just edit a file and click "Commit changes" — deployment starts automatically.

**Using git on your local machine:**

```bash
git add .
git commit -m "update portfolio content"
git push
```

Deployment takes about 1–2 minutes. Check the progress under the **Actions** tab of your repo.

**Finding your live URL:**

Go to your repo → Settings → Pages. The URL is shown at the top of that page.

---

## Troubleshooting

**My site is not updating after I push.**

- Check the **Actions** tab in your repo for errors (red X means something failed).
- Make sure GitHub Pages is set to deploy from the `gh-pages` branch (see Part 2).

**I get a "Page not found" error on my site.**

- GitHub Pages can take a few minutes to go live for the first time.
- Double-check that the `site_url` in `mkdocs.yml` matches your actual GitHub Pages URL.

**The Leaflet map is blank or not loading.**

- Make sure you have an internet connection (Leaflet loads from a CDN).
- Check that the div id in the HTML block matches the id in the `L.map()` call in the script.

**I broke something and want to start over.**

- On GitHub, you can view the history of any file by clicking **History** on the file page.
  Click on any past commit to see the old version, then restore it.
