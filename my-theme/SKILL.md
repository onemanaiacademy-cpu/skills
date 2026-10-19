---
name: My-Theme
description: >
  This is the user's personal saved UI/UX theme preference for ALL frontend work.
  ALWAYS read this skill before building any UI, web app, Chrome extension, dashboard,
  React component, or any visual interface — even if the user doesn't mention a theme.
  This skill overrides the theme-selection step in ui-bootstrap-guidelines: do NOT ask
  the user to pick a theme again; use the saved theme defined here directly.
  Trigger phrases: "build", "create", "design", "make a UI", "extension", "dashboard",
  "component", "page", "popup", "panel", "form", "widget", or any visual/frontend task.
  If the user says "change my theme", "update my theme", or "switch theme", open the
  theme picker and update this file with their new selection.
---

# My-Theme — Personal Theme Preference

This skill stores the user's saved UI/UX theme. Claude reads this **before** any frontend
build and applies it automatically — no need to ask for a theme.

---

## 🎨 Active Theme: Classic Light

**Theme ID:** `classic`  
**Last updated:** 2026-05-28

> To change your theme, say: **"change my theme"** or **"switch my theme"**  
> Claude will show the theme picker and update this file automatically.

---

## Theme: Classic Light — Full Spec

### Identity
- **Name:** Classic Light
- **Emoji:** 🪟
- **Feel:** Clean, professional, versatile — great all-rounder for Chrome extensions, dashboards, and web apps
- **Bootstrap Only:** ✅ Yes — pure CSS variable overrides, no extra libraries

### CSS Variables

```css
:root {
  /* Backgrounds */
  --bg-primary:      #F8FAFC;
  --bg-surface:      #FFFFFF;
  --bg-subtle:       #F1F5F9;
  --bg-hover:        #E2E8F0;

  /* Accent / Brand */
  --accent-primary:  #6366F1;   /* soft indigo */
  --accent-light:    #EEF2FF;
  --accent-hover:    #4F46E5;
  --accent-success:  #10B981;
  --accent-warning:  #F59E0B;
  --accent-danger:   #EF4444;

  /* Text */
  --text-primary:    #1E293B;
  --text-secondary:  #64748B;
  --text-muted:      #94A3B8;
  --text-on-accent:  #FFFFFF;

  /* Borders */
  --border-light:    #E2E8F0;
  --border-medium:   #CBD5E1;

  /* Shadows */
  --shadow-sm: 0 1px 3px rgba(0,0,0,0.06), 0 1px 2px rgba(0,0,0,0.04);
  --shadow-md: 0 4px 12px rgba(0,0,0,0.08), 0 2px 4px rgba(0,0,0,0.04);
  --shadow-lg: 0 10px 30px rgba(0,0,0,0.10), 0 4px 8px rgba(0,0,0,0.06);

  /* Radii */
  --radius-sm:   6px;
  --radius-md:   10px;
  --radius-lg:   16px;
  --radius-xl:   24px;
  --radius-pill: 999px;

  /* Typography */
  --font-heading: 'Poppins', sans-serif;
  --font-body:    'Inter', sans-serif;

  /* Motion */
  --transition-base: 0.18s cubic-bezier(0.4, 0, 0.2, 1);

  /* Bootstrap variable bridge */
  --bs-primary:       var(--accent-primary);
  --bs-border-radius: var(--radius-md);
  --bs-border-color:  var(--border-light);
  --bs-body-bg:       var(--bg-primary);
  --bs-body-color:    var(--text-primary);
  --bs-card-bg:       var(--bg-surface);
}
```

### Google Fonts
```html
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=Poppins:wght@500;600;700&display=swap" rel="stylesheet">
```

### Body / Root Background
```css
body { background: #F8FAFC; }
```

### Glass / Blur Effects
Not required for this theme. No `backdrop-filter` needed.

### Special Overrides
None — this theme works with Bootstrap 5 out of the box via `:root` variable overrides.

---

## How Claude Must Apply This Theme

When building any frontend:

1. **Inject the CSS variables block** above into the `<style>` tag or CSS file, at `:root` level
2. **Import Google Fonts** (Inter + Poppins) in the HTML `<head>` or `@import` in CSS
3. **Bridge Bootstrap vars** — include the `--bs-*` overrides so Bootstrap components inherit the palette
4. **Use ONLY the CSS variable names** for all colors, radii, shadows, fonts — never hardcode hex values
5. **Apply `.ext-compact`** on the root div of all Chrome extension popups (see ui-bootstrap-guidelines §2)
6. **Use `AccordionSection`** for all filter/settings/rules panels (see ui-bootstrap-guidelines §4.1)

---

## Quick Theme Switch Reference

If the user wants to change theme, present this picker and update the **Active Theme** section above:

| ID         | Theme Name           | Feel                          | Bootstrap Only |
|------------|---------------------|-------------------------------|----------------|
| `classic`  | 🪟 Classic Light     | Clean, indigo, professional   | ✅ Yes         |
| `material` | 🎨 Google Material 3 | Tonal, ripple, Roboto         | ✅ Yes         |
| `ios`      | 🍎 iOS 26 Liquid Glass | Frosted glass, translucent  | ⚠️ +30 lines  |
| `dark`     | 🌙 Dark Pro          | Rich dark, vivid accent       | ✅ Yes         |
| `fluent`   | 🪟 Windows Fluent    | Acrylic, Reveal, Segoe UI     | ⚠️ +30 lines  |

Full variable specs for each theme are in the ui-bootstrap-guidelines skill, Section 3.

---

## Anti-Patterns for This Theme (Classic Light)

| ❌ Never                          | ✅ Always                          |
|----------------------------------|------------------------------------|
| Dark or black backgrounds        | `var(--bg-primary)` #F8FAFC        |
| Purple gradients or neon accents | Soft indigo `var(--accent-primary)`|
| `border-radius: 0`               | Minimum `var(--radius-md)` (10px)  |
| Raw hex colors in code           | CSS variables only                 |
| Flat cards with no shadow        | `var(--shadow-sm)` minimum         |
| `transition: none`               | `var(--transition-base)` always    |
