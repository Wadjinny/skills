---
name: soft-rounded-ui
description: >-
  Ilyas's "Soft & Rounded" design system — indigo-to-violet gradients, glass
  tiles on a gradient header, tinted buttons, colored icon bubbles, status
  colors, and automatic dark mode. Flat layout with no boxes inside boxes,
  pill navigation, and soft shadows. Use this skill whenever building, styling, or restyling ANY user
  interface for Ilyas's projects, including dashboards, landing pages, admin
  panels, forms, mockups, prototypes, phone apps, and HTML/React components,
  or when the user mentions "my design", "my style", "soft rounded", "house
  style", or asks for something "sleek and minimal". Apply it even if the user
  doesn't name the style explicitly — this is the default look for all UI work.
---

# Soft & Rounded UI

A design system for sleek, rounded interfaces. The signature is an indigo-to-violet gradient header, glass tiles for the numbers that matter, tinted (never white-on-white) controls, one colored icon per card, and meaning colors for status. The layout is flat: one layer of surfaces on the page, never a box inside a box. Dark mode follows the system automatically.

## The one-paragraph mental model

Every screen opens with a gradient band (indigo `#4F46E5` to violet `#9333EA`). The title sits on that band in white, and the few facts the user checks first (status, a level, a count) sit under it as translucent glass tiles. Below the band, content sits on a pale lilac-gray canvas. Sections are separated by headings, spacing, and hairlines; a white card is only for read-only content (stats, lists, charts), never for wrapping controls. The main action is a gradient pill. Secondary actions are a light violet fill with violet text. Each card gets one colored icon bubble. Status uses green, amber, and rose. Everything interactive is a pill or a 14px squircle. If a decision is unclear, choose the softer, rounder option, and put color where it carries meaning.

## Design tokens

Always define these as CSS variables (or the equivalent theme object) and derive every style from them. Never hard-code one-off colors, radii, or shadows. Provide both light and dark values. In CSS, put the dark values under `@media (prefers-color-scheme: dark)`. In an app, follow the system setting.

```css
:root{
  /* Brand gradient — header band, selected options, primary button, progress */
  --grad-start:#6366F1; --grad-end:#A855F7;
  --header-start:#4F46E5; --header-end:#9333EA;
  --indigo:#4F46E5; --indigo-hover:#4338CA;
  --indigo-soft:#EEF2FF; --indigo-100:#E0E7FF;
  --violet:#7C3AED; --violet-soft:#F3E8FF; --violet-text:#7E22CE;

  /* Surfaces */
  --white:#FFFFFF;          /* card surface */
  --bg:#F5F5FB;             /* page background */
  --g100:#EEF0FB; --g200:#E5E7EB; --g300:#D1D5DB;
  --g400:#9CA3AF; --g500:#6B7280; --g700:#374151; --g900:#1E1B4B;

  /* Status — meaning colors, used only for status */
  --ok-bg:#DCFCE7;   --ok-fg:#15803D;
  --warn-bg:#FEF3C7; --warn-fg:#B45309;
  --err-bg:#FFE4E6;  --err-fg:#BE123C;

  /* Icon accents — one per card, not a rainbow on one chart */
  --pink:#DB2777;  --pink-soft:#FCE7F3;
  --green:#16A34A; --green-soft:#DCFCE7;
  --amber:#D97706; --amber-soft:#FEF3C7;
  --sky:#0284C7;   --sky-soft:#E0F2FE;

  /* Radii */
  --r-card:20px;   /* cards, panels, modals */
  --r-ctrl:14px;   /* buttons, inputs, avatars, icon bubbles */
  --r-pill:999px;  /* tabs, chips, badges, progress, chart bars */
  --r-glass:16px;  /* glass tiles on the header */

  /* Shadows — indigo-tinted, never harsh. Drop them in dark mode. */
  --sh-card:0 8px 24px rgba(79,70,229,.08);
  --sh-hover:0 12px 28px rgba(79,70,229,.14);
  --sh-pop:0 12px 32px rgba(124,58,237,.28);

  --font:'Segoe UI',system-ui,-apple-system,'Inter',sans-serif;
}

@media (prefers-color-scheme: dark){
  :root{
    --grad-start:#818CF8; --grad-end:#C084FC;
    --header-start:#3730A3; --header-end:#7E22CE;
    --indigo:#A5B4FC; --indigo-hover:#C7D2FE;
    --indigo-soft:#2A2650; --indigo-100:#3B3568;
    --violet:#C4B5FD; --violet-soft:#2E2650; --violet-text:#D8B4FE;
    --white:#1B1A2E; --bg:#0F0E1A;
    --g100:#26243D; --g200:#34324F; --g300:#4B4868;
    --g400:#8C89AA; --g500:#A5A3C4; --g700:#D6D4EE; --g900:#EDEBFF;
    --ok-bg:#14352A; --ok-fg:#4ADE80;
    --warn-bg:#3A2E12; --warn-fg:#FBBF24;
    --err-bg:#3D1A22; --err-fg:#FB7185;
    --pink:#F472B6; --pink-soft:#3D1A32;
    --green:#4ADE80; --green-soft:#14352A;
    --amber:#FBBF24; --amber-soft:#3A2E12;
    --sky:#38BDF8; --sky-soft:#123044;
    --sh-card:none; --sh-hover:none; --sh-pop:none;
  }
}
```

