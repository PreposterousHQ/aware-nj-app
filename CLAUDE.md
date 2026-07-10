# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A.W.A.R.E. NJ is a single-page marketing/informational website for a New Jersey
coalition that gives first responders a framework for recognizing and responding
to religious and spiritual experiences during mental health crisis calls (rather
than defaulting to restraint, misdiagnosis, and hospitalization).

There is **no build step, no framework, no npm, and no package.json**. The entire
site is one hand-authored HTML file with inlined CSS and JavaScript. Editing means
editing `index.html` directly.

## Architecture

- **`index.html`** — the whole site. It contains:
  - Inline `<style>` block (~lines 12–283): all CSS, mobile-first with a single
    `@media` breakpoint. Brand colors are hard-coded hex values (see palette below).
  - Semantic `<section>` elements, each with an anchor `id` that the header nav
    links to: `#top` (hero), `#transition`, `#app` (the framework), `#questions`,
    `#fork`, `#training`, `#audience`, `#emergency`, `#know`, `#voices`,
    `#resources`, `#contact`.
  - Inline `<script>` at the bottom (~lines 767–783): only two behaviors — the
    mobile menu toggle (`#menuBtn` / `#navlinks`) and an `IntersectionObserver`
    that adds the `in` class to `.reveal` elements as they scroll into view.
    Reveal animation is gated on `prefers-reduced-motion`.
- **Assets** live at the repo root (not in a subfolder) and are referenced by
  relative filename: `logo.png`, `nj-aware-tagline.png`, and headshots/photos in
  kebab-case (`bret-headshot.jpg`, `janet-headshot.jpg`, `scott-medlin.png`,
  `ernie-stevens.jpg`, `help-model.jpg`). Note there are also Title-Case duplicates
  of some images at the root (`Scott Medlin.png`, `Ernie Stevens.jpg`, etc.) — these
  are **not** referenced by `index.html`; edit/replace the kebab-case versions.
- **`sw.js`** is a *self-destructing* service worker: on activate it deletes all
  caches, unregisters itself, and reloads open clients. This intentionally undoes a
  previous PWA/offline setup so stale cached versions of the site clear on visitors'
  devices. Do not "fix" it into a caching worker unless offline support is being
  deliberately reintroduced.
- **Fonts** (Archivo, Public Sans) are loaded from Google Fonts via CDN `<link>`
  tags in `<head>` — the only external runtime dependency.

## Editing conventions

- Reuse existing utility classes rather than adding inline styles: `.wrap`
  (max-width container), `.band` / `.band-navy` (alternating section backgrounds),
  `.btn` / `.btn-red` / `.btn-navy`, `.eyebrow` / `.kicker` (labels), and `.reveal`
  (fade-in-on-scroll — add this class to make new content animate in).
- Keep content changes accessibility-safe: preserve `aria-*` attributes, the skip
  link, alt text, and the `.a` spans that highlight the A-W-A-R-E acronym letters.
- Brand palette (keep consistent): background `#07111F`, surface cards `#0F1E30`,
  blue accent `#63B3ED`, red/urgency `#E53E3E`, amber/questions `#F59E0B`,
  green/reference `#68D391`.

## Develop, preview, deploy

- **Preview locally:** open `index.html` directly in a browser, or serve the
  directory with any static server (e.g. `python3 -m http.server`). No tooling
  required. There are no tests, linters, or build commands.
- **Deploy:** the site is served by **GitHub Pages from the `main` branch root**,
  bound to the custom domain in `CNAME` (`awarenj.com`). Pushing to `main`
  publishes; there is no CI workflow.

## Other files

- **`index aware website v2.html`** — a working/staging copy of the page. The live
  page is `index.html`; treat the v2 file as scratch unless told otherwise.
- **`archive/old-site-2026-07-06/`** — a snapshot of a prior version of the site.
  Reference only; not deployed.
- **`README.md`** describes the original PWA setup (installable app with
  `manifest.json` and an `icons/` folder). That setup no longer matches the repo —
  there is currently no `manifest.json` or `icons/` directory, and `sw.js` now
  tears the PWA down. Trust this file and the actual sources over the README for
  current architecture.
