---
name: mini-web-app
description: Build small single-file web tools (trackers, calculators, converters, planners, simple dashboards) in the One Man AI house style - one index.html, works offline, mobile-first, light and dark mode, saves data in the browser, Polish zł and number formats by default. Use whenever the user asks for a small web app, an HTML tool, a mini tool, a tracker, a calculator or "a simple page that does X", even if they don't mention style or format.
---

# Mini web app

Build small tools that look like they come from the same studio and work the first time.

## Deliver exactly this

1. **One file: `index.html`.** HTML, CSS and JavaScript inline. No build step, no npm, and no external scripts or fonts unless the user asks. *Why: the user double-clicks the file and it works, even offline.*
2. **A short "How to use" note** in chat: 3 lines max.
3. **The test checklist** from `references/checklist.md`, ticked, with anything you could not check marked as not checked.

Start from `assets/template.html`. It already has the layout, the colour tokens, dark mode and the safe storage helpers. Fill in the parts marked `TODO`.

## Look and feel (brand)

| Token | Light | Dark | Use |
|---|---|---|---|
| `--bg` | `#F3F0E9` (cream) | `#0D1117` (navy) | Page background |
| `--card` | `#FFFFFF` | `#161B22` | Cards |
| `--ink` | `#0D1117` | `#F3F0E9` | Text |
| `--muted` | `#6B7280` | `#969CA6` | Secondary text |
| `--accent` | `#FFB020` (amber) | `#FFB020` | Primary button, key numbers, focus ring |

- System font stack, so nothing is downloaded. Headings are bold; numbers use `font-variant-numeric: tabular-nums`.
- Cards have a 14px radius and a soft shadow, with generous spacing (16–24px).
- **Mobile first:** must work at 360px wide with no horizontal scroll. Buttons are at least 44px tall.
- Small "ONE MAN AI" text in the footer. No logos of real brands.

## Behaviour rules (and why)

- **Money and numbers:** use `Intl.NumberFormat('pl-PL', {style: 'currency', currency: 'PLN'})` unless the user names another currency. *Why: the user and the audience are in Poland, and `$` looks wrong to them.*
- **Dates:** `DD.MM.YYYY` in the UI. Store ISO dates (`YYYY-MM-DD`).
- **Saving:** keep the user's data in `localStorage` via the template's `load()`/`save()` helpers, which are wrapped in try/catch. *Why: storage can be blocked in private windows, and the app must still work, just without saving.*
- **Validate every input:** show a clear inline message ("Enter an amount above 0") and never add an empty or negative entry.
- **Empty state:** when there is no data, show a friendly line plus what to do first, not a blank table.
- **Delete needs a confirm step**, and offer an "Export CSV" button whenever the app stores a list. *Why: users lose data by mistake, and the export is their backup.*
- **Accessibility:** every input has a `<label>`, colour is never the only signal, there's a visible focus ring, and contrast is AA.
- **No `innerHTML` with user text.** Use `textContent`. *Why: it prevents broken pages and script injection from typed text.*

## Before you answer

Open `references/checklist.md` and go through it. If you can run a browser or a headless check, do it. Otherwise say which items you checked by reading the code.
