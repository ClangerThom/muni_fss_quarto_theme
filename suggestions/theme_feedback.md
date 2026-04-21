# muni-fss Theme — Feedback for Next Version

**Theme version:** 0.1.0  
**Date:** 2026-04-21  
**Tested in:** https://github.com/ClangerThom/parental_readiness_survey_experiment

## Bug: Logo fails to load in self-contained mode

When rendering with `self-contained: true`, Quarto inlines all CSS as `<style>` blocks rather than `<link>` tags. The `getExtBase()` function in `muni-fss-init.html` searches for a `<link>` tag containing `muni-fss` in the href to resolve the extension's base URL. In self-contained mode this search always fails, and the fallback fires instead.

The fallback path is also incorrect — `./_extensions/muni-fss/` omits the owner prefix and should be `./_extensions/ClangerThom/muni-fss/`. Correcting the fallback is a minimal fix, but the robust fix is to **embed both logos (`logo-fss-blue.png` and `logo-mu-wordmark.png`) as base64 data URIs directly in the init script**. This removes any dependency on runtime path resolution and makes the theme work correctly under both self-contained and standard rendering.

## Issue: Extension not found when qmd is in a subdirectory

Quarto does not reliably walk up the directory tree to find `_extensions` when the input file is in a subfolder. If a user places their qmd in e.g. `presentation/`, the extension must live at `presentation/_extensions/` rather than the project root. This is non-obvious and causes a hard error with no clear diagnostic.

The README should document this explicitly. For users working in multi-file Quarto projects, the recommended setup is either to keep all qmd files at the project root or to use a `_quarto.yml` project file, which changes how Quarto resolves extension paths.

## Underlying principle

Both issues stem from the init script making assumptions about the runtime environment (link tags present, qmd rendered from root) that break under common configurations. Defensive defaults — embedded assets, clear path documentation — would make the theme work out of the box in more situations.
