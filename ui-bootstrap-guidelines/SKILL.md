---
name: ui-bootstrap-guidelines
description: >
  Apply this skill whenever the user asks to build any UI, web app, dashboard, Chrome extension,
  React component, or frontend interface — especially when they want a smooth, modern, clean look.
  This skill enforces Bootstrap 5 + React with a theme system (Classic Light, Google Material,
  iOS 26 Liquid Glass, Windows Fluent, Dark Pro). Trigger for ANY of the following: "build me an app",
  "create a UI", "make a Chrome extension", "design a dashboard", "create a React component",
  "build a web page", or any time the user mentions Bootstrap, light theme, dark theme, minimal,
  smooth, Material, iOS 26, Liquid Glass, Fluent, or a specific theme name.
  If the My-Theme skill is present, read it first and apply the saved theme — do NOT ask
  the user to pick again. If My-Theme is absent, show the theme picker.
  Never skip this skill when building interactive frontends.
---

# UI Design Guidelines — Bootstrap 5 + React + Theme System

Read this entire document before writing a single line of code. All rules are non-negotiable.

---

## ⚡ STEP 0 — ALWAYS DO THIS FIRST: Resolve Theme

**Check for `My-Theme` skill first:**
- If the `My-Theme` skill is available and loaded → read it, use its **Active Theme** directly. Skip the picker entirely. Do NOT ask the user to choose a theme again.
- If `My-Theme` is NOT available → show the picker below and ask the user to choose.

```
Which UI/UX theme would you like?

A) 🪟 Classic Light       — clean Bootstrap default, soft indigo accent, great all-rounder
B) 🎨 Google Material 3   — Material Design 3, tonal color roles, ripple effects, Roboto
C) 🍎 iOS 26 Liquid Glass — frosted glass, translucent panels, Apple SF Pro feel
D) 🌙 Dark Pro            — rich dark surfaces, vivid accent, developer/power-user feel
E) 🪟 Windows Fluent      — Acrylic/Mica blur, Microsoft Fluent Design, Segoe UI
```

Once theme is resolved (from `My-Theme` or picker), apply ONLY that theme's variables and
patterns from Section 3.

---

## 1. Stack Defaults

| Layer       | Default Choice                            |
|-------------|------------------------------------------|
| Framework   | **React** (functional components + hooks) |
| Styling     | **Bootstrap 5** + CSS custom properties  |
| Icons       | **lucide-react** or Bootstrap Icons      |
| Motion      | CSS transitions + subtle keyframes       |
| Fonts       | Theme-specific (see Section 3)           |

**Chrome Extensions**: React + Bootstrap in popup. Popup width ≤ 400px. Height 480–600px.  
**Plain HTML tasks**: Bootstrap 5 CDN + vanilla JS. Same design rules.

---

## 2. Universal Layout & Spacing Rules

These apply to ALL themes — never override them.

```
Page padding:    py-4 px-3 (mobile) → py-5 px-4 (desktop)
Card gaps:       gap-3 minimum
Section gaps:    mb-4 or mb-5 between major blocks
Sticky header:   56–64px tall; content gets matching padding-top
Grid system:     ALWAYS use container / row / col-* — never manual float
Flex helpers:    d-flex, align-items-center, justify-content-between, gap-2/3
```

### 🔑 Chrome Extension Sizing (CRITICAL — always enforce)

```css
/* popup.css */
body {
  width: 380px;
  min-height: 480px;
  max-height: 600px;
  overflow-y: auto;
  overflow-x: hidden;
}

/* COMPACT mode for extension popups */
.ext-compact .card       { padding: 10px 12px !important; }
.ext-compact .btn        { padding: 6px 14px !important; font-size: 13px; }
.ext-compact h5, .ext-compact h6 { font-size: 14px; }
.ext-compact .form-label { font-size: 12px; margin-bottom: 3px; }
.ext-compact .mb-3       { margin-bottom: 8px !important; }
.ext-compact .gap-3      { gap: 8px !important; }
```

