# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single static HTML/CSS portfolio site for Danian Martin (design / motion / photography), served as-is with no build step, bundler, or package.json. `index.html` and `styles.css` are hand-authored and edited directly.

Live deploy target: GitHub Pages from this repo (`Lonerismdan/Dan-Design-`, branch `main`, root folder). There is no CI — pushing `main` is the deploy.

## Commands

Preview locally (required before trusting any visual change — opening `index.html` directly via `file://` breaks video posters and some relative paths):

```bash
python3 -m http.server 8000
```
then open `http://localhost:8000`.

Ship a change:
```bash
git add .
git commit -m "..."
git push
```
GitHub Pages redeploys automatically within ~1-2 minutes of a push to `main`. There is no separate build/lint/test command — there's no tooling to run.

## Architecture

### Page structure (in `index.html`, top to bottom)
Sticky `.topbar` nav → `.hero` (headline + portrait) → `.ribbon` (scrolling skills marquee) → `.about` (statement section) → `.contents` (jump-link index) → three `.dept` sections (`#motion`, `#design`, `#photo`) each containing a `.grid` of `.card`s → `.quote` → services `.stage-list` (accordion) → `.closing` (CTA + footer columns).

### The project grid is CSS multi-column masonry, not CSS Grid
`.grid` uses `columns: N <minwidth>` (see `styles.css`), and each `.card` has `break-inside: avoid` plus its own `margin-bottom`. This is deliberate: an earlier CSS Grid version caused a bug where a tall multi-image card (e.g. Kate, Eyes) would force its entire grid row to match its height, leaving large dead gaps under shorter cards sharing that row. Multi-column layout lets each card flow independently by its own content height. Do not revert this to `display:grid` with `grid-template-columns` without re-solving that row-height coupling problem.

### Card patterns (copy an existing one, don't invent a new shape)
Two card shapes exist in the `.grid` containers, both under `class="card"`:
- **Single image/video**: `.thumb` (img or `<video poster="..." controls preload="none">`) + `.card-body` (`.meta` mono label, `<h4>` title, `<p>` description).
- **Multi-image set** (e.g. Kate/Eyes, Golden Light, Studio Portraits): `.card-body` first (title/description), then a plain inline-styled sub-grid `div` of `.thumb` elements, no second `.card-body` needed unless closing text follows.

Motion & Film uses `.grid.wide` (wider min column) since video thumbnails read better larger than poster grids.

### Visual system (`styles.css`)
Dark/monochrome only — no light mode, no color accents beyond what's in the actual photos/artwork. Type is Archivo (headlines/UI, weight 900 for bold display type) and Courier Prime (`.mono` class — small labels, meta lines, nav). CSS custom properties (`--bg`, `--fg`, `--fg-dim`, `--fg-mute`, `--line`, etc.) live in `:root` at the top of `styles.css` — change the palette there, not per-component. Thumbnails intentionally show full color (an earlier grayscale-by-default/color-on-hover treatment was removed at the user's request).

### Adding a new project card
1. Drop the image in `images/` (lowercase, dash-separated filename) or video in `videos/` (+ a poster image in `images/`).
2. Copy the nearest matching existing `.card` block in the relevant `.dept` section and edit the `src`, `alt`, `.meta`, `<h4>`, and `<p>` in place.
3. No JS, no build step — save and refresh.

### `images/originals/`
Holds a few unedited source photos kept for reference. Not referenced by `index.html` — don't treat files here as in-use assets.

### Content/tone conventions worth preserving
- Project descriptions are specific and process-forward (what was actually done, not generic marketing copy) — match that register when adding new ones.
- AI-assisted pieces are captioned honestly rather than hidden or oversold (see the Business Card — AI Concept entry and the note in `.about-body`) — this is a deliberate positioning choice for a hiring-manager audience, not an oversight.
