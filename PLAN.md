# MUNI FSS Quarto Presentation Theme — Implementation Plan

## Overview

Convert the Masaryk University FSS Beamer/PowerPoint theme into a **Quarto Revealjs extension** so presentations can be authored as `.qmd` files and rendered to polished HTML slides that match the official brand.

> **Approach**: Plain SCSS + vanilla JS. Quarto Revealjs renders to HTML/CSS/JS — no React dependency needed. Interactive components (charts etc.) can be added later via Observable JS or vanilla JS if required.

---

## Design Tokens (extracted from beamerthemeMU.sty)

| Token | Value | Usage |
|---|---|---|
| `--color-fss` | `#007A53` (rgb 0,122,83) | Primary accent, structure |
| `--color-mu-blue` | `#0000DC` (rgb 0,0,220) | MU base (unused for FSS) |
| `--color-orange-1` | `#F5A500` (rgb 245,165,0) | Alerts / highlights |
| `--color-orange-2` | `#F08C00` (rgb 240,140,0) | Alert blocks |
| `--color-gray-1…7` | `#F0F0F0` → `#828282` | Backgrounds, rules |
| Font | **PT Sans Caption** | Headings + UI |
| Margin | `0.7cm` | Slide gutter |
| Footer height | `0.4cm` | Footline bar |
| Logo height | `1.4cm` | Title slide logo |

---

## Repository Structure (target)

```
muni_theme/
├── _extensions/
│   └── muni-fss/
│       ├── _extension.yml          # Quarto extension manifest
│       ├── muni-fss.scss           # Main theme stylesheet
│       ├── muni-fss.css            # Compiled output (generated)
│       ├── muni-fss.js             # Optional: slide init / React bundle
│       ├── template.html           # Revealjs HTML template override
│       └── assets/
│           ├── logo-fss-color.svg  # FSS logo (extracted from .potx)
│           ├── logo-mu-color.svg   # MU logo
│           └── fonts/              # PT Sans Caption woff2 files
├── example/
│   ├── example.qmd                 # Demo presentation
│   └── example.bib
├── fss.zip                         # Source Beamer theme (reference)
├── muni-fss-prezentace-16-9-en-v10.potx  # Source PowerPoint theme
├── PLAN.md                         # This file
└── README.md
```

---

## Phases

### Phase 1 — Asset Extraction ✅
- [x] Extract logos from `.potx` (`ppt/media/` — 4 EMF files converted to PNG via PowerShell)
- [x] Extract color palette from `.potx` theme XML — confirmed `#007A53`, `#0000DC`, `#FBAE40`
- [x] Cross-check against Beamer theme values — all match
- [x] PT Sans Caption TTFs placed in `_extensions/muni-fss/assets/fonts/`
- [x] Logos placed in `_extensions/muni-fss/assets/logos/` (blue, white, wordmark variants)

**Logo inventory:**
- `logo-fss-blue.png` — `MUNI / FSS` blue wordmark (light backgrounds)
- `logo-fss-white.png` — `MUNI / FSS` white wordmark (green header bar)
- `logo-mu-wordmark.png` — `MASARYK UNIVERSITY` outline (outro slide, green background)

### Phase 2 — Quarto Extension Scaffold ✅
- [x] `_extension.yml` with revealjs format, SCSS theme, linked CSS, and `include-after-body`
- [x] `_quarto.yml` project file at root (required for subdirectory renders to find the extension)
- [x] `example/example.qmd` with all slide types demonstrated
- [x] `quarto render example/example.qmd` produces output without errors

**Font loading note:** Quarto does not auto-copy SCSS `url()` assets from extensions.
Solution: `muni-fss-fonts.css` is linked directly from `_extensions/muni-fss/` (relative path),
so `url("assets/fonts/...")` resolves correctly from the CSS file location. Logos are loaded
by `muni-fss-init.html` JS using the same extension base URL (extracted from the CSS `<link>`).

### Phase 3 — Core SCSS Theme ✅
- [x] **Color variables** — `$fss-green: #007A53`, `$mu-blue`, `$orange`, grays mapped to Reveal vars
- [x] **Typography** — PT Sans Caption, `$mainFontSize: 28px`, h2 `1.5em` bold green
- [x] **Title slide** — JS rebuilds DOM: green header bar + `logo-fss-white.png`, body with title/subtitle/author/institute/date
- [x] **Content slide** — white background, green bold h2, `36px` gutters
- [x] **Footer** — `author · title · date` left, `page / total` right; hidden on title + outro slides
- [x] **Outro slide** — `.outro` class → full-green background + centered `logo-mu-wordmark.png` via JS
- [x] **Extras** — `.slide-gray`, `.cols-2`, `.cols-3-7`, `.cols-7-3` layout helpers; blockquote, callout, code styles

### Phase 4 — Slide Layouts & Partials
- [ ] `template.html` overrides for title slide and outro slide HTML structure
- [ ] Revealjs `data-` attributes or CSS classes for:
  - Gray background slide (`.slide-gray`)
  - Two-column layout (`.columns-50-50`, `.columns-33-67`, etc.)
  - Full-bleed image slide (`.slide-image-full`)
- [ ] Block styles: `block`, `exampleblock`, `alertblock` matching Beamer rounded+shadow

### Phase 5 — ~~Interactive Components~~ _(deferred — Option A chosen, no React bundle needed)_

### Phase 6 — Example Presentation & Docs
- [ ] Expand `example.qmd` to demonstrate all slide types
- [ ] Write `README.md` with installation and usage instructions
- [ ] Test 16:9 and 16:10 aspect ratios
- [ ] Test PDF export via `quarto render --to pdf`

---

## Open Questions

1. ~~**React vs. plain JS**~~ — **Option A confirmed**: plain SCSS + vanilla JS. No React dependency.
2. ~~**Logo licensing**~~ — **Public repo**. Use only logos covered by MUNI's public brand guidelines; no internal/restricted assets.
3. ~~**Quarto version**~~ — **1.9.36** confirmed. Full extension API available, Lua filters supported, `contributes.formats` stable.
4. ~~**Font hosting**~~ — **Self-host confirmed**. PT Sans Caption woff2 in `assets/fonts/`, loaded via `@font-face`.
5. ~~**Locale**~~ — **English-only first**. Czech/Slovak variants deferred.

---

## Key Reference Files

| File | Purpose |
|---|---|
| `fss.zip/beamerthemeMU.sty` | Full Beamer theme source — layout, colors, templates |
| `fss.zip/fss.tex` | Canonical usage example |
| `fss.zip/fss-aspect-16_9.pdf` | Visual reference for 16:9 layout |
| `muni-fss-prezentace-16-9-en-v10.potx` | PowerPoint source — logos, exact colors |