Add `class="ext-compact"` to the root `<div>` of ALL Chrome extension popups.

### 📐 Typography Scale (Universal)

```
11px  xs  — captions, helper text
13px  sm  — form labels, secondary info
15px  base — body text
17px  md  — emphasized body
20px  lg  — section headings
24px  xl  — panel headers
30px  2xl — page titles (rarely used in extensions)
```

Minimum font size: **11px**. Line height: body 1.5–1.7, headings 1.2–1.3.  
**Never use default Bootstrap font sizes unmodified in extension popups** — they are too large.

---

## 3. Theme Definitions

---

### Theme A — Classic Light (Bootstrap Default + Soft Indigo)

Best for: general web apps, dashboards, Chrome extensions, professional tools.

```css
:root {
  --bg-primary:      #F8FAFC;
  --bg-surface:      #FFFFFF;
  --bg-subtle:       #F1F5F9;
  --bg-hover:        #E2E8F0;
  --accent-primary:  #6366F1;
  --accent-light:    #EEF2FF;
  --accent-hover:    #4F46E5;
  --accent-success:  #10B981;
  --accent-warning:  #F59E0B;
  --accent-danger:   #EF4444;
  --text-primary:    #1E293B;
  --text-secondary:  #64748B;
  --text-muted:      #94A3B8;
  --text-on-accent:  #FFFFFF;
  --border-light:    #E2E8F0;
  --border-medium:   #CBD5E1;
  --shadow-sm:  0 1px 3px rgba(0,0,0,0.06), 0 1px 2px rgba(0,0,0,0.04);
  --shadow-md:  0 4px 12px rgba(0,0,0,0.08), 0 2px 4px rgba(0,0,0,0.04);
  --shadow-lg:  0 10px 30px rgba(0,0,0,0.10), 0 4px 8px rgba(0,0,0,0.06);
  --radius-sm:   6px;
  --radius-md:   10px;
  --radius-lg:   16px;
  --radius-xl:   24px;
  --radius-pill: 999px;
  --font-heading: 'Poppins', sans-serif;
  --font-body:    'Inter', sans-serif;
  --transition-base: 0.18s cubic-bezier(0.4,0,0.2,1);
}
```

Google Fonts import: `Inter:wght@400;500;600` + `Poppins:wght@500;600;700`

---

### Theme B — Google Material Design 3

Best for: Android-adjacent apps, productivity tools, Google Workspace integrations.

Key visual language: tonal color roles, large rounded corners (28px+), ripple on tap,
M3 typography scale, surface tints, no hard shadows (use elevation via tint instead).

```css
:root {
  /* M3 Color Roles */
  --bg-primary:      #FFFBFE;   /* surface */
  --bg-surface:      #FFFBFE;
  --bg-subtle:       #F3EFF4;   /* surface-variant */
  --bg-hover:        #E7E0EC;
  --accent-primary:  #6750A4;   /* primary */
  --accent-light:    #EADDFF;   /* primary-container */
  --accent-hover:    #4F378B;   /* on-primary-container */
  --accent-success:  #386A20;
  --accent-warning:  #7D5700;
  --accent-danger:   #BA1A1A;
  --text-primary:    #1C1B1F;   /* on-surface */
  --text-secondary:  #49454F;   /* on-surface-variant */
  --text-muted:      #79747E;
  --text-on-accent:  #FFFFFF;   /* on-primary */
  --border-light:    #CAC4D0;   /* outline-variant */
  --border-medium:   #79747E;   /* outline */
  --shadow-sm:  0 1px 2px rgba(0,0,0,0.05);
  --shadow-md:  0 2px 8px rgba(0,0,0,0.08);  /* M3 uses tint over shadow */
  --shadow-lg:  0 4px 16px rgba(0,0,0,0.10);
  --radius-sm:   4px;
  --radius-md:   12px;   /* M3 medium = 12 */
  --radius-lg:   16px;   /* M3 large  = 16 */
  --radius-xl:   28px;   /* M3 extra-large */
  --radius-pill: 999px;
  --font-heading: 'Google Sans', 'Roboto', sans-serif;
  --font-body:    'Roboto', sans-serif;
  --transition-base: 0.2s cubic-bezier(0.2,0,0,1); /* M3 easing */
}
```

