# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

This repo is a "slide factory": each top-level folder (e.g. `powerbi/`) is a
self-contained sub-project for one presentation topic. Output is static
HTML (Reveal.js) using a shared "lean/clean" design system, with no build
step — decks are opened directly in a browser.

## Workflow / skills

Two project skills drive the workflow, in order:

1. **`slide-interview`** (`.claude/skills/slide-interview/`) — runs an
   interview to define objective, audience, plan, and per-slide content,
   then writes `<sous-projet>/brief.md`.
2. **`slide-builder`** (`.claude/skills/slide-builder/`) — reads
   `<sous-projet>/brief.md` and generates the Reveal.js deck in
   `<sous-projet>/slides/index.html` plus `<sous-projet>/slides/assets/`.

Always run the interview first (or confirm an existing `brief.md` is
up to date) before generating/regenerating HTML.

## Shared theme

`shared/theme/lean.css` is the single design system used by every deck —
do not duplicate styling per deck. Decks reference it with a relative path
(`../../shared/theme/lean.css` from `<sous-projet>/slides/index.html`).

- Reusable layout classes: `.eyebrow`, `.grid.cols-2`/`.cols-3`, `.card`,
  `.stat`/`.stat-label`, slide types `section.title.center`,
  `section.section.center`.
- Per-deck branding (accent color, etc.) is done by overriding CSS
  variables (`--accent`, `--bg`, `--text`, ...) in a `<style>` block in the
  deck's `index.html`, loaded *after* `lean.css`. Never hardcode colors —
  this is what makes dark mode work automatically.
- If a new deck needs a layout primitive that doesn't exist yet, add it to
  `shared/theme/lean.css` (not as deck-local inline CSS) so future decks
  benefit too.
- `shared/theme/template.html` is the starting point for new decks —
  Reveal.js via CDN (no npm install needed), Inter + JetBrains Mono fonts,
  highlight + notes plugins preconfigured.

## Language switcher (FR/EN/ES) & dark mode

Every deck ships with the `.deck-controls` toolbar from `template.html`
(language switcher + dark mode toggle), driven by `data-lang` and
`data-theme` attributes on `<html>` (persisted in `localStorage`).

- Every translatable text element exists as **three sibling elements**
  tagged `lang="fr"`, `lang="en"`, `lang="es"` — `lean.css` shows only the
  one matching `data-lang`. Elements without a `lang` attribute (code
  blocks, images, speaker notes) are always shown.
- `slide-builder` is responsible for producing all three language versions
  when generating a deck — `brief.md` itself stays French-only.
- Dark mode works purely through the CSS variables overridden under
  `:root[data-theme="dark"]` in `lean.css` — decks must not set colors
  outside of those variables.

## Niveau de détail (Light / Complet)

`.deck-controls` also includes a third toggle (`−` / `+`), driven by
`data-detail="light"|"full"` on `<html>` (persisted in `localStorage`,
mechanism mirrors `data-lang`/`data-theme`).

- Any element with class `.detail` is hidden in `light` mode (the default,
  used while presenting live) and shown in `full` mode (for self-guided
  reading afterwards).
- This is optional per deck — only add `.detail` blocks if the brief calls
  for a light/complete reading mode. Don't add the toggle button/script
  itself per deck, it's already in `template.html`/`lean.css`.

## Sub-project layout

```
<sous-projet>/
  brief.md            # output of slide-interview
  slides/
    index.html        # output of slide-builder, based on shared/theme/template.html
    assets/           # images/screenshots specific to this deck
```

## Previewing / exporting

No build tooling — just serve statically, e.g. from the deck's directory:

```
python3 -m http.server
```

PDF export: open `index.html?print-pdf` in the browser and print to PDF.

## Distribution / GitHub Pages

- `index.html` at the repo root is the landing page listing all decks
  (served as the GitHub Pages homepage). Add a card/link for each new deck
  here when it's ready to share.
- Decks are either **public** (link straight to
  `<sous-projet>/slides/index.html`) or **password-protected**.
- Password protection uses [staticrypt](https://github.com/robinmoisson/staticrypt)
  (client-side AES encryption, no server needed — fine for GitHub Pages).
  Workflow to (re-)generate the protected file after editing a deck:
  ```
  cd <sous-projet>/slides
  cp index.html index.protected.html
  staticrypt index.protected.html -p '<password>' --short -d . \
    --template-title "<Deck title>" \
    --template-color-primary "<accent color>" \
    --remember 30
  ```
  Commit both `index.html` (editable source) and `index.protected.html`
  (what the landing page links to) — plus the generated
  `.staticrypt.json` (salt config, keep stable across re-encryptions of
  the same deck).

## Current sub-projects

- `powerbi/` — Introduction à Power BI, 4h training, password-protected
  on the landing page (password shared separately with the audience, not
  stored in this repo).
