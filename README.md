# MUNI FSS Quarto Theme

A [Quarto](https://quarto.org) Revealjs presentation theme for the Faculty of Social Studies, Masaryk University.

---

## Using this theme in another project

### 1. Install the extension

Inside your project folder, run:

```bash
quarto add ClangerThom/muni_fss_quarto_theme
```

This creates `_extensions/muni-fss/` in your project. You only need to do this once per project.

> **Offline / manual install:** copy the `_extensions/muni-fss/` folder from this repo directly into your project root.

---

### 2. Create a presentation file

Create a `.qmd` file with this frontmatter:

```yaml
---
title: "Your Presentation Title"
subtitle: "Optional subtitle"
author: "Your Name"
institute: "Faculty of Social Studies, Masaryk University"
date: today
format: muni-fss-revealjs
---
```

Then write your slides using standard Quarto Markdown — each `##` heading starts a new slide.

---

### 3. Render

```bash
quarto render your-presentation.qmd
```

Opens as a self-contained HTML file. For a live preview while writing:

```bash
quarto preview your-presentation.qmd
```

---

## Slide types

### Regular content slide

```markdown
## Slide Title

Plain text, **bold**, *italic*, `inline code`.

- Bullet one
- Bullet two
  - Nested bullet
```

### Two-column layout

```markdown
## Slide Title

:::: {.cols-2}
::: {}
Left column content
:::
::: {}
Right column content
:::
::::
```

Also available: `.cols-3-7` (narrow left) and `.cols-7-3` (narrow right).

### Gray background slide

```markdown
## Section Break {.slide-gray}

Used for transitions or section headings.
```

### Outro / closing slide

The last slide with no content — just add `.outro` and leave the body empty:

```markdown
## {.outro}
```

This renders a full-green slide with the Masaryk University wordmark centered.

---

## Frontmatter reference

| Option | Default | Description |
|---|---|---|
| `title` | — | Full presentation title (shown on title slide) |
| `subtitle` | — | Optional subtitle |
| `author` | — | Presenter name (also appears in footer) |
| `email` | — | Presenter email shown on title slide below the name |
| `institute` | — | Affiliation shown on title slide |
| `date` | — | Use `today` for the current date (formatted dd-mm-yyyy) |
| `format` | — | Must be `muni-fss-revealjs` |
| `footer-author` | `author` | Short name in footer, e.g. `"R. Speedwagon"` |
| `footer-title` | `title` | Short title shown **bold** in footer |

**Footer format:** `[footer-author] · [footer-title] · [date]` with slide number on the right, matching the Beamer footline template.

Any standard [Quarto Revealjs options](https://quarto.org/docs/presentations/revealjs/) can be added under `format: muni-fss-revealjs:` and will override the theme defaults.

---

## Requirements

- [Quarto](https://quarto.org/docs/get-started/) ≥ 1.4
- PT Sans Caption font files in `_extensions/muni-fss/assets/fonts/`  
  (included when you install via `quarto add`; if installing manually, download from [Google Fonts](https://fonts.google.com/specimen/PT+Sans+Caption))
