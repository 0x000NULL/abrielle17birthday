# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A static birthday picturebook website for Abrielle's 17th birthday. Single-page, no build tools, no framework — just `index.html`, `manifest.js`, and a `photos/` directory.

## How to Run

Open `index.html` directly in a browser, or serve with any static file server:
```
python -m http.server 8000
```

No build step, no dependencies, no package manager.

## Architecture

**`index.html`** — The entire app: CSS, HTML structure, and JavaScript all in one file.
- CSS (lines 10–779): Styles for cover page, photo layouts, lightbox, family messages, finale, year navigation, and responsive breakpoints.
- HTML (lines 781–870): Static cover page, `#chapters` container (populated by JS), hardcoded family message cards, finale section, and lightbox overlay.
- JavaScript (lines 875–1171): Reads `MANIFEST` array, groups photos by year, builds chapter sections with varied grid layouts, sets up lazy loading via IntersectionObserver, lightbox with keyboard nav, scroll-triggered animations, progress bar, and year quick-jump nav dots.

**`manifest.js`** — Exports a single `const MANIFEST` array. Each entry has: `file` (filename in `photos/`), `date`, `year`, `w`, `h`, `undated` (boolean). This is the data source for all photos displayed on the site.

**`photos/`** — JPG images referenced by the manifest. Filenames are iPhone-style (`IMG_XXXX.jpg`). Some have suffixes like `-COLLAGE`, `-ANIMATION.gif`, `-POP_OUT`, or `(1)` for duplicates.

## Key Patterns

- Photos are grouped by `year` and rendered as chapters. Undated photos (`undated: true`) are grouped under year 9999, displayed as "More Memories".
- Layout groups cycle through grid patterns: single, two, three, one-two, four, five — chosen based on chunk size.
- Decorations (tape, pins) are randomly assigned to photo wrappers via CSS classes (`deco-tape`, `deco-pin`, etc.).
- Images use `data-src` for lazy loading with IntersectionObserver (400px root margin).
- Family messages (Mom, Dad, Aidan, Ethan) are hardcoded in the HTML, not in the manifest. Aidan's message is a placeholder.

## Adding Photos

1. Place the image file in `photos/`.
2. Add a corresponding entry to the `MANIFEST` array in `manifest.js` with the correct `file`, `date`, `year`, `w`, `h`, and `undated` fields.
