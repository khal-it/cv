# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a LaTeX-based professional CV/resume for Khalit Hartmann, built on a customized version of the [AltaCV](https://github.com/liantze/AltaCV) template. The CV is maintained in two languages: English and German.

## Build Commands

Compile PDFs with `pdflatex` (not XeLaTeX/LuaLaTeX):

```bash
pdflatex cv-khalit-hartmann-EN.tex   # English version
pdflatex cv-khalit-hartmann-DE.tex   # German version
```

The Lato font package and fontawesome are required. Biber is configured as the bibliography backend but `sample.bib` is mostly a placeholder.

## File Structure

- **`cv-khalit-hartmann-EN.tex`** / **`cv-khalit-hartmann-DE.tex`** — Main CV documents (English/German). These are the primary files to edit for content changes.
- **`page1sidebar-en.tex`** / **`page1sidebar-de.tex`** — Page 1 sidebar: languages, education, areas of expertise (skills by proficiency level).
- **`page2sidebar-en.tex`** / **`page2sidebar-de.tex`** — Page 2 sidebar: personal projects with links.
- **`altacv.cls`** — The document class. Defines all custom commands (`\cvevent`, `\cvtag`, `\cvskill`, `\cvsection`, `\makecvheader`, etc.). Modify this to change layout/styling behavior.
- **`khalit.png`** — Profile photo. **`company-logos-outline.png`** — Company logos banner at the bottom.
- **`sample.bib`** — Bibliography file (minimal, not actively used for publications).

## Architecture Notes

- The EN and DE `.tex` files are **independent documents** with parallel structure — they are not generated from a shared template. Content changes must be applied to both files manually.
- Sidebar content is loaded via `\cvsection[page1sidebar-en]{...}` (the optional argument loads the sidebar file). Page 2 sidebars use `\addnextpagesidebar` defined in `altacv.cls`.
- The `\fullwidth` environment spans the full page width (overriding the margin layout). Used for experience entries on pages 2+ and the references section.
- Page geometry: `left=1cm, right=9cm, marginparwidth=6.8cm, marginparsep=1.2cm` — the large right margin accommodates the sidebar.
- The accent color `VividPurple` is actually teal (`#008b8b`), not purple.
- The German version adds `\usepackage[ngerman]{babel}` for German typographic conventions.

## Key LaTeX Commands (from altacv.cls)

- `\cvevent{Title}{Company}{Date}{Location}` — Work experience entry
- `\cvtag{Label}` — Rounded skill/technology tag
- `\cvskill{Name}{Rating}` — Skill with 1-5 circle rating
- `\cvsection[sidebar-file]{Title}` — Section heading with optional sidebar
- `\printinfo{\faIcon}{text}` — Icon + text pair for personal info

## When Making Content Changes

- Each experience entry follows the pattern: `\cvevent` → bold Task/Achievements → `\begin{itemize}` → `\cvtag` list → `\vspace`
- Keep both EN and DE files in sync when adding/updating positions
- Dates use DD.MM.YYYY format (European style)
- `\newpage` is used manually to control page breaks