Google Fonts import: `Roboto:wght@300;400;500;700`

**M3-specific patterns:**
- State layers: hover adds 8% primary tint over surface; pressed adds 12%
- Ripple effect on buttons: use Bootstrap's btn + `position:relative overflow:hidden` + CSS `::after` radial-gradient keyframe
- Cards: use `--radius-xl` (28px), very light shadow, subtle tint fill on surface-variant
- FAB: round pill button, prominent, `--accent-primary` fill, 56px height
- Top app bar: `--bg-subtle` background, no divider, 64px height
- No outline on text fields in rest state — underline only (or use filled variant)

```css
/* M3 Ripple (add to buttons) */
.btn-m3 {
  position: relative;
  overflow: hidden;
  border-radius: var(--radius-pill);
  background: var(--accent-primary);
  color: var(--text-on-accent);
  border: none;
  padding: 10px 24px;
  font-weight: 500;
  letter-spacing: 0.01em;
  transition: box-shadow var(--transition-base);
}
.btn-m3::after {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at center, rgba(255,255,255,0.3) 0%, transparent 70%);
  opacity: 0;
  transition: opacity 0.3s ease;
}
.btn-m3:hover::after { opacity: 1; }
.btn-m3:active { transform: scale(0.97); }
```

---

### Theme C — iOS 26 Liquid Glass

Best for: premium, Apple-adjacent extensions, media apps, aesthetic tools.

Key visual language: frosted glass panels (backdrop-filter), translucent surfaces,
floating controls, SF Pro–like typography, minimal borders, depth through blur layers.

```css
:root {
  /* Base — works over any background/wallpaper */
  --bg-primary:      rgba(242,242,247,0.85);
  --bg-surface:      rgba(255,255,255,0.72);      /* glass panel */
  --bg-subtle:       rgba(255,255,255,0.45);
  --bg-hover:        rgba(255,255,255,0.60);
  --glass-blur:      blur(20px) saturate(180%);   /* backdrop-filter value */
  --glass-border:    rgba(255,255,255,0.55);
  --accent-primary:  #007AFF;   /* iOS blue */
  --accent-light:    rgba(0,122,255,0.12);
  --accent-hover:    #0062CC;
  --accent-success:  #34C759;
  --accent-warning:  #FF9F0A;
  --accent-danger:   #FF3B30;
  --text-primary:    #1C1C1E;
  --text-secondary:  #3C3C43;
  --text-muted:      rgba(60,60,67,0.45);
  --text-on-accent:  #FFFFFF;
  --border-light:    rgba(60,60,67,0.12);
  --border-medium:   rgba(60,60,67,0.22);
  --shadow-sm:  0 2px 8px rgba(0,0,0,0.06);
  --shadow-md:  0 8px 24px rgba(0,0,0,0.10);
  --shadow-lg:  0 20px 48px rgba(0,0,0,0.14);
  --radius-sm:   8px;
  --radius-md:   12px;
  --radius-lg:   18px;   /* iOS large = 18 */
  --radius-xl:   22px;
  --radius-pill: 999px;
  --font-heading: -apple-system, 'SF Pro Display', 'Helvetica Neue', sans-serif;
  --font-body:    -apple-system, 'SF Pro Text', 'Helvetica Neue', sans-serif;
  --transition-base: 0.25s cubic-bezier(0.25,0.46,0.45,0.94); /* iOS ease */
}
```

**Liquid Glass card pattern (the signature element):**

