# aymgal.github.io — Aymeric Galan's academic website

This is a personal academic website built with [al-folio](https://github.com/alshedivat/al-folio), a Jekyll theme designed for researchers. It is meant to be hosted for free on GitHub Pages at `https://aymgal.github.io`.

This README is the map back into the site: what each file does and exactly what to edit for common updates. You should not need any help to make routine changes — just edit a file and push.

## How the site is built (the short version)

- The site is written in **Markdown and YAML**, not raw HTML. You edit small text files; [Jekyll](https://jekyllrb.com/) (a static site generator) turns them into the actual website.
- You never build the site yourself. Every time you `git push` to the `main` branch, a GitHub Actions workflow (`.github/workflows/deploy.yml`) automatically rebuilds the site and publishes it to `https://aymgal.github.io`. This usually takes 1–3 minutes — check the "Actions" tab of the repo on GitHub to see progress or catch errors.
- The visual theme itself (colors, layout, fonts, the actual HTML/CSS/JS) lives in the `al_folio_core` Ruby gem, referenced in `_config.yml` (`theme: al_folio_core`) and `Gemfile`. It is **not** part of this repo, which is why you won't find template/layout files here — only your content and configuration. You don't need to touch it.

## What to edit for common updates

| I want to... | Edit this file |
|---|---|
| Change my bio / homepage text | `_pages/about.md` |
| Change my photo | Replace `assets/img/prof_pic.jpg` with a new image of the same filename (or update the `image:` field in `_pages/about.md` if you rename it) |
| Add a news item / update | Add a new file in `_news/`, see below |
| Update my CV (positions, education, skills, etc.) | `_data/cv.yml` |
| Add/update publications | `_bibliography/papers.bib`, see below |
| Change contact email, GitHub link, etc. | `_data/socials.yml` |
| Change the site title, description, favicon | `_config.yml` (top section, "Site settings") |

### Adding a news item

Each file in `_news/` is one entry. Create a new file named `_news/YYYY-MM-DD-short-slug.md`, for example `_news/2026-09-15-new-paper.md`:

```markdown
---
inline: true
date: 2026-09-15
---

Your announcement text goes here. You can use [links](https://example.com) and **bold**.
```

The homepage shows the 5 most recent (see `announcements.limit` in `_pages/about.md`); the `/news/` page shows all of them.

### Updating publications

Publications are rendered automatically from `_bibliography/papers.bib` (standard BibTeX) — you don't write any publications HTML by hand. It was populated from your [ORCID record](https://orcid.org/0000-0003-2547-9815) on 2026-09-01, cross-checked against [Google Scholar](https://scholar.google.com/citations?user=nqkUADkAAAAJ) (40 entries; `selected = {true}` is set on first-author papers and the software packages you maintain, e.g. Herculens, COOLEST, SLITRONOMY). It won't update itself — re-export periodically (e.g. whenever you have a new paper) and merge in new entries by hand, or replace the file wholesale from a fresh export.

Scholar shows several more very recent (2026) items your ORCID record hasn't picked up yet — HST/JWST telescope proposals, VizieR data catalogs, AAS conference abstracts, and a handful of brand-new preprints (e.g. "HOLISMOKES XX", the ESO0286 IMF paper, "AgileLens", "Discovery Engine F"). Proposals/catalogs/abstracts were left out on purpose (not typically listed as papers), but the new preprints are real — add them next time you update this file.

To populate it (or refresh it later): export a BibTeX file of your papers from one of these sources, then paste the entries into `_bibliography/papers.bib` (or replace the whole file):

- **ORCID**: your profile → "Works" → export
- **NASA ADS**: build a library of your papers → Export → BibTeX
- **Google Scholar**: your profile page → select papers → "Export" → BibTeX

Useful optional fields you can add to any BibTeX entry (see the [al-folio publications guide](https://github.com/alshedivat/al-folio/blob/master/docs/CUSTOMIZE.md) for the full list):

- `selected = {true}` — show it in the "selected papers" list on the homepage
- `abbr = {A&A}` — a short badge shown next to the entry (e.g. journal abbreviation)
- `pdf = {my_paper.pdf}` — link to a PDF placed in `assets/pdf/`
- `arxiv = {2501.12345}` — link to the arXiv listing

Once you have your ORCID iD or Google Scholar ID, also add them to `_data/socials.yml` (`orcid_id:` / `scholar_userid:`) to show the profile links/icons.

## Previewing changes before pushing (optional)

You can just edit files and push — GitHub Actions will catch build errors and you'll see them in the "Actions" tab. If you'd like to preview locally first (requires Ruby):

```bash
bundle install
bundle exec jekyll serve
```

Then open `http://localhost:4000` in your browser.

## What was trimmed from the original al-folio template

To keep this repo focused on your actual content, the following were removed from the stock al-folio template: the blog, projects, teaching, and books demo pages/collections; demo images, audio, and video assets; local development tooling (Docker, various AI-agent config files, linters); and most GitHub Actions workflows except the one that deploys the site. The `collections:` block in `_config.yml` still declares `books`, `projects`, and `teachings` — if you want any of those sections back, you can re-add a page in `_pages/` and start dropping entries into the corresponding folder (e.g. `_projects/`).

**Privacy note:** your source CV PDF included a home address and date of birth. `_data/cv.yml` (the structured CV shown on `/cv/`) intentionally leaves those out.

**No downloadable CV PDF, on purpose:** the `/cv/` page shows only the structured version generated from `_data/cv.yml` — there is no "download PDF" button and no PDF file in this repo. If you want one back later: drop a PDF into `assets/pdf/`, then add `cv_pdf: /assets/pdf/your-file.pdf` back to the front matter of both `_pages/cv.md` and `_data/socials.yml`.

## First-time GitHub setup

If you're setting this up for the first time (rather than reading this after it's already live), see the setup notes your session left you, or:

1. Create a new **empty** repository on GitHub named exactly `aymgal.github.io` (no README/gitignore/license — this repo already has them).
2. In this folder: `git remote add origin git@github.com:aymgal/aymgal.github.io.git`
3. `git add -A && git commit -m "Initial site"` (if not already committed)
4. `git push -u origin main`
5. Check the repo's "Actions" tab for the deploy run, then visit `https://aymgal.github.io` once it finishes.
