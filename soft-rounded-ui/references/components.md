# Soft & Rounded — Component Recipes

Copy-paste CSS for every component in the system. All snippets assume the design tokens from SKILL.md are defined on `:root`, including the dark-mode overrides.

Rule 0 in SKILL.md (flat, no boxes inside boxes) applies to every recipe: a card or glass tile never holds a button, input, or other box, and controls sit directly on the page or the gradient band.

## Contents

1. [Base / reset](#base--reset)
2. [Gradient header, glass tiles, glass button](#gradient-header-glass-tiles-glass-button)
3. [Card](#card)
4. [Pill navigation & tabs](#pill-navigation--tabs)
5. [Bottom tab bar](#bottom-tab-bar)
6. [Buttons](#buttons)
7. [Inputs & forms](#inputs--forms)
8. [Stat card (incl. feature variant)](#stat-card)
9. [Status pills & chips](#status-pills--chips)
10. [Avatars & icon bubbles](#avatars--icon-bubbles)
11. [Table](#table)
12. [Bar chart (CSS-only)](#bar-chart-css-only)
13. [Donut chart (CSS-only)](#donut-chart-css-only)
14. [Progress bar](#progress-bar)
15. [Activity feed](#activity-feed)
16. [Segmented control](#segmented-control)
17. [Feature/upgrade panel](#featureupgrade-panel)
18. [Layout scaffold & responsive](#layout-scaffold--responsive)

---

## Base / reset

```css
*{margin:0;padding:0;box-sizing:border-box}
body{font-family:var(--font);background:var(--bg);color:var(--g900);font-size:14px;line-height:1.45}
a{text-decoration:none;color:inherit}
button{font-family:inherit;border:none;cursor:pointer;background:none}
```

## Gradient header, glass tiles, glass button

The band at the top of every screen. Content below overlaps it by about 26px. Decorative circles stay behind the text.

```css
.band{position:relative;overflow:hidden;color:#fff;
      background:linear-gradient(135deg,var(--header-start),var(--header-end));
      border-radius:0 0 28px 28px;padding:22px 22px 48px;margin-bottom:-26px}
.band .blob{position:absolute;border-radius:50%;background:#fff;pointer-events:none}
.band .b1{width:170px;height:170px;right:-30px;top:-60px;opacity:.10}
.band .b2{width:110px;height:110px;left:-40px;bottom:-20px;opacity:.07}
.band-row{position:relative;display:flex;align-items:center;gap:14px}
.band-mark{width:44px;height:44px;border-radius:var(--r-ctrl);flex-shrink:0;
           background:rgba(255,255,255,.22);display:flex;align-items:center;justify-content:center;
           font-weight:800}
.band h1{font-size:24px;font-weight:750;letter-spacing:-.6px}
.band .sub{font-size:12.5px;color:rgba(255,255,255,.82);margin-top:2px}
.glass-row{position:relative;display:flex;gap:10px;margin-top:16px}
.glass{flex:1;min-width:0;border-radius:var(--r-glass);padding:10px 12px;
       background:rgba(255,255,255,.16);border:1px solid rgba(255,255,255,.22)}
.glass b{display:block;font-size:17px;font-weight:700;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.glass span{display:block;font-size:11px;color:rgba(255,255,255,.85);margin-top:2px}
.glass .dot{width:8px;height:8px;border-radius:50%;display:inline-block;margin-right:6px;vertical-align:1px}
.glass .dot.ok{background:#4ADE80}
.glass .dot.warn{background:#FBBF24}
.glass .dot.err{background:#FB7185}
.glass-btn{border-radius:var(--r-pill);padding:8px 14px;font-size:13px;font-weight:700;color:#fff;
           background:rgba(255,255,255,.20);border:1px solid rgba(255,255,255,.30)}
```

```html
<header class="band">
  <div class="blob b1"></div><div class="blob b2"></div>
  <div class="band-row">
    <div class="band-mark">A</div>
    <div style="flex:1"><h1>Overview</h1><p class="sub">Ready to print</p></div>
    <button class="glass-btn">Export</button>
  </div>
  <div class="glass-row">
    <div class="glass"><b><i class="dot ok"></i>Ready</b><span>Printer</span></div>
    <div class="glass"><b>80%</b><span>Black ink</span></div>
    <div class="glass"><b>64%</b><span>Color ink</span></div>
  </div>
</header>
```

On a phone, also paint the status-bar inset with the same header gradient so light status icons never sit on scrolled content.

## Card

No borders. Elevation only, and no shadow in dark mode (the token is `none` there).

```css
.card{background:var(--white);border-radius:var(--r-card);padding:22px;
      box-shadow:var(--sh-card);transition:.15s}
.card:hover{box-shadow:var(--sh-hover)}
.card h3{font-size:15px;font-weight:700}
.card-title{display:flex;align-items:center;gap:10px;margin-bottom:14px}
```

## Pill navigation & tabs

Horizontal nav for dashboards. The active tab is the brand gradient, not flat indigo. No container behind the tabs.

```css
.tabs{display:flex;gap:4px}
.tabs button{padding:9px 20px;border-radius:var(--r-pill);font-size:13px;
             color:var(--g500);font-weight:500;transition:.15s}
.tabs button:hover{color:var(--g900)}
.tabs button.on{background:linear-gradient(90deg,var(--grad-start),var(--grad-end));
                color:#fff;font-weight:600}
```

```html
<nav class="tabs">
  <button class="on">Overview</button>
  <button>Analytics</button>
  <button>Settings</button>
</nav>
```

```js
tabs.addEventListener('click', e => {
  if (e.target.tagName !== 'BUTTON') return;
  tabs.querySelectorAll('button').forEach(b => b.classList.remove('on'));
  e.target.classList.add('on');
});
```

## Bottom tab bar

For phone apps. One tab per main section. The gradient pill on the icon is the selected state — do not add a second focus ring. The bar is a flat full-width strip, not a floating rounded box.

```css
.tabbar{display:flex;gap:4px;background:var(--white);border-top:1px solid var(--g100);
        padding:6px 18px 12px}
.tabbar button{flex:1;display:flex;flex-direction:column;align-items:center;gap:3px;
               padding:6px 0;border-radius:18px;color:var(--g400);font-size:11px;font-weight:700}
.tabbar button .ico{width:44px;height:28px;border-radius:var(--r-pill);
                    display:flex;align-items:center;justify-content:center}
.tabbar button.on{color:var(--violet)}
.tabbar button.on .ico{background:linear-gradient(90deg,var(--grad-start),var(--grad-end));color:#fff}
```

```html
<nav class="tabbar">
  <button class="on"><span class="ico">▣</span>Print</button>
  <button><span class="ico">◉</span>Calibrate</button>
  <button><span class="ico">▤</span>Printer</button>
</nav>
```

## Buttons

One gradient primary per view. Everything else is tinted violet, never white-on-white.

```css
.btn{padding:11px 20px;border-radius:var(--r-pill);font-size:13px;font-weight:700;transition:.15s}
.btn.primary{background:linear-gradient(90deg,var(--grad-start),var(--grad-end));
             color:#fff;box-shadow:var(--sh-pop)}
.btn.primary:hover{filter:brightness(.96)}
.btn.primary:disabled{background:var(--g200);color:var(--g400);box-shadow:none}
.btn.ghost{background:var(--violet-soft);color:var(--violet-text)}
.btn.ghost:hover{filter:brightness(.97)}
.btn.ghost:disabled{background:var(--g100);color:var(--g300)}

.icon-btn{width:40px;height:40px;border-radius:var(--r-ctrl);background:var(--white);
          box-shadow:var(--sh-card);display:flex;align-items:center;justify-content:center;
          position:relative;transition:.15s}
.icon-btn:hover{box-shadow:var(--sh-hover)}
.icon-btn .dot{position:absolute;top:9px;right:10px;width:7px;height:7px;
               border-radius:50%;background:var(--violet);border:1.5px solid var(--white)}
```

## Inputs & forms

Inputs are the one exception to the no-border rule.

```css
input[type=text],input[type=email],select,textarea{
  border:1.5px solid var(--g200);border-radius:var(--r-ctrl);
  padding:10px 14px;font:inherit;font-size:12.5px;outline:none;transition:.15s;
  background:var(--white);color:var(--g900)}
input:focus,select:focus,textarea:focus{
  border-color:var(--indigo);
  box-shadow:0 0 0 3px var(--indigo-soft);
}

.search{display:flex;align-items:center;gap:8px;background:var(--white);
        border-radius:var(--r-pill);padding:10px 18px;box-shadow:var(--sh-card);
        color:var(--g400);font-size:13px}
.search input{border:none;box-shadow:none;padding:0;background:none;width:100%;color:var(--g900)}
```

Toggle switch. On = brand gradient.

```css
.switch{width:40px;height:24px;border-radius:var(--r-pill);background:var(--g200);
        position:relative;cursor:pointer;transition:.15s}
.switch.on{background:linear-gradient(90deg,var(--grad-start),var(--grad-end))}
.switch::after{content:"";position:absolute;top:3px;left:3px;width:18px;height:18px;
               border-radius:50%;background:#fff;transition:.15s;
               box-shadow:0 1px 3px rgba(0,0,0,.15)}
.switch.on::after{left:19px}
```

## Stat card

```css
.stat .k{font-size:12.5px;color:var(--g400);margin-bottom:8px;font-weight:500}
.stat .v{font-size:27px;font-weight:750;letter-spacing:-1px}
.stat .chip{display:inline-flex;align-items:center;gap:4px;margin-top:12px;
            font-size:11px;font-weight:700;padding:4px 12px;
            border-radius:var(--r-pill);background:var(--ok-bg);color:var(--ok-fg)}
.stat .chip.flat{background:var(--g100);color:var(--g500)}
.stat .chip.warn{background:var(--warn-bg);color:var(--warn-fg)}

/* Feature variant — max ONE per view, on the header gradient */
.stat.feature{background:linear-gradient(135deg,var(--header-start),var(--header-end));
              color:#fff;box-shadow:var(--sh-pop)}
.stat.feature .k{color:rgba(255,255,255,.7)}
.stat.feature .chip{background:rgba(255,255,255,.18);color:#fff}
```

```html
<div class="card stat feature">
  <div class="k">Total revenue</div>
  <div class="v">$48,290</div>
  <span class="chip">▲ +12.4% this month</span>
</div>
```

## Status pills & chips

Green = ready or success. Amber = needs attention. Rose = failed. Indigo = in progress. Gray = neutral.

```css
.pill{font-size:11px;font-weight:700;padding:4px 12px;border-radius:var(--r-pill)}
.pill.ok{background:var(--ok-bg);color:var(--ok-fg)}
.pill.warn{background:var(--warn-bg);color:var(--warn-fg)}
.pill.err{background:var(--err-bg);color:var(--err-fg)}
.pill.busy{background:var(--indigo-soft);color:var(--indigo)}
.pill.wait{background:var(--g100);color:var(--g500)}
```

## Avatars & icon bubbles

Squircles, never circles. Each card gets one accent. The bubble fill is a soft tint of that accent.

```css
.avatar{width:40px;height:40px;border-radius:var(--r-ctrl);
        background:linear-gradient(135deg,var(--indigo-100),var(--indigo-soft));
        display:flex;align-items:center;justify-content:center;
        font-weight:700;color:var(--indigo);font-size:13px}

.bubble{width:34px;height:34px;border-radius:12px;background:var(--indigo-soft);color:var(--indigo);
        display:flex;align-items:center;justify-content:center;font-size:11.5px;font-weight:700}
.bubble.violet{background:var(--violet-soft);color:var(--violet)}
.bubble.pink{background:var(--pink-soft);color:var(--pink)}
.bubble.green{background:var(--green-soft);color:var(--green)}
.bubble.amber{background:var(--amber-soft);color:var(--amber)}
.bubble.sky{background:var(--sky-soft);color:var(--sky)}
.bubble.alt{background:var(--g100);color:var(--g500)}

.team{display:flex}
.team .avatar{border:2.5px solid var(--white);margin-left:-8px}
.team .avatar:first-child{margin-left:0}
```

## Table

No cell borders. Hairline row dividers only.

```css
table{width:100%;border-collapse:collapse}
th{text-align:left;font-size:11px;text-transform:uppercase;letter-spacing:.6px;
   color:var(--g400);font-weight:600;padding:0 10px 12px}
td{padding:11px 10px;border-top:1px solid var(--g100);font-size:13px;color:var(--g500)}
td:first-child,th:first-child{padding-left:0}
td:last-child,th:last-child{padding-right:0;text-align:right}
.amt{font-weight:700;color:var(--g900)}
```

Table headers are the single sanctioned use of uppercase, and only at 11px / `--g400`.

## Bar chart (CSS-only)

Pill bars. One highlight uses the brand gradient. The rest are indigo tint.

```css
.bars{display:flex;align-items:flex-end;gap:10px;height:190px}
.bar{flex:1;display:flex;flex-direction:column;align-items:center;gap:8px;
     height:100%;justify-content:flex-end}
.bar i{width:100%;max-width:34px;border-radius:var(--r-pill);
       background:var(--indigo-soft);transition:.2s}
.bar:hover i{background:var(--indigo-100)}
.bar.hi i{background:linear-gradient(180deg,var(--grad-end),var(--grad-start))}
.bar span{font-size:10.5px;color:var(--g400);font-weight:500}
.bar.hi span{color:var(--violet);font-weight:700}
```

```html
<div class="bars">
  <div class="bar"><i style="height:52%"></i><span>Feb</span></div>
  <div class="bar hi"><i style="height:100%"></i><span>Mar</span></div>
</div>
```

For real charts, map series to `--grad-start`, `--indigo-100`, and at most one of `--sky` or `--violet`. Keep `borderRadius` maxed so bars stay pills.

## Donut chart (CSS-only)

Brand first, then one or two accents. Not a new hue per slice.

```css
.donut{width:130px;height:130px;border-radius:50%;position:relative;
       background:conic-gradient(var(--indigo) 0 52%, var(--violet) 52% 78%, var(--sky) 78% 100%)}
.donut::after{content:"";position:absolute;inset:22px;border-radius:50%;background:var(--white)}
.donut b{position:absolute;inset:0;display:flex;align-items:center;justify-content:center;
         font-size:19px;font-weight:750;z-index:1;letter-spacing:-.5px}
```

## Progress bar

```css
.track{height:10px;border-radius:var(--r-pill);background:var(--g100);overflow:hidden}
.fill{height:100%;border-radius:var(--r-pill);
      background:linear-gradient(90deg,var(--grad-start),var(--grad-end))}
```

## Activity feed

```css
.activity .item{display:flex;gap:12px;padding:11px 0}
.activity .item + .item{border-top:1px solid var(--g100)}
.activity .t{font-size:12.5px;color:var(--g500)}
.activity .t b{color:var(--g900);font-weight:600}
.activity .time{font-size:11px;color:var(--g400);margin-top:2px}
```

Use `.bubble` and its accent classes for the icons. Alternate accents down the list.

## Segmented control

The selected segment is the brand gradient with white text.

```css
.seg{display:flex;gap:2px;background:var(--g100);border-radius:var(--r-pill);padding:4px}
.seg button{flex:1;padding:8px 14px;border-radius:var(--r-pill);font-size:13px;
            color:var(--g500);font-weight:600}
.seg button.on{background:linear-gradient(90deg,var(--grad-start),var(--grad-end));color:#fff}
```

## Feature/upgrade panel

```css
.upgrade{background:linear-gradient(135deg,var(--header-start),var(--header-end));
         color:#fff;box-shadow:var(--sh-pop)}
.upgrade h3{color:#fff}
.upgrade p{font-size:12.5px;color:rgba(255,255,255,.78)}
.upgrade button{background:#fff;color:var(--header-start);border-radius:var(--r-pill);
                padding:10px 18px;font-weight:700;font-size:12.5px}
```

## Layout scaffold & responsive

Dashboards use a centered column and a top pill nav. Phone apps use the gradient header plus `.tabbar`, and pad the bottom for the system gesture area.

```css
.app{max-width:1280px;margin:0 auto;padding:24px 28px 48px}
.stats{display:grid;grid-template-columns:repeat(4,1fr);gap:16px}
.grid{display:grid;grid-template-columns:1.9fr 1fr;gap:16px}

@media(max-width:1000px){
  .stats{grid-template-columns:repeat(2,1fr)}
  .grid{grid-template-columns:1fr}
  .search{display:none}
}
@media(max-width:600px){
  .app{padding:16px}
  .stats,.glass-row{grid-template-columns:1fr 1fr}
  .glass-row{display:grid}
  .tabs{overflow-x:auto}
}
```

## Tailwind mapping (if the project uses Tailwind)

| Token | Tailwind |
|---|---|
| `--grad-start` → `--grad-end` | `bg-gradient-to-r from-[#6366F1] to-[#A855F7]` |
| `--header-start` → `--header-end` | `bg-gradient-to-br from-[#4F46E5] to-[#9333EA]` |
| `--indigo` | `text-[#4F46E5]` (dark: `#A5B4FC`) |
| `--violet-soft` / `--violet-text` | `bg-[#F3E8FF] text-[#7E22CE]` |
| `--bg` | `bg-[#F5F5FB]` (dark: `bg-[#0F0E1A]`) |
| `--white` (card) | `bg-white` (dark: `bg-[#1B1A2E]`) |
| `--ok-bg` / `--ok-fg` | `bg-[#DCFCE7] text-[#15803D]` |
| `--warn-bg` / `--warn-fg` | `bg-[#FEF3C7] text-[#B45309]` |
| `--err-bg` / `--err-fg` | `bg-[#FFE4E6] text-[#BE123C]` |
| `--r-card` | `rounded-[20px]` |
| `--r-ctrl` | `rounded-[14px]` |
| `--r-pill` | `rounded-full` |
| `--sh-card` | `shadow-[0_8px_24px_rgba(79,70,229,.08)]` |
| `--sh-pop` | `shadow-[0_12px_32px_rgba(124,58,237,.28)]` |

Gate the dark values with `dark:` and set `darkMode: 'media'` so the theme follows the system.