```css
.glass-card {
  background: var(--bg-surface);
  backdrop-filter: var(--glass-blur);
  -webkit-backdrop-filter: var(--glass-blur);
  border: 1px solid var(--glass-border);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-md),
              inset 0 1px 0 rgba(255,255,255,0.6);   /* specular top highlight */
}
.glass-card:hover {
  background: var(--bg-hover);
  box-shadow: var(--shadow-lg),
              inset 0 1px 0 rgba(255,255,255,0.7);
}

/* Tinted glass variant */
.glass-card-tinted {
  background: rgba(0,122,255,0.08);
  backdrop-filter: var(--glass-blur);
  border: 1px solid rgba(0,122,255,0.18);
  border-radius: var(--radius-lg);
}
```

**iOS-style segmented control (replaces tabs):**

```css
.ios-segment {
  background: rgba(118,118,128,0.12);
  border-radius: var(--radius-md);
  padding: 2px;
  display: inline-flex;
}
.ios-segment-btn {
  border: none;
  background: transparent;
  border-radius: calc(var(--radius-md) - 2px);
  padding: 5px 14px;
  font-size: 13px;
  font-weight: 500;
  color: var(--text-secondary);
  transition: all var(--transition-base);
}
.ios-segment-btn.active {
  background: rgba(255,255,255,0.9);
  backdrop-filter: blur(8px);
  color: var(--text-primary);
  box-shadow: 0 1px 4px rgba(0,0,0,0.12);
}
```

> ⚠️ **Bootstrap only caveat**: `backdrop-filter` is pure CSS — Bootstrap doesn't block it.
> Add `.glass-card` as a custom class alongside Bootstrap's `.card` utility classes.
> The translucent effect requires a colorful/image background to show properly — for extension
> popups, set a gradient on `body` (e.g. `background: linear-gradient(135deg,#e0f2fe,#f0e6ff)`).

---

### Theme D — Dark Pro

Best for: developer tools, code editors, analytics dashboards, power-user extensions.

```css
:root {
  --bg-primary:      #0F1117;
  --bg-surface:      #1A1D27;
  --bg-subtle:       #22263A;
  --bg-hover:        #2A2F45;
  --accent-primary:  #7C8BFF;   /* bright indigo on dark */
  --accent-light:    rgba(124,139,255,0.15);
  --accent-hover:    #9AA8FF;
  --accent-success:  #4ADE80;
  --accent-warning:  #FCD34D;
  --accent-danger:   #F87171;
  --text-primary:    #E2E8F0;
  --text-secondary:  #94A3B8;
  --text-muted:      #475569;
  --text-on-accent:  #FFFFFF;
  --border-light:    #2D3748;
  --border-medium:   #4A5568;
  --shadow-sm:  0 1px 3px rgba(0,0,0,0.4);
  --shadow-md:  0 4px 16px rgba(0,0,0,0.5);
  --shadow-lg:  0 12px 36px rgba(0,0,0,0.6);
  --radius-sm:   6px;
  --radius-md:   10px;
  --radius-lg:   14px;
  --radius-xl:   20px;
  --radius-pill: 999px;
  --font-heading: 'JetBrains Mono', 'Fira Code', monospace;   /* for dev tools */
  --font-body:    'Inter', sans-serif;
  --transition-base: 0.16s cubic-bezier(0.4,0,0.2,1);
}
/* Override Bootstrap's default text/bg for dark */
body { background: var(--bg-primary); color: var(--text-primary); }
.card { background: var(--bg-surface); border-color: var(--border-light); }
.form-control { background: var(--bg-subtle); color: var(--text-primary); border-color: var(--border-medium); }
.form-control:focus { background: var(--bg-hover); border-color: var(--accent-primary); color: var(--text-primary); }
```

Google Fonts import: `Inter:wght@400;500;600` + `JetBrains+Mono:wght@400;500` (optional)

---

### Theme E — Windows Fluent (Acrylic / Mica)

Best for: Windows-native feel, productivity apps, enterprise tools, Edge extensions.

Key visual language: Acrylic blur (like iOS but more muted), Segoe UI, reveal highlight
on hover (light sweep effect), tight spacing, subtle depth, Microsoft accent colors.