Glass tiles and glass buttons sit on the gradient. They stay `rgba(255,255,255,.16)` with a `rgba(255,255,255,.22)` hairline in both themes. Do not recolor them with `--white`.

## Non-negotiable rules

0. **Flat: no boxes inside boxes.** Count the visible surfaces from the page inward (page or gradient band → box → box). Never go past one box. A box is anything with its own fill, border, or shadow: card, glass tile, tinted button, input, tab track, panel. So:
   - Controls (buttons, inputs, switches, selects) sit directly on the page or the gradient band, never inside a card, glass tile, or other container.
   - Group controls with a section heading, spacing (~24px between sections), and a 1px `--g100` hairline, not with a card.
   - Tabs and segmented choices have no track behind them. Mark the active one with its own fill or an underline bar.
   - A card or glass tile may hold text, numbers, icons, and small inline marks (icon bubble, status pill, dot), but no controls or other boxes.
   - When restyling an existing screen, remove wrapper boxes first; don't add a lighter box to fix contrast.
1. **No borders on cards.** Separation comes from the card-on-canvas contrast plus `--sh-card`. Inside a card, use a 1px `--g100` hairline to divide rows. In dark mode the lighter card is enough; skip the shadow.
2. **The gradient is the brand.** Header, selected pills, the primary button, progress fills, and the one feature card use `linear-gradient(90deg, var(--grad-start), var(--grad-end))` or the header pair for large bands. Flat indigo is for text, icons, and focus rings.
3. **Secondary controls are tinted.** A secondary button is `--violet-soft` fill with `--violet-text`. A white button on a white surface is a bug.
4. **One main action per view.** That action is the gradient pill with `--sh-pop`. Other buttons stay tinted.
5. **Status has meaning colors.** Ready / success = green. Needs attention = amber. Failed / blocked = rose. In progress = indigo tint. Neutral = gray. Do not use these colors as decoration.
6. **One accent per card icon.** Cycle pink, green, amber, sky, indigo, and violet so neighboring cards are easy to tell apart. Charts stay on the brand gradient plus one or two of those accents, not every hue at once.
7. **Nothing square.** Minimum radius is `--r-ctrl` (14px). Navigation, chips, badges, and bar-chart bars are full pills.
8. **Dark mode is part of the system.** Ship both token sets. Follow the system setting. Do not invent a third palette.

## Typography

Numbers are the heroes; labels whisper. On the gradient band, titles and tile values are white.

| Role | Size | Weight | Color | Notes |
|---|---|---|---|---|
| Page title | 24px | 750 | white on the band, else `--g900` | `letter-spacing:-0.6px` |
| Glass tile value | 17px | 700 | white | one line |
| Big stat value | 27px | 750 | `--g900` | `letter-spacing:-1px` |
| Card heading | 15px | 700 | `--g900` | sits beside the icon bubble |
| Body / cell | 13–14px | 400–500 | `--g500` | key values get 600–700 + `--g900` |
| Label / caption | 11–12.5px | 500 | `--g400`, or white at 85% on glass | sentence case, not uppercase |
| Chip / badge | 11px | 700 | the status foreground | |

Sentence case everywhere. Table headers are the one sanctioned uppercase, and only at 11px in `--g400`.

## Core component recipes

