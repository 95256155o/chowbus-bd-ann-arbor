# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A single-page static HTML field intelligence tool built for a Chowbus Sales Manager role (Chinese restaurant vertical, Ann Arbor, MI). It is deployed to GitHub Pages at `https://95256155o.github.io/chowbus-bd-ann-arbor/`.

The repo contains one file: `index.html`. There is no build system, no package.json, no framework.

## Deployment

```bash
# Edit index.html, then:
git add index.html
git commit -m "your message"
git push
```

GitHub Pages auto-deploys on push to `main`. Live within ~30 seconds.

## Local Preview

```bash
# From the repo directory:
python3 -m http.server 8080
# Open http://localhost:8080
```

## Architecture

Everything is self-contained in `index.html`:

- **CSS variables** at `:root` — Chowbus brand palette (`--brand: #f43f5e`, `--text-primary: #1f2937`). All color changes go here.
- **Leaflet.js** loaded from unpkg CDN — map tiles from `tile.openstreetmap.org`. No local tile files in this repo (local tiles live at `/home/jl/projects/tiles/` on the dev machine but are not deployed).
- **Restaurant data** — hardcoded in the Leaflet JS block at the bottom of `<body>`. The `restaurants` array contains all 10 accounts with lat/lng, BD priority, POS system, DM info, visit windows, and pitch angles.
- **All field data is mock/fictional** — only restaurant names, real addresses, and Chowbus product descriptions are factual. The disclaimer banner at the top of the page makes this explicit.

## Page Structure (top to bottom)

1. Disclaimer banner
2. Sticky header (Chowbus branding)
3. "Why Chowbus Wins" strip — 4 short phrases, no symbols
4. 90-Day Field Playbook — 3 phase cards (Days 1–30 / 31–60 / 61–90)
5. Leaflet map — 10 pins color-coded by lead temp, numbered by BD priority
6. BD Priority Summary table — 9 rows ranking accounts by urgency
7. 10 Restaurant Intelligence Cards — 2-column grid, each with location, POS/contract, DM, pain points, Chowbus pitch angle, BD notes

## Key Design Decisions

- **Pin numbers = BD priority rank**, not list order. Priority 1 = Palace Tang (renewal imminent), Priority 9 = King Shing (Jan 2027 contract).
- **Chowbus Fit badge** (HIGH/MEDIUM) appears alongside the lead temp badge on each card — HIGH means strong ICP alignment for Chowbus specifically.
- **No backend, no localStorage** — the page is read-only; visit logging and pipeline tracking are manual for now.
- **Bilingual touches** — subtle Chinese characters (中餐厅垂直赛道) in the header subtitle signal Mandarin capability without making the page inaccessible.

## Source Files (dev machine only, not in repo)

The working source and all related files live at `/home/jl/projects/`:
- `ann_arbor_bd_restaurants.html` — the editable source (has localhost paths for local preview)
- `tiles/` — pre-downloaded OSM tiles for offline local use
- `libs/` — local Leaflet JS/CSS for offline local use
- `ann_arbor_top10_chinese_restaurants.md` — the original research markdown
- `chowbus_sales_manager_interview_prep.md` — job description and interviewer profiles

When editing locally, use the source file and run the local server. When ready to deploy, swap localhost paths to CDN (see sed command below) and push:

```bash
sed 's|http://localhost:8080/libs/leaflet.css|https://unpkg.com/leaflet@1.9.4/dist/leaflet.css|g; s|http://localhost:8080/libs/leaflet.js|https://unpkg.com/leaflet@1.9.4/dist/leaflet.js|g; s|http://localhost:8080/tiles/{z}/{x}/{y}.png|https://tile.openstreetmap.org/{z}/{x}/{y}.png|g' \
  /home/jl/projects/ann_arbor_bd_restaurants.html > /tmp/chowbus-bd-ann-arbor/index.html
cd /tmp/chowbus-bd-ann-arbor && git add index.html && git commit -m "Update" && git push
```