```css
:root {
  --bg-primary:      rgba(243,243,243,0.9);
  --bg-surface:      rgba(255,255,255,0.78);      /* Acrylic */
  --bg-subtle:       rgba(0,0,0,0.04);
  --bg-hover:        rgba(0,0,0,0.07);
  --acrylic-blur:    blur(30px) saturate(125%);
  --acrylic-border:  rgba(0,0,0,0.08);
  --accent-primary:  #0078D4;   /* MS fluent blue */
  --accent-light:    #EBF3FB;
  --accent-hover:    #005A9E;
  --accent-success:  #107C10;
  --accent-warning:  #D83B01;
  --accent-danger:   #A80000;
  --text-primary:    #201F1E;
  --text-secondary:  #605E5C;
  --text-muted:      #A19F9D;
  --text-on-accent:  #FFFFFF;
  --border-light:    rgba(0,0,0,0.07);
  --border-medium:   rgba(0,0,0,0.14);
  --shadow-sm:  0 2px 4px rgba(0,0,0,0.07);
  --shadow-md:  0 4px 16px rgba(0,0,0,0.10);
  --shadow-lg:  0 16px 40px rgba(0,0,0,0.15);
  --radius-sm:   4px;   /* Fluent uses smaller radii */
  --radius-md:   6px;
  --radius-lg:   8px;
  --radius-xl:   12px;
  --radius-pill: 999px;
  --font-heading: 'Segoe UI', system-ui, sans-serif;
  --font-body:    'Segoe UI', system-ui, sans-serif;
  --transition-base: 0.167s cubic-bezier(0.0,0.0,0.58,1.0); /* Fluent easing */
}

/* Acrylic panel */
.acrylic-panel {
  background: var(--bg-surface);
  backdrop-filter: var(--acrylic-blur);
  -webkit-backdrop-filter: var(--acrylic-blur);
  border: 1px solid var(--acrylic-border);
  border-radius: var(--radius-lg);
}

/* Reveal highlight on hover (Fluent signature) */
.fluent-btn {
  position: relative;
  overflow: hidden;
  background: var(--bg-subtle);
  border: 1px solid var(--border-light);
  border-radius: var(--radius-md);
  color: var(--text-primary);
  padding: 6px 16px;
  transition: background var(--transition-base);
}
.fluent-btn::before {
  content: '';
  position: absolute;
  width: 80px; height: 80px;
  background: radial-gradient(circle, rgba(255,255,255,0.5) 0%, transparent 70%);
  transform: translate(-50%, -50%);
  pointer-events: none;
  opacity: 0;
  transition: opacity 0.2s;
}
.fluent-btn:hover::before { opacity: 1; }
/* Move ::before with JS: el.addEventListener('mousemove', e => el.style.setProperty('--x', e.offsetX+'px') */
```

---

## 4. Universal Component Patterns (all themes use these, themed via CSS vars)

### 4.1 Accordion / Collapsible Panels — THE DEFAULT FOR FILTER & SETTINGS PANELS

> **Rule**: ALL filter panels, settings panels, rule editors, and condition groups MUST use
> accordion by default. Never show all options expanded simultaneously.

