# MUNI FSS Beamer Template for Quarto

Write presentations as [Quarto](https://quarto.org) `.qmd` files and render them
to PDF slides in the official Masaryk University / Faculty of Social Studies
design, using the university's own
[mubeamer](https://gitlab.fi.muni.cz/external_relations/document_templates/mubeamer)
Beamer theme (by Vít Novotný and Aleš Křenek) with two small modifications:

- the footline shows the **page number first**, bottom left
  (`3 / 10 · Author · Title · Date`), matching the PowerPoint template;
- a small **MUNI FSS wordmark** sits in the bottom-right corner of every slide.

Both modifications live in the YAML header of `muni-conference.qmd` — the theme files
themselves are unmodified upstream copies.

---

## Repository contents

| Path | Purpose |
|---|---|
| `muni-conference.qmd` | The presentation template — copy and rename this |
| `_contact.qmd` | Reusable closing contact slide (name, affiliation, links) |
| `beamerthemeMU.sty` | The official mubeamer theme (unmodified) |
| `mubeamer/` | Theme assets (logos, labels) and its LPPL-licensed source |

---

## Requirements

- [Quarto](https://quarto.org/docs/get-started/) ≥ 1.4
- A LaTeX distribution. TinyTeX works well: `quarto install tinytex`
- The PT Sans fonts used by the MU design: `tlmgr install paratype`
  (TinyTeX does not ship them; without them the render fails looking for
  `t1ptsans-tlf.fd` or similar)
- Optional, only for executable R chunks (figures/tables): R with `knitr`,
  plus whatever the chunks use (`ggplot2`, `dplyr`, `kableExtra`, `ggforce`
  in the shipped template). Delete the chunks if you don't need R.

---

## Using the template in another project

1. Copy **`muni-conference.qmd`** (rename it), **`_contact.qmd`**,
   **`beamerthemeMU.sty`**, and the **`mubeamer/`** folder into your project
   directory. The `.sty` and `mubeamer/` must sit next to the `.qmd`.
2. Edit the YAML front matter (title, author, institute, date).
3. Render:

   ```bash
   quarto render your-presentation.qmd
   ```

The PDF engine must remain `pdflatex` (set in the YAML) — the font packages
used here are not set up for xelatex/lualatex.

---

## Writing slides

- `## Heading` starts a new slide; `# Heading` starts a section.
- Columns:

  ```markdown
  :::: {.columns}
  ::: {.column width="45%"}
  left
  :::
  ::: {.column width="55%"}
  right
  :::
  ::::
  ```

- Tables from R data frames: `knitr::kable(..., booktabs = TRUE)`; styled
  variants via `kableExtra` (`kbl(format = "latex") |> kable_styling(...)`).
- Figure annotations (highlight boxes, arrows, labels) are best drawn in
  ggplot itself — `annotate("rect"/"curve"/"label", ...)` or
  `ggforce::geom_mark_rect()` — so they survive re-renders. `muni-conference.qmd`
  demonstrates all of the above.
- Raw LaTeX passes through, e.g. `\makeoutro` for the closing MU slide.
- The closing "Thank You / Contact" slide comes from `_contact.qmd` via
  `{{{< include _contact.qmd >}}}` — edit your details once there and every
  presentation picks them up. A commented-out per-paper line (title + DOI)
  is inside for talk-specific use.

---

## Customizing the modifications

Everything sits in the `include-in-header` block of the YAML:

- **Footline order/content** — the `\setbeamertemplate{footline}{...}` block.
  It reuses mubeamer's own fonts, colors, and spacing.
- **Corner logo** — the `tikzpicture` node at the end of that block:
  `height=0.6cm` for size, `xshift`/`yshift` for position.
- **Faculty/variant** — `themeoptions`: `workplace=fss` (other MU workplaces
  work too, e.g. `mu`), add `gray` for the grayscale variant, or
  `fonts=none` to fall back to Latin Modern instead of PT Sans.

---

## Upstream theme

The vendored `beamerthemeMU.sty` and `mubeamer/` assets are byte-identical to
the latest upstream source (last upstream change: January 2023). The theme is
not on CTAN/TeX Live; it is distributed via the
[MU GitLab repository](https://gitlab.fi.muni.cz/external_relations/document_templates/mubeamer)
and as an
[Overleaf template](https://www.overleaf.com/latex/templates/a-beamer-theme-for-the-faculty-of-social-studies-at-the-masaryk-university-in-brno/vnszhkkcdhrw).

---

## Troubleshooting

- **`namespace 'rlang' ... is required` during render** — Quarto is using a
  different R than you expect. Positron sets `QUARTO_R` to its active R
  interpreter; make sure that R has the needed packages, or point `QUARTO_R`
  at the right one.
- **Missing font errors mentioning `ptsans`** — run `tlmgr install paratype`.
