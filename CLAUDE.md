# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-file static website: `index.html` is a long, scroll-driven presentation deck — a beginner's masterclass on using Claude ("Claude for Complete Beginners", branded Axecute). There is **no build system, no package manager, no tests, and no installed dependencies**. GSAP and Google Fonts load from CDNs at runtime. `frontend-design-skill.zip` is an unrelated bundled asset, not part of the site.

## Working with it

- **Preview:** open `index.html` directly in a browser, or serve the folder (`python3 -m http.server`) and visit it.
- **Deploy:** Vercel (`.vercel` is gitignored). No deploy command lives in the repo.
- There is nothing to compile or lint — edit the HTML and refresh.

## Architecture

Everything lives in `index.html`: one `<style>` block in `<head>`, the slide markup in `<body>`, and one `<script>` block at the end. The three are coupled by conventions below — keep all edits consistent with them.

### Slides are `<section>` elements driven by `data-nav`

- Each slide is a full-height `<section>` with `id="sec-<name>"` and (if it should be a scroll target) `data-nav="<name>"`. Sections are separated by numbered HTML comments, e.g. `<!-- 12. CONNECTORS -->`.
- The right-hand **nav dots are generated at runtime** from `document.querySelectorAll('section[data-nav]')`. Adding a section with `data-nav` automatically adds a dot and scroll-spy entry — **no JS change needed**. A section without `data-nav` (e.g. the closing slide) is intentionally skipped.
- Because of this, the **order of `<section>` elements in the source is the order of the deck.** To reposition a slide, move its block.

### Animation conventions (don't break these when adding markup)

- Any element with class `reveal` fades in on scroll via an `IntersectionObserver` that adds `.is-visible`. New copy/cards should carry `reveal` to match the rest.
- GSAP `ScrollTrigger` applies parallax to `img.img-cover` **except** those inside a `.card`. A top progress bar (`#progressFill`) tracks scroll.

### Styling: design tokens + reusable classes (no inline-CSS sprawl for structure)

- Colors and surfaces are CSS variables in `:root`: `--ink` (text), `--void`/`--carbon`/`--graphite` (backgrounds, darkest→lighter), `--line`, and the two accents `--acid` (lime green, primary highlight) and `--cyan`. Use these variables, never hardcoded hex.
- Build slides from the existing component classes rather than new CSS: `.card` / `.card-acid` / `.card-cyan`; `.split` + `.split-left` / `.split-right` / `.split-copy` (two-column slides); `.section-band` + `.section-inner` (centered full-width slides); `.body-lg` / `.body-md`, `.section-kicker`, `.step-num`, `.kbd` / `.kbd-row` / `.kbd-plus` (keyboard chips), `.desk-window` / `.desk-bar` / `.desk-main` (app mockups), `.button primary|secondary`, `.three-col` / `.four-col` (grids that collapse on mobile), `.app-panel` + bubble classes (chat mockups), `.hl`.
- Fonts by role: **Manrope** body, **Instrument Serif** for `h1`/`h2` (italic variant used decoratively), **JetBrains Mono** for kickers, labels, and `<code>`.

## Conventions to match

- **Tone:** plain-English, beginner-facing. Copy uses British spelling in places ("analyse", "organise", "summarise") — match the surrounding text.
- **Cross-platform shortcuts:** keyboard chips show both keys, e.g. `Cmd / Ctrl + N` and `⌥ Opt / Alt + Space`.
- **Accent usage maps to the "three Claudes" theme:** cyan ≈ Claude.ai/Chat, acid ≈ Cowork, ink/white ≈ Code. Reuse the same accent for a concept across slides so the color coding stays meaningful.
</content>