```jsx
// AccordionSection.jsx — reusable for filters/settings/rules
import { useState } from 'react';
import { ChevronDown } from 'lucide-react';

const AccordionSection = ({ title, icon, badge, defaultOpen = false, children }) => {
  const [open, setOpen] = useState(defaultOpen);
  return (
    <div className="accordion-section" style={{ borderRadius: 'var(--radius-md)', overflow: 'hidden',
      border: '1px solid var(--border-light)', marginBottom: 8 }}>
      <button
        onClick={() => setOpen(o => !o)}
        style={{
          width: '100%', display: 'flex', alignItems: 'center', gap: 8,
          padding: '10px 14px', background: open ? 'var(--accent-light)' : 'var(--bg-subtle)',
          border: 'none', cursor: 'pointer', color: open ? 'var(--accent-primary)' : 'var(--text-primary)',
          fontSize: 13, fontWeight: 500, transition: 'background var(--transition-base)'
        }}
        aria-expanded={open}
      >
        {icon && <span style={{ opacity: 0.7 }}>{icon}</span>}
        <span style={{ flex: 1, textAlign: 'left' }}>{title}</span>
        {badge && (
          <span style={{ fontSize: 11, padding: '1px 7px', borderRadius: 'var(--radius-pill)',
            background: 'var(--accent-primary)', color: '#fff', fontWeight: 600 }}>{badge}</span>
        )}
        <ChevronDown size={14} style={{
          transform: open ? 'rotate(180deg)' : 'rotate(0deg)',
          transition: 'transform 0.2s ease', flexShrink: 0
        }} />
      </button>
      <div style={{
        maxHeight: open ? '600px' : '0',
        overflow: 'hidden',
        transition: 'max-height 0.3s cubic-bezier(0.4,0,0.2,1)',
      }}>
        <div style={{ padding: '10px 14px', background: 'var(--bg-surface)' }}>
          {children}
        </div>
      </div>
    </div>
  );
};
```

