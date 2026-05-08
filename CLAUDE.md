# CLAUDE.md

Guidance for Claude Code working in this repository.

## What this repo is

Deployment artifact repository for the **PopMonster (泡泡怪獸 / Popbobble)** static website. The actual site source lives in a separate authoring repository; this repo exists solely to publish a snapshot to GitHub Pages at **popmonster.vip**.

The site is a 33-page e-commerce catalog: 31 product pages (`A001.html` – `A031.html`), an `index.html` landing page, and a `products.json` data file.

## Repository layout

```
.
├── .github/workflows/deploy.yml   # GitHub Pages deployment pipeline
├── popmonster-pages.zip           # Archived site bundle (33 files, ~67 KB)
├── LICENSE                        # CC0 1.0 (public domain)
└── CLAUDE.md                      # This file
```

There is **no source code** here. The HTML/JSON content is bundled inside `popmonster-pages.zip` and is not extracted in the repo working tree.

## Deployment workflow

`.github/workflows/deploy.yml` runs on every push to `main` (and manual `workflow_dispatch`):

1. Checks out the repo
2. Uses `peaceiris/actions-gh-pages@v4` to publish `./` to the `gh-pages` branch
3. Sets the custom domain via `cname: popmonster.vip`

The workflow publishes the **entire repo root** — including the zip file. For the site to actually be live, either:
- The site files must be **extracted at the repo root** before pushing to `main`, OR
- The workflow must be modified to unzip `popmonster-pages.zip` before deploy.

If you find the deployed site is broken, check whether the zip extraction step is missing.

## Common tasks

**Inspect bundle contents** without extracting:
```bash
unzip -l popmonster-pages.zip
```

**Extract for local preview**:
```bash
unzip popmonster-pages.zip -d site/
cd site && python3 -m http.server 8000
```

**Update the deployed site** (typical flow):
1. Replace `popmonster-pages.zip` with a new bundle from the source repo
2. Commit on `main` — the workflow auto-deploys
3. Verify at https://popmonster.vip

## Conventions

- Product pages use uppercase IDs (`A001.html` … `A031.html`) — note this differs from the `popmonster-vip` repo which uses lowercase (`a001.html`).
- All site copy is in Traditional Chinese (zh-TW).
- License is CC0 — site assets are public domain.

## Branch policy

Active development happens on feature branches; `main` is the deploy trigger. Do **not** push directly to `main` without confirming the bundle is correct, as it will immediately publish to production at popmonster.vip.

Current working branch: `claude/add-claude-documentation-68Wrh`.

## Related Repos

Part of the PopMonster ecosystem under `milk790-code`:

| Repo | Role |
|---|---|
| **`popmonster-website-deployment`** (this repo) | Deployment artifact (zip → GitHub Pages → popmonster.vip) |
| `popmonster-vip` | Static site source + `social_distributor` Flask backend |
| `customer-project-portal` | Full-stack SaaS portal with AI search; also serves PopMonster site at `/` |
| `popmonster-linebot` | LINE customer-service bot (Flask + OpenAI) |
| `Repository-name-popmonster-website-` | Placeholder / stub repo |
