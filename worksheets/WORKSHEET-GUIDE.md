# Worksheet Subsystem Guide

## Overview

The `worksheets/` folder is a self-contained subsystem for producing polished, printable scale practice worksheets. Each worksheet is an HTML page that links to shared CSS and can be exported as a PDF via browser print.

The Piano Exercise Matrix (`index.html`, `style.css`, `script.js`) lives at the repo root and has its own styling. The worksheet pages do not use Matrix styles and should never import them.

---

## Folder Structure

```
piano-exercise-matrix/
├── index.html                  ← Piano Exercise Matrix (app)
├── style.css                   ← Matrix styles (do not use in worksheets)
├── script.js                   ← Matrix logic
├── README.md
│
└── worksheets/                 ← Worksheet subsystem root
    ├── TEMPLATE.html           ← Annotated starter template
    ├── WORKSHEET-GUIDE.md      ← This file
    │
    ├── css/
    │   └── worksheet.css       ← Shared stylesheet (screen + print)
    │
    ├── drafts/                 ← Plain-text drafts from transcripts
    │   └── C_Major_Practice_Worksheet.txt
    │
    ├── pdf/                    ← Exported PDF files
    │   └── c-major.pdf
    │
    └── c-major.html            ← Finished worksheet page
```

---

## Workflow: Transcript → Worksheet

Every future transcript follows the same pipeline:

1. **Draft** — Feed transcript through processing. Save the plain-text worksheet draft into `worksheets/drafts/` (e.g. `D_Major_Practice_Worksheet.txt`).

2. **Review** — Read through the draft. Approve, edit, or restructure the content.

3. **Template** — Copy `TEMPLATE.html`, rename it (e.g. `d-major.html`), and fill in the approved content using the existing HTML classes.

4. **Export PDF** — Open the HTML file in a browser, print to PDF (Ctrl/Cmd+P → Save as PDF, A4, no headers/footers). Save the PDF into `worksheets/pdf/`.

5. **Link** — Update the download button `href` to point to `pdf/d-major.pdf`.

That's it. No new classes, no new CSS, no new page structure.

---

## Adding a New Worksheet Page

### Step-by-step

1. **Copy the template:**
   ```
   cp worksheets/TEMPLATE.html worksheets/g-major.html
   ```

2. **Replace placeholders** in the new file:

   | Placeholder         | Example value                |
   |---------------------|------------------------------|
   | `{{KEY_NAME}}`      | G Major                      |
   | `{{KEY_SIGNATURE}}` | One sharp (F♯)               |
   | `{{PDF_FILENAME}}`  | g-major.pdf                  |
   | `{{DOMINANT_CHORD}}`| D7                           |
   | `{{EXERCISE_TITLE}}`| Scale — Parallel Motion      |
   | `{{EXERCISE_DESCRIPTION}}` | Play G major up and down... |
   | `{{TIP_TEXT}}`       | The F♯ gives you a black-key landmark... |

3. **Duplicate `.ws-card` blocks** for each exercise. Number them sequentially with `.ws-card-number`.

4. **Use section labels and dividers** to group exercises:
   - `.ws-section-label` for group headings
   - `.ws-divider` between groups

5. **Export to PDF** and save in `worksheets/pdf/`.

### Naming convention

| File type    | Pattern                | Example            |
|-------------|------------------------|--------------------|
| HTML page   | `{key}-{quality}.html` | `d-major.html`     |
| PDF export  | `{key}-{quality}.pdf`  | `d-major.pdf`      |
| Text draft  | `{Key}_{Quality}_Practice_Worksheet.txt` | `D_Major_Practice_Worksheet.txt` |

Use lowercase with hyphens for HTML/PDF. Use title case with underscores for text drafts (matching the existing convention).

---

## CSS Class Reference

All worksheet classes are prefixed with `ws-` to avoid collision with Matrix styles.

### Layout

| Class | Purpose |
|-------|---------|
| `.ws-page` | Outer page container (max-width, centering, padding) |
| `.ws-header` | Centered page header |
| `.ws-header-title` | Large key name (e.g. "C Major") |
| `.ws-header-meta` | Subtitle line (key signature, octave range) |
| `.ws-footer` | Centered footer text |

### Sections

| Class | Purpose |
|-------|---------|
| `.ws-section-label` | Uppercase spaced-letter section heading |
| `.ws-divider` | Thin horizontal line between sections |
| `.ws-page-break` | Force a page break before this element in print |

### Cards

| Class | Purpose |
|-------|---------|
| `.ws-card` | White bordered card — one per exercise |
| `.ws-card-header` | Flex row with number + title |
| `.ws-card-number` | Gray exercise number (01, 02, ...) |
| `.ws-card-title` | Bold exercise name |
| `.ws-card-body` | Indented body text area |

### Callouts

| Class | Purpose |
|-------|---------|
| `.ws-tip` | Blue-tinted callout box for tips or technique notes |
| `.ws-tip-label` | Small uppercase label inside a tip ("TIP", "TECHNIQUE") |

### Typography

| Class | Purpose |
|-------|---------|
| `.ws-chord` | Serif bold for chord/note names (C – E – G) |
| `.ws-fingering` | Gray pill badge for fingering info (4th finger on D) |

### Special cards

| Class | Purpose |
|-------|---------|
| `.ws-principles-card` | Two-column grid of practice principles |
| `.ws-principles-list` | Grid container inside principles card |
| `.ws-principle` | Single principle item with dash prefix |
| `.ws-highlight-card` | Centered quote/highlight (e.g. Chopin note) |
| `.ws-highlight-label` | Uppercase label inside highlight card |
| `.ws-highlight-keys` | Serif bold key display inside highlight card |

### Print control

| Class | Purpose |
|-------|---------|
| `.ws-screen-only` | Hidden in print (add to any element) |
| `.ws-page-break` | Forces a page break before this element |

---

## PDF Export Settings

When printing to PDF from a browser:

- **Paper size:** A4
- **Margins:** Default (the CSS `@page` rule sets 20mm)
- **Headers/Footers:** Disable (uncheck in browser print dialog)
- **Background graphics:** Enable (so tip backgrounds print)
- **Scale:** 100%

The print stylesheet automatically hides the download bar and tightens spacing for a clean PDF.

---

## Design Decisions

- **`ws-` prefix**: Every class is namespaced to prevent collision with the Matrix app styles. The Matrix uses unprefixed classes (`card`, `tip`, etc.) — these are completely separate.

- **Single shared CSS file**: Print rules are embedded in `worksheet.css` via `@media print` rather than a separate file. This keeps the link tag count to one and avoids missed includes.

- **No JavaScript**: Worksheets are static documents. No JS needed, no framework dependencies.

- **Page-break safety**: Every card has `page-break-inside: avoid`. Section labels have `page-break-after: avoid` to stay attached to their first card.

- **Principles are stable**: The General Practice Principles and Chopin note appear on every worksheet. They live in the template and should only change if the source material changes.
