# CLAUDE.md

Guidance for AI assistants working in this repository.

## What this is

A single-page personal engineering portfolio for **Trevor North** (mechanical
engineering student, University of Missouri). The entire site is one
self-contained file: **`index.html`**. There is no build step, no framework, no
package manager, no backend, and no dependencies to install.

- **Static site.** Open `index.html` in a browser to view it — that is the whole
  app. Nothing needs to be compiled or served (though a local static server like
  `python3 -m http.server` is fine for previewing).
- **Everything is inline.** HTML, CSS (in one `<style>` block in `<head>`), and
  JavaScript (one `<script>` before `</body>`) all live in `index.html`.
- **Images are embedded as base64 data URIs.** This is why the file is ~4 MB
  despite being only ~900 lines. There is no `images/` directory — every
  `<img src="data:image/...;base64,...">` carries its own binary inline.

## Repository layout

```
Portfolio/
├── index.html   ← the entire site (HTML + CSS + JS + embedded images)
└── CLAUDE.md    ← this file
```

That's it. Do not expect other source files, config, or tooling.

## Structure of `index.html`

Read top to bottom, the file is organized as:

1. **`<head>`** — meta tags, an author-facing "HOW TO ADD NEW ITEMS" comment
   block, Google Fonts (`Archivo Black`, `IBM Plex Mono`, `IBM Plex Sans`), and
   one big `<style>` block.
2. **CSS** — a `:root` block of design tokens (custom properties), then component
   sections each marked with a `/* ---------- name ---------- */` comment:
   `top bar`, `shell`, `hero / title block`, `sheets`, `tabs`, `record rail`,
   `plates`, `awards`, `text-only plate`, `empty state`, `role card`,
   `lightbox`, `footer`.
3. **`<body>`** — marked out with `<!-- ===== SECTION ===== -->` banners:
   - **Topbar** — sticky identity bar.
   - **Hero** — name, photo, lede, and a `titleblock` of key/value fields.
   - **Sheet 01 — Academic** (`#academic`) — three tab panels: Honors & Awards,
     Hands-On Projects, Conceptual Projects.
   - **Sheet 02 — Professional** (`#professional`) — a `role` card plus two tab
     panels: Engineering (currently empty, shows an `empty` state) and
     Automation.
   - **Lightbox** — a hidden `<div id="lb">` dialog for viewing images full-size.
4. **`<script>`** — a single IIFE with two behaviors: accessible tabs (roving
   focus + arrow keys) and the image lightbox (open on plate click, close on
   Esc / outside click).

## Current portfolio contents

Snapshot of the work currently on the site (as of Rev 2026.07). Use this as a
map of what already exists before adding or editing items; keep it current when
the content changes.

**Hero / identity.** Trevor North — Mechanical Engineering, University of
Missouri (Mizzou), based in St. Louis, MO. Engineering & Automation intern in
defense/aerospace. Graduating May 2027, 3.34 GPA, Dean's List 5 semesters, open
to 2027 roles.

**Sheet 01 — Academic** (`#academic`)

- *Honors & Awards* (tab count 6): Curator's Scholar Award (text `award` entry),
  plus five Dean's List plates — High Dean's List Spring 2026 (A-01), Dean's List
  Spring 2025 (A-02), Dean's List Fall 2024 (A-03), High Dean's List Spring 2024
  (A-04), Dean's List Fall 2023 (A-05). A `rail` of chips summarizes the same
  record.
- *Hands-On Projects* (tab count 5): Turned shaft & milled block — Machining Lab
  (B-01); Breadboard circuit build (B-02), Instrumentation bench setup (B-03),
  Signal measurement (B-04), Bench measurement (B-05) — all Instruments &
  Measurements Lab.
- *Conceptual Projects* (tab count 2): Gearbox assembly drawing (C-01) and
  Two-stage gear train model (C-02) — Gear Box Assembly Project for Machine
  Element Design.

**Sheet 02 — Professional** (`#professional`)

Role card: **Riverbend Energetics** — Engineering & Automation Intern, Summer
2026. A `[ RELEASE ]` note states that only material approved for public release
appears here (nothing proprietary or export-controlled).

- *Engineering* (tab count 0): empty — shows the "Cleared work goes here"
  `empty` state pending release.
- *Automation*: eight plates (E-01…E-08) — Frame sealing, Spindle guarding,
  Pneumatic flow control, Enclosure door locks, Bracket surface prep, Stepper
  motor assembly (NEMA 23, 23HS30-2804S · 1.9 N·m · 2.8 A), Control enclosure
  wiring, Equipment cart fabrication. **Note:** the tab `count` currently reads
  **9** but only 8 plates are present — the count is out of sync and should be
  corrected to 8 (or a ninth item added) next time this panel is edited.

## Core content conventions

