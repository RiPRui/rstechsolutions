# RSTECH

Static marketing site for **RS Tech Solutions** — Home, Services, About and Blog.

Built as self-contained HTML. Each page loads React/Babel from a CDN at runtime and the
Nocturne design-system bundle from the local `_ds/` folder, so the site works as plain
static files with no build step.

## Hosting on GitHub Pages

1. Repo **Settings → Pages**.
2. **Source:** *Deploy from a branch*.
3. **Branch:** `main`, **folder:** `/ (root)`.
4. Save. The site publishes at `https://<username>.github.io/RSTECH/`.

> `.nojekyll` is included so GitHub Pages serves the `_ds/` bundle (Jekyll otherwise
> skips folders that start with `_`).

## Pages

- `index.html` — Home (hero, services preview, blog preview, contact form)
- `services.html` — all 12 services
- `about.html` — about, values, process
- `blog.html` — blog index (links out to live articles)

An internet connection is required to load the React/Babel CDN scripts, the blog
thumbnails and the About headshot (all referenced from rstechsolutions.co.uk).
