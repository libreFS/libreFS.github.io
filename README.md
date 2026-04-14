# libreFS.github.io

Source for [librefs.org](https://librefs.org) — the project website for [libreFS](https://github.com/libreFS/libreFS), a community-maintained open-source S3-compatible object storage server.

## Structure

```
libreFS.github.io/
├── index.html          # Homepage
├── downloads.html      # Downloads page
├── style.css           # Site-wide stylesheet
├── sitemap.xml
├── robots.txt
├── CNAME               # Custom domain (librefs.org)
├── .nojekyll           # Disables Jekyll so MkDocs output is served as-is
├── mkdocs.yml          # MkDocs config used by CI to build versioned docs
├── requirements.txt    # Python deps for the docs build (mkdocs-material, mike)
└── docs/               # Built documentation (generated — do not edit by hand)
    ├── latest/         # Always points to the most recent release
    ├── RELEASE.*/      # One directory per server release tag
    └── versions.json   # Version list consumed by the MkDocs version selector
```

## Editing the website

The site is plain HTML + CSS — no build step. Edit `index.html`, `downloads.html`, or `style.css` directly and push to `main`. GitHub Pages deploys automatically.

## Versioned documentation

Docs are built from the Markdown files in [libreFS/libreFS](https://github.com/libreFS/libreFS) and deployed here under `docs/<version>/`. The build is driven by the workflow at [`.github/workflows/docs.yml`](.github/workflows/docs.yml).

### Deploying docs for a new release

After cutting a new server release tag, trigger the workflow manually:

1. Go to **Actions → Build and Deploy Docs → Run workflow**
2. Enter the release tag (e.g. `RELEASE.2026-04-12T22-27-24Z`)
3. Leave **Mark as latest** checked for the newest release

The workflow will:
- Check out the server repo at that tag
- Build MkDocs using the `mkdocs.yml` in this repo (`docs_dir` points at the server checkout)
- Commit the output to `docs/<version>/` and `docs/latest/`
- Regenerate `docs/versions.json`

Alternatively, the workflow is triggered automatically via `repository_dispatch` when a release is published from `libreFS/libreFS`.

## Local preview

To preview the docs locally:

```bash
pip install -r requirements.txt
# Check out the server repo alongside this one
git clone https://github.com/libreFS/libreFS _server
mkdocs serve
```