The most common edit is adding or changing a portfolio item. Two block types:

**Image item — `<figure class="plate">`** (the primary pattern, ~21 of them):
```html
<figure class="plate">
  <button class="plate-btn" data-cap="Caption shown in lightbox">
    <img src="data:image/jpeg;base64,..." alt="Descriptive alt text">
  </button>
  <figcaption class="plate-cap">
    <span class="cap-idx mono"><span>A-01</span></span>
    <b class="cap-title">Item name</b>
    <span class="cap-meta mono">Date · detail line</span>
  </figcaption>
</figure>
```
Per the in-file guide, adding an item = copy an existing plate and change the
`<img src>`, the `cap-title`, and the `cap-meta`. The `plate-btn` wraps the image
so JS can open it in the lightbox; keep `data-cap` in sync with the caption.

**Text award — `<li class="award">`** inside `<ul class="awards">`: an
`award-tag`, a `cap-title`, a `cap-meta`, and a `<p>` description. Used for
honors that have no image.

Other conventions:
- **Tab counts.** Each tab button has a `<span class="count">N</span>` that states
  how many items are in its panel. Update this number when you add or remove
  items in that panel.
- **Empty panels** use the `empty` block (e.g. the Professional → Engineering
  panel), with `empty-code`, an `<h4>`, and guidance text.
- **`.mono` class** applies the IBM Plex Mono font — used for meta/label text.
- **`hidden` attribute** on non-active tab panels is toggled by the JS; the first
  panel in each tablist is visible, the rest carry `hidden`.

## Design system

Colors, spacing, and other design values are CSS custom properties on `:root`
(e.g. `--ink`, `--graphite`, `--steel`, `--brass`, `--panel`, `--sp`). Reuse
these tokens rather than hardcoding new hex values, so the "technical drawing /
blueprint" aesthetic stays consistent. The look is intentionally engineering-
drawing themed: grid background, brass accent, monospace metadata, sheet/plate
terminology.

## Accessibility — preserve it

The markup is deliberately accessible; keep it that way when editing:
- Tabs use `role="tablist"` / `role="tab"` / `role="tabpanel"` with
  `aria-selected`, `aria-controls`, `aria-labelledby`, and roving `tabindex`.
- The lightbox is `role="dialog" aria-modal="true"` and manages focus
  (focuses close button on open, restores focus on close).
- Every `<img>` needs meaningful `alt` text.
- `prefers-reduced-motion` is respected in CSS.

## Editing guidance

- **Make all changes in `index.html`.** There is nowhere else to put code.
- **Don't add a build system, framework, or dependencies** unless explicitly
  asked — the single-file, zero-dependency design is the point.
- When adding images, embed them as base64 data URIs to match the existing
  pattern (the site is meant to be a portable single file). Be aware this grows
  the file; large images noticeably increase load size.
- The `Rev 2026.07` string appears in the topbar-area eyebrow and footer; the
  content is periodically refreshed, so keep revision markers consistent if you
  bump them.
- Match the surrounding indentation and the existing comment style
  (`<!-- ===== ... ===== -->` banners, `/* ---------- ... ---------- */` CSS
  section markers).

## Verifying changes

There are no tests or linters. To verify:
1. Open `index.html` in a browser (or serve it locally).
2. Check that tabs switch, images open in the lightbox and close via Esc/outside
   click, and layout holds at mobile and desktop widths.
3. Confirm tab `count` numbers still match the number of items shown.

## Chat memory — the "End Chat" clause

The user keeps a running memory of their chats in this repo. This is **manual,
on request only** — never write memory automatically.

When the user says **"End Chat"** (or an equivalent like "end the chat" / "save
memory"), before finishing:

1. Write a digest of the current conversation to `memory/<YYYY-MM-DD>.md`, using
   the date the chat took place. If a file for that date already exists, append
   the new session **below a `---` divider** — do not overwrite it.
2. Structure the file for fast AI ingestion (it exists so a future assistant can
   get up to speed quickly), e.g.:
   - a one-line topic header with the date,
   - **Requests** — a bullet list of what the user asked for,
   - **Outcomes** — key decisions and changes made, with files touched,
   - **Open items** — anything unfinished or to follow up on.
3. Commit to `main` with a message like `chat memory <YYYY-MM-DD>` and push.

If more than one repository is loaded in the session, **ask the user which repo
(or "all") the memory should be saved into** before writing — the digest belongs
with the repo the chat concerns. With only this repo loaded, no need to ask.

Only create or modify a memory file when the user explicitly asks (by saying
"End Chat" or similar). The `memory/` directory is the store for these files.

## Git workflow

- Remote: `https://github.com/northpoole/Portfolio` (default branch `main`).
- Commit with clear, descriptive messages. Only open a pull request when the user
  explicitly asks for one.