Full copy-paste CSS lives in `references/components.md`. Read it before writing markup. The essentials:

- **Gradient header**: full-width band, `linear-gradient(135deg, var(--header-start), var(--header-end))`, white title, two soft white circles as decoration (about 10% and 7% white, clipped so they don't cover text). Optional glass tiles in a row under the title. Controls on the band (a device picker, a connect button) sit directly on the gradient, not inside a glass tile. Page content starts below the band; don't overlap it with cards.
- **Glass tile**: 16px radius, `rgba(255,255,255,.16)` fill, 1px `rgba(255,255,255,.22)` border, white value, caption at 85% white. An optional 8px status dot. Read-only: never put a button or input in it.
- **Glass button**: pill, `rgba(255,255,255,.20)` fill, 1px `rgba(255,255,255,.30)` border, white bold text. Only for actions that sit on the band. A key action on the band can be a solid white pill with indigo text instead.
- **Card**: `--white` bg, `--r-card`, ~20–22px padding, `--sh-card`. Hover lifts to `--sh-hover` in light mode. Read-only content only (rule 0). Settings and forms are flat sections, not cards.
- **Card title**: a 34px squircle bubble at 14% of the accent (22% in dark mode) with the accent icon, then the heading.
- **Top pill nav** (dashboards): tabs sit directly on the page with no container. Active tab is a brand-gradient pill with white text; inactive tabs are plain `--g500` text.
- **Bottom tab bar** (phone apps): a full-width `--white` strip with a `--g100` hairline on top (no floating rounded bar), one tab per section. The selected tab's icon sits in a small gradient pill; its label is `--violet`. Unselected icons and labels are `--g400`. No extra focus rectangle.
- **Tabs on the gradient band**: plain white labels, inactive at 60% white, active marked by a short 3px white underline bar. No glass track behind them.
- **Primary button**: gradient pill, white text, `--sh-pop`. Disabled = `--g200` fill and `--g400` text, no shadow.
- **Secondary button**: `--violet-soft` pill, `--violet-text`. Disabled = `--g100` / `--g300`.
- **Input**: 1.5px `--g200` border, `--r-ctrl`. Focus = indigo border plus a 3px `--indigo-soft` ring.
- **Status pill**: 11px / weight 700 / `4px 12px`, the matching `--*-bg` and `--*-fg`.
- **Progress**: `--g100` pill track, brand-gradient fill.
- **Bar chart**: pill bars. Default `--indigo-soft`. One highlight bar uses the brand gradient.

## Layout

- **Phone apps.** Gradient header (with the status bar covered by the same gradient, so scrolled content never shows through light status icons), flat sections below it, a compact bottom tab bar for the app's main sections. Keep the bar clear of the system navigation inset.
- **Dashboards.** Centered container, max-width 1240–1280px, on `--bg`. A top nav row directly on the page (logo squircle, tabs, search pill, icon button, avatar), then the gradient header with glass tiles, then the card grid of read-only content. 16px gaps. Asymmetric columns (`1.9fr 1fr`) rather than equal halves. Collapse to one column below ~1000px, and hide the search pill on small screens.
- Generous card padding (~20–22px). Let stats breathe.

## Working reference

`assets/dashboard-template.html` is a complete dashboard in this style, including the gradient header, glass tiles, tinted buttons, status colors, and dark mode. When building something new, skim it first and reuse its patterns. It is the ground truth for tokens and component look. Where it nests boxes (a nav pill container, controls inside cards), rule 0 wins. For a phone app, keep the same tokens and swap the top pill nav for the bottom tab bar.

## Common mistakes to avoid

- Boxes inside boxes: a button inside a glass tile inside the header, a settings card holding inputs and buttons, a tab track holding a selected pill. Flatten to one layer.
- White secondary buttons on white cards.
- A flat indigo primary button. The brand action is the gradient.
- Using green, amber, or rose as decoration. Those are status colors.
- Painting every chart series a different hue. Charts stay on the gradient plus one or two accents.
- Bordered cards, or a hairline `0 1px 2px` shadow. Use the token shadows, and none in dark mode.
- Circular avatars. Squircles (`--r-ctrl`) are part of the signature.
- Uppercase micro-labels with letter-spacing.
- A second focus box around a tab that is already a gradient pill.
- A dark theme that is just the light theme with inverted gray. Use the dark token set above.