**Accordion best practices (from research):**
- Use chevron icon that rotates 180° on expand — never "+" or "-" alone
- Allow multiple sections open simultaneously (don't force single-open unless content is mutually exclusive)
- Headers must be descriptive — user should know contents before clicking
- Show a badge/count on header when section has active filters/rules (e.g. "Filters • 3")
- Smooth `max-height` transition (not `display:none` toggle which has no animation)
- First section can be `defaultOpen={true}` if it's the most important
- Keep panel content compact — use the `.ext-compact` modifier inside accordion bodies

### 4.2 Cards

```css
.card-smooth {
  background: var(--bg-surface);
  border: 1px solid var(--border-light);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-sm);
  padding: var(--space-lg, 20px);
  transition: box-shadow var(--transition-base), transform var(--transition-base);
}
.card-smooth:hover {
  box-shadow: var(--shadow-md);
  transform: translateY(-2px);
}
/* Compact card for extension popup */
.card-sm {
  padding: 10px 12px;
  border-radius: var(--radius-md);
  border: 1px solid var(--border-light);
  background: var(--bg-surface);
  box-shadow: var(--shadow-sm);
}
```

### 4.3 Buttons

```css
.btn-primary-smooth {
  background: var(--accent-primary);
  color: var(--text-on-accent);
  border: none;
  border-radius: var(--radius-md);
  padding: 8px 18px;
  font-size: 13px;
  font-weight: 500;
  transition: background var(--transition-base), transform 0.1s ease, box-shadow var(--transition-base);
}
.btn-primary-smooth:hover  { background: var(--accent-hover); transform: translateY(-1px); }
.btn-primary-smooth:active { transform: translateY(0); }

.btn-ghost-smooth {
  background: transparent;
  color: var(--accent-primary);
  border: 1.5px solid var(--border-medium);
  border-radius: var(--radius-md);
  padding: 7px 18px;
  font-size: 13px;
  transition: all var(--transition-base);
}
.btn-ghost-smooth:hover { background: var(--accent-light); border-color: var(--accent-primary); }

/* Icon-only button */
.btn-icon {
  width: 32px; height: 32px;
  display: inline-flex; align-items: center; justify-content: center;
  border-radius: var(--radius-md);
  background: var(--bg-subtle);
  border: 1px solid var(--border-light);
  cursor: pointer;
  transition: background var(--transition-base);
}
.btn-icon:hover { background: var(--bg-hover); }
```

### 4.4 Inputs & Forms

```css
.input-smooth {
  background: var(--bg-subtle);
  border: 1.5px solid var(--border-light);
  border-radius: var(--radius-md);
  padding: 7px 12px;
  color: var(--text-primary);
  font-size: 13px;
  transition: border-color var(--transition-base), box-shadow var(--transition-base);
  width: 100%;
}
.input-smooth:focus {
  outline: none;
  border-color: var(--accent-primary);
  box-shadow: 0 0 0 3px var(--accent-light);
  background: var(--bg-surface);
}
```

**Form layout rules:**
- Label font-size: 12px, font-weight: 500, margin-bottom: 4px
- Input height: 34px (compact), 40px (normal)
- Group related fields in accordion sections (see 4.1)
- Never place more than 4 form fields visible at once in a Chrome extension popup

### 4.5 Badges / Tags

```css
.badge-sm {
  padding: 2px 8px;
  border-radius: var(--radius-pill);
  font-size: 11px;
  font-weight: 500;
  line-height: 1.4;
}
.badge-accent  { background: var(--accent-light);   color: var(--accent-primary); }
.badge-success { background: #ECFDF5; color: #059669; }
.badge-warning { background: #FFFBEB; color: #D97706; }
.badge-danger  { background: #FEF2F2; color: #DC2626; }
.badge-muted   { background: var(--bg-subtle); color: var(--text-muted); }
```

### 4.6 Navigation / Sidebar

- Sidebar width: 220px (collapsed: 56px) — slightly narrower than old default
- Active nav: `background: var(--accent-light)`, `color: var(--accent-primary)`, `border-left: 3px solid var(--accent-primary)`
- Nav item height: 36px (compact), 40px (normal)
- Nav font-size: 13px

---

## 5. Motion & Animation System

```css
/* Global easing (set per theme in :root or override here) */
*, *::before, *::after {
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
}

/* Entry animation */
@keyframes fadeSlideUp {
  from { opacity: 0; transform: translateY(10px); }
  to   { opacity: 1; transform: translateY(0); }
}
.animate-in { animation: fadeSlideUp 0.28s ease forwards; }

/* Stagger children */
/* .animate-in:nth-child(1) { animation-delay: 0.04s; }
   .animate-in:nth-child(2) { animation-delay: 0.08s; } ... */

/* Skeleton loading */
@keyframes shimmer {
  from { background-position: -200% 0; }
  to   { background-position: 200% 0; }
}
.skeleton {
  background: linear-gradient(90deg, var(--bg-subtle) 25%, var(--bg-hover) 50%, var(--bg-subtle) 75%);
  background-size: 200% 100%;
  animation: shimmer 1.5s infinite;
  border-radius: var(--radius-sm);
}
```

**Rules:**
- Max hover lift: `translateY(-3px)` — never more
- Accordion open/close: `max-height` transition, 0.25–0.35s
- Never `display: none` for animated show/hide — use `opacity` + `max-height` or `visibility`
- Spinner: Bootstrap `spinner-border` sized to `1rem`, colored `var(--accent-primary)`

---

## 6. Chrome Extension Architecture

```
extension/
├── manifest.json        ← Manifest V3
├── popup.html           ← Bootstrap 5 CDN + React mount
├── popup.jsx            ← Root component (apply ext-compact class here)
├── popup.css            ← Theme variables + extension overrides
├── background.js        ← Service worker (if needed)
└── content.js           ← Content script (if needed)
```

**manifest.json (Manifest V3):**
```json
{
  "manifest_version": 3,
  "name": "Extension Name",
  "version": "1.0",
  "action": { "default_popup": "popup.html", "default_icon": "icon.png" },
  "permissions": [],
  "content_security_policy": {
    "extension_pages": "script-src 'self'; object-src 'self'"
  }
}
```

**Extension-specific rules:**
- Always wrap root div with `ext-compact` class
- Use `chrome.storage.local` — never `localStorage`
- Settings panels → AccordionSection with `defaultOpen={false}`
- Filter panels → AccordionSection, show badge with active count
- Rules/conditions editor → nested AccordionSections (max 2 levels deep)
- Show loading skeletons for any async fetch
- Keep each accordion section body ≤ 250px tall before scrolling

---

## 7. Accessibility Checklist (all themes)

- [ ] `aria-expanded` on all accordion toggle buttons
- [ ] `aria-label` on icon-only buttons
- [ ] `for` + `id` pairing on all form labels
- [ ] Focus ring visible: `outline: 2px solid var(--accent-primary); outline-offset: 2px`
- [ ] Color contrast WCAG AA (4.5:1 text, 3:1 UI)
- [ ] Keyboard: Tab to focus, Enter/Space to toggle accordions
- [ ] `alt` text on all images
- [ ] Reduced motion: `@media (prefers-reduced-motion: reduce) { * { animation: none !important; transition: duration 0.01ms !important; } }`

---

## 8. Bootstrap-Only Theme Feasibility

**Can Bootstrap CSS alone achieve these themes? Here's the honest answer:**

| Theme              | Bootstrap CSS Only? | What you need beyond Bootstrap |
|--------------------|--------------------|---------------------------------|
| A. Classic Light   | ✅ Yes, fully       | Just override CSS variables in `:root` |
| B. Google Material | ✅ Mostly           | Override vars + add ripple CSS (40 lines) |
| C. iOS Liquid Glass| ⚠️ Partially        | `backdrop-filter` is pure CSS (not Bootstrap) — add 30 lines |
| D. Dark Pro        | ✅ Yes, fully       | Bootstrap's `data-bs-theme="dark"` + var overrides |
| E. Windows Fluent  | ⚠️ Partially        | Acrylic = same as iOS; reveal hover = 20 lines extra CSS |

**Key insight**: Bootstrap provides the layout, grid, and component structure for ALL themes.
The theme personality comes from CSS custom property overrides in `:root` — no extra libraries needed.
Only Liquid Glass and Fluent Acrylic need ~30 lines of CSS beyond Bootstrap for `backdrop-filter`.

Use Bootstrap's built-in dark mode: add `data-bs-theme="dark"` to `<html>` for Theme D.
Override `--bs-*` variables to align Bootstrap's internals with your theme vars.

```css
/* Bridge Bootstrap vars to your theme */
:root {
  --bs-primary:        var(--accent-primary);
  --bs-border-radius:  var(--radius-md);
  --bs-border-color:   var(--border-light);
  --bs-body-bg:        var(--bg-primary);
  --bs-body-color:     var(--text-primary);
  --bs-card-bg:        var(--bg-surface);
}
```

---

## 9. Anti-Patterns — NEVER DO THESE

| ❌ Don't                                               | ✅ Do instead                                         |
|-------------------------------------------------------|------------------------------------------------------|
| Show all filter/settings options expanded by default  | Use AccordionSection, collapsed by default           |
| Use Bootstrap default font sizes in extension popups  | Apply `.ext-compact` to reduce padding + font sizes  |
| Fixed pixel sizes without the scale                   | Use the 11/13/15/17/20/24px scale                    |
| `display:none` for show/hide animation                | Use `max-height` + `opacity` transition              |
| Multiple cards crammed with no breathing room         | Use `gap-3`, `mb-3`, and compact card padding        |
| Accordion with + / - icons only                       | Use rotating chevron icon (ChevronDown from lucide)  |
| Raw hex colors inline                                 | Always use CSS variables                             |
| `box-shadow: none` on any card                        | Always `var(--shadow-sm)` minimum                    |
| `transition: none`                                    | All interactive elements have transitions            |
| Class components                                      | Functional + hooks only                              |
| `localStorage` in extensions                          | `chrome.storage.local`                               |
| Random `border-radius` values                         | Theme radius scale only                              |

---

## 10. Quick Bootstrap Class Reference

```
Layout:   container, container-fluid, row, col-*, g-3, g-4
Flex:     d-flex, align-items-center, justify-content-between, gap-2, gap-3
Spacing:  p-2, p-3, py-3, px-3, mb-2, mb-3, mt-auto
Text:     fw-semibold, fw-medium, text-muted, small, fs-6
Display:  d-none d-md-block (responsive)
Utils:    rounded-3, shadow-sm, border, border-light, overflow-hidden
Accordion: accordion, accordion-item, accordion-header, accordion-button, accordion-collapse
Dark mode: data-bs-theme="dark" on <html>
```
