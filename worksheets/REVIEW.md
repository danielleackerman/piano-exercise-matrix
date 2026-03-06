# Worksheet Subsystem — Structure Review & Refactoring Notes

## 1. Review of Current Worksheet HTML

The existing `C_Major_Worksheet.html` is well-designed but structurally a one-off:

**What works well:**
- Clean card-based layout with consistent visual hierarchy
- Good use of section labels, dividers, and callout boxes
- Print-aware CSS (`@page`, `page-break-inside: avoid`, print media query)
- Typography system: serif bold for chord names, pill badges for fingering, uppercase spaced labels for sections
- The design language is distinct from the Matrix app — that separation is correct and worth preserving

**What needs refactoring:**
- All CSS is embedded in `<style>` inside the HTML — duplicating this for every new key would create a maintenance problem
- Class names are generic (`.card`, `.tip`, `.chord`) — these collide with the Matrix's own class names if the pages ever coexist in the same context
- No template structure — adding a D Major worksheet means copying the entire file and carefully replacing content while leaving structure intact
- The PDF download link is hardcoded with a sibling-relative path — needs a predictable folder convention
- The plain-text draft (`C_Major_Practice_Worksheet.txt`) lives at the repo root alongside the Matrix app files

## 2. What Changed in the Refactor

| Before | After |
|--------|-------|
| All CSS inline in each HTML file | Single shared `worksheet.css` linked by all pages |
| Generic class names (`.card`, `.tip`) | Namespaced with `ws-` prefix (`.ws-card`, `.ws-tip`) |
| No template file | `TEMPLATE.html` with annotated placeholders |
| Files at repo root | Everything under `worksheets/` subfolder |
| No draft storage convention | `worksheets/drafts/` for plain-text drafts |
| PDF next to HTML | `worksheets/pdf/` for all exported PDFs |
| No documentation | `WORKSHEET-GUIDE.md` with class reference and workflow |

## 3. Recommended Repo Structure

```
piano-exercise-matrix/
├── index.html                      ← Matrix app
├── style.css                       ← Matrix styles
├── script.js                       ← Matrix logic
├── README.md                       ← Repo readme
│
└── worksheets/                     ← Worksheet subsystem
    ├── TEMPLATE.html               ← Copy this to start a new worksheet
    ├── WORKSHEET-GUIDE.md          ← Usage guide and class reference
    ├── css/
    │   └── worksheet.css           ← Shared stylesheet (screen + print)
    ├── drafts/
    │   └── C_Major_Practice_Worksheet.txt
    ├── pdf/
    │   └── c-major.pdf
    └── c-major.html                ← Refactored C Major worksheet
```

## 4. Print / PDF Notes

The print styles are embedded in `worksheet.css` via `@media print` — no separate print stylesheet needed. This avoids an extra `<link>` tag and keeps the system to one CSS file.

Key print behaviors:
- Download bar hidden automatically
- Cards never split across pages
- Section labels stay attached to their first card
- Tip backgrounds preserved (`print-color-adjust: exact`)
- Spacing tightened slightly for better page density
- `.ws-page-break` class available to force breaks where needed
- `.ws-screen-only` class to hide any element from print

Export settings: A4, no browser headers/footers, background graphics enabled, 100% scale.

## 5. Future Worksheet Pipeline

```
Transcript → Plain-text draft → Review → Fill template → Export PDF
                  ↓                           ↓              ↓
          drafts/X_Major_...txt        x-major.html     pdf/x-major.pdf
```

No CSS changes needed. No class invention. No structure decisions. Just content into slots.
