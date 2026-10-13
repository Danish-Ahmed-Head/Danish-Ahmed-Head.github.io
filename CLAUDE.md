# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Danish Ahmed's personal portfolio: a hand-coded static site (plain HTML/CSS/JS, no build step) served by GitHub Pages at https://danish-ahmed-head.github.io/. The README describes it as "pure HTML/CSS/JS, no build step".

## Commands

There is no package.json, bundler, linter or test suite. Do not add one without being asked.

- Run locally: `python -m http.server 8765 --bind 127.0.0.1` from the repo root (this is the `portfolio` entry in `.claude/launch.json`, port 8765), then open http://127.0.0.1:8765/.
- Deploy: merging to the default branch publishes via GitHub Pages. History shows work lands through PRs (`audit-fixes-*`, `cv-sync-*`).

`.claude/settings.json` allows npm/pytest/ruff/tsc commands, but none of those apply to this repo.

## Architecture

Everything lives in one file, `index.html` (about 3000 lines), plus `assets/`:

- `<style>` block (head): all CSS, themed by `:root` tokens (navy/gold/ivory palette, Cormorant Garamond + Inter). Sections are marked with `/* SECTION NAME */` comments; the responsive overrides and later "override" blocks come after the base rules, so a rule can be silently overridden further down the file. Search for the selector before editing.
- Body sections, in order: `#home` (hero), `#skills`, `#experience`, `#projects`, `#about`, `#contact`. The nav links, the right-hand scroll dots (`data-target`) and the `sections` array in the first script must stay in sync with these ids.
- Two inline `<script>` blocks at the bottom: the first handles cursor glow, scroll progress, scroll-dot highlighting, smooth-scroll anchors and reveal-on-scroll; the second is the anime.js-powered layer (typewriter hero text, counters, per-section IntersectionObserver animations).
- External runtime dependencies: anime.js 3.2.1 from cdnjs (script in `<head>`) and Google Fonts. The README's "zero dependencies" claim is not strictly true.
- `#openclaw` pseudo-page: a hidden `<div id="openclaw">` toggled by `openOCPage()` / `closeOCPage()`, which hide the main sections and fixed chrome via `ocHidden` markers. The client's own site deep-links to `/#openclaw`, so the hash handler must keep working. The inline comment notes the dedicated case-study page was removed; if `#openclaw` is missing the function falls back to scrolling to `#projects`. Do not break that fallback.
- `assets/`: favicons, `og-image.png` (referenced by absolute URL in og/twitter meta), `danish-ahmed.webp`, and `danish-ahmed-cv.pdf` (linked from the hero "Download CV" button).

## Content rules (from the owner's standing preferences)

- Content is mirrored in three places that drift easily: `index.html` copy, `README.md` ("Selected Work" and "Tech" tables) and the CV PDF in `assets/`. Recent commits ("Sync portfolio site and CV PDF with the current CV") show they are updated together. The README tagline and the page `<title>`/meta description currently differ ("AI Engineer & Systems Architect" vs "Full-Stack & AI Engineer"); the HTML is the newer wording.
- Never show a GPA or grades on the public site, and do not add employers or projects that are not already on it.
- Client details are intentionally anonymised (see commit "Anonymise client details"); do not reintroduce client names or unverified claims such as offline or "zero-human" operation.
- Contact email is hard-coded in several `mailto:` links in `index.html`; change all of them together.
