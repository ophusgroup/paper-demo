# Dapagliflozin in Outpatient Heart Failure

Web edition of **"Dapagliflozin in Outpatient Heart Failure: Effects on
Hospitalization and Cardiovascular Mortality"** by Ana Johnson and Aaron Johnson
(April 20, 2026), a systematic review of seven high-quality studies of
dapagliflozin across the heart failure spectrum.

## Building the site

The site is built with [MyST Markdown](https://mystmd.org/):

```bash
npm install -g mystmd   # or: pip install mystmd
myst start              # local dev server with live reload
myst build --html       # static site in _build/html
```

The book-theme template needs two patches that have no configuration option
(top-level sidebar sections stay expanded, and the modal search is replaced by
a flat top-bar input that does not shift the page). Run them after the template
has been downloaded, and again whenever `_build/` is cleared:

```bash
python3 scripts/patch_theme.py
```

## Deployment

Pushes to `main` trigger the GitHub Actions workflow in
`.github/workflows/deploy.yml`, which builds the site and publishes it to
GitHub Pages. One-time setup on GitHub: repository **Settings → Pages →
Source → GitHub Actions**.

## Layout

- `index.md`: title page, abstract, and PICOT question
- `sections/`: the body of the paper, one page per section
- `references.md`: APA reference list; every in-text citation links to its entry
- `style.css`: theme overrides for the MyST book-theme
- `scripts/patch_theme.py`: sidebar and search patches described above

## Why the DOIs in `references.md` are escaped

The reference list is reproduced verbatim from the submitted manuscript, and
each DOI is written as `https\://doi.org/...` — the backslash is deliberate.

MyST's `transformLinkedDOIs` rewrites *any* link whose target resolves to a
DOI, including a bare URL that Markdown auto-links. It replaces the visible URL
with an author-year label fetched from doi.org and appends a second,
auto-generated bibliography, which both duplicates this list and rewrites the
author's entries — for one DOI it resolved to a different paper than the one
cited. There is no setting to turn the transform off, and it matches any URL
with a DOI in its path, so escaping the scheme is what keeps each DOI as
literal text.

The trade-off is that DOIs are displayed but not clickable. Non-DOI URLs in the
list (Perplexity, the two FDA documents) are unaffected and remain links. The
explanation lives here rather than in a `%` comment in `references.md` because
MyST puts comment text into the site search index.
