# DESIGN.md — GenLayer Design System

> **For AI agents and developers.** This file is the single entry point to the GenLayer
> visual system. Drop it (or a link to it) into any project. It tells you what every brand
> token is, how to wire it up, and where to download the real files — fonts, CSS, logos,
> icons, and ready-made UI kits — **only when you actually need them.**

GenLayer is the **adjudication layer for the agentic economy** — decentralized
AI-validator consensus that resolves contracts requiring judgment, not just code.

---

## 0. How to use this file

**If you are an AI agent:** read sections 1–9 below. They contain every token value,
voice rule, and layout pattern inline — you can produce on-brand work *without fetching
anything*. Fetch real files (section 1) only when you need to actually render them:
fonts, logo SVGs, icons, or a UI kit to fork.

**If you are a developer:** clone the repo, then `<link>` the CSS (section 2). Done.

### Repo base URL

Every download link below is built from one base. **If your repo path differs, find-and-replace
this one line:**

```
RAW_BASE = https://raw.githubusercontent.com/genlayer-foundation/genlayer-design/main
```

> Repository: <https://github.com/genlayer-foundation/genlayer-design>.
> Everything else in this file uses `{RAW_BASE}/...`. If you fork to another path,
> change this one line.

---

## 1. File manifest — download on demand

Pull only what the task needs. Paths are relative to repo root; prepend `{RAW_BASE}/` for a
direct download.

### Always (the foundation)
| File | Why |
| --- | --- |
| `colors_and_type.css` | Every token (color, type, spacing, radius, shadow) + base element styles + `@font-face`. **Link this first.** |
| `components.css` | Reusable, theme-aware component classes (`gl-btn`, `gl-card`, `gl-table`, fields, badges, …). Link **after** the tokens. See §10. |
| `theme.css` | Shared page chrome (nav, hero, sections, footer) + dark/light theme tokens + the theme-toggle button. Link **after** components. See §11. |
| `theme.js` | Persists the dark/light choice (localStorage `gl-theme`, default dark) and wires `[data-theme-toggle]` buttons. Load before `</body>`. |
| Space Grotesk (Google Fonts) | Display/title font — loaded via `@import` inside `colors_and_type.css`. No file to bundle. |
| `fonts/Switzer-Variable.ttf` | Body font (variable 100–900). |
| `fonts/Switzer-VariableItalic.ttf` | Body italic. |

### Logos & mark (use the SVG — never recreate in CSS text)
| File | Use |
| --- | --- |
| `assets/GenLayer_Logo_Black.svg` | Wordmark on light surfaces |
| `assets/GenLayer_Logo_White.svg` | Wordmark on dark surfaces |
| `assets/GenLayer_Logo_Black_Large.svg` | Hero-scale wordmark |
| `assets/GenLayer_Mark_Black.svg` | Triangle/hex mark, light bg |
| `assets/GenLayer_Mark_White.svg` | Triangle/hex mark, dark bg |
| `assets/gl-symbol-small.svg` | Favicon-scale mark |

### Icons (custom, thin-stroke, geometric)
`assets/arrow-right.svg` · `assets/checkbox-circle.svg` · `assets/points-gradient-icon.svg`
Track icons: `assets/track-consensus.svg` · `track-prediction.svg` · `track-governance.svg`
· `track-work.svg` · `track-justice.svg` · `track-gaming.svg`
Category badges: `assets/badge-builder.svg` · `badge-validator.svg` · `badge-community.svg`

> If an icon you need isn't here, fall back to **Lucide** (thin, geometric — closest match)
> and flag the substitution. **Never use emoji.**

### Textures & photography (campaign / hero use)
`assets/hex-blur-builder.png` · `hex-blur-validator.png` · `hex-blur-community.png`
(soft hex glow behind category badges) ·
`assets/hero-background.jpg` · `assets/cta-background.jpg` · `assets/prize-background.jpg`

### UI kits — fork these, don't build from scratch
| File | What it is |
| --- | --- |
| `ui_kits/portal/Dashboard.html` | Full Portal app shell: sidebar, topbar, hero, stat cards, leaderboard, contributions table. Light surface. |
| `ui_kits/marketing/HackathonPage.html` | Full dark marketing page: nav, gradient hero, tracks grid, earn-forever banner, footer. |

Both reference `../../colors_and_type.css`. Keep that relative path intact, or re-point it
to `{RAW_BASE}/colors_and_type.css`.

### Pages — one consistent, cross-linked set
Every page shares the same nav (same order: **Components · Dashboard · Slides · Assets · About**),
the same chrome from `theme.css`, and a dark/light toggle. Open any one to browse them all.

| Order | File | What it is |
| --- | --- | --- |
| 1 | `components.html` | Every component + charts **and** the composed Portal patterns (hexagon role icons, profile header, stat cards, role sections) |
| 2 | `dashboard.html` | Full Portal-style dashboard (sidebar app shell) |
| 3 | `slides.html` | 11-slide GenLayer ecosystem-update deck (always dark) |
| 4 | `assets.html` | Every brand asset + how to use it |
| 5 | `about.html` | Text-only editorial page |
| — | `starter.html` | Minimal wired-up page you can copy to begin |

### Reference docs
`guidelines/brand_voice.md` (condensed copy + casing rules) ·
`guidelines/brand_guidelines_text.md` (full 48pp brand guidelines extract).

---

## 2. Quick start

```html
<!-- 1) tokens + fonts   2) components   3) theme + shared chrome. Order matters. -->
<link rel="stylesheet" href="{RAW_BASE}/colors_and_type.css">
<link rel="stylesheet" href="{RAW_BASE}/components.css">
<link rel="stylesheet" href="{RAW_BASE}/theme.css">
<!-- no-flash: set the theme before paint -->
<script>(function(){var t=localStorage.getItem('gl-theme')||'dark';
  document.documentElement.classList.toggle('on-dark',t==='dark');})()</script>
…
<script src="{RAW_BASE}/theme.js"></script>   <!-- before </body>: wires the toggle -->
<!-- For local/offline use, vendor these files + the /fonts folder and link relatively. -->
```

Then build with **tokens** (`var(--*)`) and **component classes** (`gl-*`) — never hard-code a
hex or px value that has a token:

```html
<h1 class="display-2">A world where trust is autonomous and universal</h1>
<p class="text-muted">Intelligent Contracts, self-interpreting and self-enforcing.</p>
<a class="gl-btn gl-btn--primary">Start building →</a>
```

### Light vs dark — one switch for the whole page

The system ships **dark/light theming** (§11). `theme.js` toggles the `.on-dark` class on `<html>`
and persists the choice; everything keyed to the surface tokens flips together. Drop a toggle button
anywhere:

```html
<button class="gl-theme-toggle" data-theme-toggle aria-label="Toggle theme">…</button>
```

To force a region to one theme regardless of the global choice, put `class="on-dark"` on a
container — every `gl-*` component inside flips to the dark surface palette. Use **page-chrome
tokens** (`--c-bg`, `--c-card`, `--c-border`, `--c-fg`, `--c-fg-muted`, `--c-fg-subtle`) for your
own page styles so they theme too — never hard-code `#fff`/`#060606`.

---

## 3. Color tokens

Use `var(--token)`. Never invent hex values.

### Brand core — neutrals do the heavy lifting
| Token | Hex | Role |
| --- | --- | --- |
| `--gl-black` | `#000000` | Primary text, bold surfaces |
| `--gl-white` | `#FFFFFF` | Primary surface |
| `--gl-navy` | `#282B5D` | Authoritative dark when black is too harsh |
| `--gl-blue` | `#110FFF` | **The single accent** — electric blue |
| `--gl-purple` | `#9B6AF6` | Gradient mid (not a flat fill) |
| `--gl-pink` | `#E37DF7` | Gradient start (not a flat fill) |

### Category colors (Portal — scoped, don't use outside their domain)
| Domain | Token | Hex |
| --- | --- | --- |
| Builder | `--gl-orange` (`-hi`/`-lo`) | `#EE8D24` (`#F8B93D`/`#DB6917`) |
| Validator | `--gl-sky` (`-hi`/`-lo`) | `#387DE8` (`#6DA7F3`/`#2159D2`) |
| Community | `--gl-purple` (`-hi`/`-lo`) | `#9B6AF6` (`#A77FEE`/`#5F33CA`) |

### Neutrals & semantic roles
`--gl-ink-900 #0F0F0F` · `-800 #1F1F1F` · `-700 #383838` · `-500 #6B6B6B` · `-400 #ABABAB`
· `--gl-line #E5E5E6` (hairline) · `--gl-line-soft #F0F0F0` · `--gl-fog #F4F4F4` · `--gl-fog-hi #F8F8F8`

Semantic: `--bg-app` · `--bg-card` · `--bg-inverse` · `--fg-1` (primary text) ·
`--fg-2` (muted) · `--fg-3` (caption) · `--fg-inverse` · `--fg-link #4083EA` · `--border` ·
`--border-soft`. State: `--state-success #16A34A` · `--state-warn #F59E0B` · `--state-danger #E00000`.

### Gradients (the brand's primary motif)
```css
--gl-gradient:    linear-gradient(135deg, #E37DF7 0%, #9B6AF6 45%, #110FFF 100%); /* pink→purple→blue */
--grad-builder:   linear-gradient(168deg, #F8B93D 15%, #EE8D24 50%, #DB6917 85%);
--grad-validator: linear-gradient(168deg, #6FA3F8 15%, #4F76F6 50%, #3B5DD6 85%);
--grad-community: linear-gradient(90deg,  #BE8FF5 0%, #AC6DF3 100%);
```
Apply the signature gradient on **dark backgrounds**, often gently animated (1.5–3s
ease-in-out loop). **Never** flood a large surface with flat purple or pink. Don't put the
gradient on body text except a hero headline fragment, sparingly.

---

## 4. Typography

| Token | Value | Use |
| --- | --- | --- |
| `--font-display` | `"Space Grotesk", …` | Titles & display. Use at **48px+**, weight 500 or 700, letter-spacing −0.04em to −0.07em. Geometric sans with subtle character — the brand's display voice in this public bundle. (Internal GenLayer projects can swap in F37 Lineca; see `LICENSE-FONTS.md`.) |
| `--font-body` | `"Switzer", …` | Body & small titles. **15–17px**, normal tracking. |
| `--font-mono` | `"JetBrains Mono", …` | Code only (brand has no prescribed mono). |

Scale: `--fs-display-1` clamp(64–160px) · `--fs-display-2` clamp(48–96px) ·
`--fs-display-3` clamp(32–64px) · `--fs-h1 40` · `--fs-h2 32` · `--fs-h3 24` · `--fs-h4 20`
· `--fs-body-lg 17` · `--fs-body 15` · `--fs-sm 14` · `--fs-xs 12`.

Line height: `--lh-tight 1.05` · `--lh-snug 1.2` · `--lh-normal 1.45` · `--lh-relaxed 1.65`.
Tracking: `--track-tight −0.04em` · `--track-snug −0.02em` · `--track-normal 0` · `--track-wide 0.02em`.

Helper classes baked into the CSS: `.display-1` `.display-2` `.display-3` `.text-muted`
`.text-caption`. Headings (`h1`–`h4`) are pre-styled with the display family + tight tracking.

**Labels / CTAs / stat headers:** Switzer semibold, **UPPERCASE, `letter-spacing: 0.1em`**.

---

## 5. Spacing, radii, shadow

Spacing (4px base): `--space-1 4` `-2 8` `-3 12` `-4 16` `-5 20` `-6 24` `-8 32` `-10 40`
`-12 48` `-16 64` `-20 80` `-24 96`.

Radii: `--radius-xs 4` · `-sm 6` · `-md 8` · `-lg 12` (**default card**) · `-xl 20`
(**signature pill button**) · `-full 999`.

Shadow (keep subtle — GenLayer never uses dramatic shadows):
`--shadow-card 0 4px 12px rgba(0,0,0,.05)` · `--shadow-card-hi 0 8px 24px rgba(0,0,0,.08)`
(the max) · `--shadow-inset` (lit-from-above hairline on dark) ·
`--shadow-glow 0 0 80px rgba(155,106,246,.35)` (gradient bloom on dark).

Hairlines are **1.2px** (Portal convention), not 1px.

---

## 6. Voice & content

Intelligent, precise, slightly grand — a civic institution for AI. Big declarations about
trust, consensus, adjudication, then concrete builder verbs. ("Build. Earn. Permanently.")

- **Display/hero titles:** sentence case. **Proper nouns:** Title Case (Intelligent Contracts,
  Testnet Bradbury, Deepthought DAO, Optimistic Democracy, Mochi).
- **Labels/CTAs:** UPPERCASE, 0.1em tracking. **Body:** sentence case.
- Triads of self-X verbs ("self-interpreting, self-executing, self-enforcing") are a device.
- Named eras: **Asimov** (past testnet), **Bradbury** (current). Numbers as digits (`$5,000`, `20%`).
- Arrow `→` at the end of links/CTAs is idiomatic ("View all →"). Em-dashes & colons welcome.
- **Never** say: leverage, unlock value, revolutionary, game-changing, generic web3 buzzwords.
- **No emoji**, anywhere.

See `guidelines/brand_voice.md` for the full rules and example lines.

---

## 7. Layout & motion

- **Portal cards:** `border-radius: 12px`, `1.2px solid var(--border)`, `var(--shadow-card)`. That's it.
- **Pill buttons:** `height: 40px; border-radius: 20px;` — half-height radius is the signature shape.
- **Dark hero surfaces:** near-black `#060606` with two blurred radial blooms (pink→purple in
  one corner, blue in another, ~40px blur).
- **Max content width** ~1200px on marketing surfaces; sidebar + main on app surfaces.
- Dashboard grid: `gap-8` columns, `space-y-8` stacked sections. Mobile padding `px-4`.
- **Motion:** gradients flow gently (1.5–3s ease-in-out). Hover deepens color one step,
  `transition: .15s`. **No scale, no bounce, no spring.** Page enters: fade + 10px translate.
- **No glassmorphism** as a system pattern. Surfaces are opaque. Blur is reserved for hex-blur
  PNGs behind badges and navy/black overlays (`rgba(0,0,0,.4–.6)`) over hero imagery.

---

## 8. Iconography & the mark

- The **Strong Mark** (triangle / hexagon) is the center of gravity — favicon to 1080px hero.
  Always from `assets/GenLayer_Mark_*.svg`. Place it prominently.
- Sizes: `16–20px` inline · `24px` stat rows · `32–48px` badge clusters.
- Tinted container pattern for small icons on busy surfaces: `padding: 6px;
  background: var(--gl-orange-lo|…); border-radius: 8px` (use the category tint).

---

## 9. Don'ts

- ❌ Recreate the wordmark with CSS text — use the SVG.
- ❌ Flat-fill a surface with just purple or pink — that range is for the **gradient**.
- ❌ Invent category colors beyond Builder/Validator/Community.
- ❌ Shadows larger than `0 8px 24px rgba(0,0,0,.08)`.
- ❌ Default line-height on big headlines — tighten to 0.9–1.05.
- ❌ Emoji. Glassmorphism everywhere. Rainbow gradients. Generic web3 slop.

---

## 10. Components (`components.css`)

Link `components.css` after `colors_and_type.css`. Every class is prefixed `gl-` so it won't
collide with a host project, and reads surface tokens that adapt to light/dark context (wrap in
`.on-dark`). **Open `components.html` for a live gallery of all of these.**

### Buttons — `gl-btn`
```html
<a class="gl-btn gl-btn--primary">Primary</a>     <!-- electric blue -->
<a class="gl-btn gl-btn--white">White</a>          <!-- strong contrast on dark -->
<a class="gl-btn gl-btn--ghost">Ghost</a>
<a class="gl-btn gl-btn--outline">Outline</a>
```
Modifiers: `--sm` `--lg`, attribute `disabled`. 40px pill by default; hover deepens color only.

### Badges & chips — `gl-badge`, `gl-chip`
`gl-badge` + `--builder` / `--validator` / `--community` (category) or `--success` / `--warn` /
`--danger` / (neutral, no modifier). Add a `<span class="gl-dot"></span>` for a status dot.
`gl-chip` is a removable filter pill with a `<button>×</button>`.

### Forms — `gl-field`, `gl-label`, `gl-input`, `gl-select`, `gl-textarea`
```html
<div class="gl-field">
  <label class="gl-label">Track</label>
  <select class="gl-select"><option>All tracks</option></select>
  <span class="gl-help">Helper text.</span>
</div>
```
- **Search:** wrap `<div class="gl-search">` around an SVG icon + `gl-input`.
- **Checkbox / radio:** `<label class="gl-check">` / `gl-radio` containing `<input>` + `<span class="box"></span>` + text.
- **Toggle:** `<label class="gl-toggle">` containing `<input type="checkbox">` + `<span class="track"></span>` + text.

### Filter bar / tabs — `gl-seg`, `gl-tabs`
`gl-seg` is a segmented control (`gl-seg__item`, add `--active`). `gl-tabs` is an underline tab row
(`gl-tab`, add `--active`).

### Cards — `gl-card`
```html
<div class="gl-card">
  <div class="gl-card__header"><h3 class="gl-card__title">Title</h3><span class="gl-badge …">…</span></div>
  <div class="gl-card__body"><p>…</p></div>
  <div class="gl-card__footer">…</div>
</div>
```
Modifiers: `--flat` (no shadow), `--hover` (lift on hover), `--bloom` (gradient glow, dark feature card).

### Stats — `gl-stat`, `gl-stat-grid`
`gl-stat` with `gl-stat__label` + `gl-stat__value` (add `--grad` for gradient text) + optional
`gl-stat__delta` (`--up` / `--down`). `gl-stat-grid` lays 4 stats in a hairline-divided grid.

### Table / leaderboard — `gl-table`
Standard `<table class="gl-table">`. `<th class="sortable">` adds a sort caret (`sorted-asc` /
`sorted-desc`). Use `.num` on numeric th/td (right-aligned, mono). `.rank` / `.rank--medal` for rank
cells, `gl-cell-user` (with `gl-avatar`, `.name`, `.addr`) for the identity column.

### Avatar — `gl-avatar`
Gradient initials chip; `--sm` / `--lg`.

### Progress — `gl-progress`
`<div class="gl-progress"><div class="gl-progress__bar" style="width:64%"></div></div>`. Bar
variants: `--gradient`, `--builder`, `--validator`, `--community`.

### Alerts — `gl-alert`
`gl-alert` + `--info` / `--success` / `--warn` / `--danger`. Left accent border + icon slot.

### Charts — `gl-bars`, `gl-linechart`, `gl-donut`, `gl-spark`
Dependency-free; geometry is data-driven, color/chrome from CSS.
- **Bars:** `<div class="gl-bars">` of `<div class="gl-bar" style="height:64%">` (variants `--gradient` / `--builder` / `--validator` / `--community` / `--muted`; `data-value` shows on hover). Pair with `gl-axis` labels.
- **Line / area:** an inline `<svg class="gl-linechart" viewBox="0 0 W H" preserveAspectRatio="none">` with a `.grid` group, an `.area` path (fill `url(#gl-area-grad)`) and a `.line` path (`--gradient` for the brand stroke). Define the two `<linearGradient>`s (`gl-area-grad`, `gl-line-grad`) once per page.
- **Donut:** `<div class="gl-donut" style="background:conic-gradient(…)">` inside `gl-donut-wrap`, with `gl-donut-center` for the middle label. The hole is a CSS mask, so it's background-agnostic.
- **Sparkline:** tiny `<svg class="gl-spark">` with a `.line` (`--up` / `--down`) — drops into stat cards and table rows.
Use `gl-legend` / `gl-legend__item` / `gl-legend__swatch` for keys.

### Pagination & misc
`gl-pager` (button group; `aria-current="true"` marks the current page), `gl-eyebrow` (pill label
with `gl-dot`), `gl-kbd` (inline code/key), `gl-divider` (hairline rule).

### Portal patterns (in `components.html`)
App-specific composed components reconstructed from the `genlayer-foundation/points` codebase (merged into the
components gallery under the `Portal ·` section kickers). **Hexagon role icons** — the **real Portal SVGs** (`assets/icons/hexagon-{builder,validator,community,
steward}.svg` gradient hexagons + white glyphs `terminal-fill-white` / `shield-white` / `group-white` /
`seedling-line`); GenLayer uses a navy `clip-path` hexagon + `gl-symbol-white` so it reads on light *and* dark. Plus the
**profile header** (gradient banner + overlapping avatar + role badges + copyable address + social pills),
**profile stat cards**, and **role sections** (category-themed header + rank pill + tinted stat tiles).
Category colors: Builder `--gl-orange`, Validator `--gl-sky`, Community `--gl-purple`, Steward `#19A663`.

---

## 11. Theming & shared chrome (`theme.css` + `theme.js`)

Link `theme.css` after `components.css`, and load `theme.js` before `</body>`. Together they give
every page a single dark/light switch plus a consistent nav / hero / section / footer shell.

### How it works
- **One class, whole page.** `theme.js` toggles `.on-dark` on `<html>` and saves the choice to
  `localStorage["gl-theme"]` (default `dark`). Add the no-flash inline snippet (see §2) so the
  correct theme is set before first paint.
- **Surface tokens flip.** Light is the base; `.on-dark` overrides the `--c-*` set:
  `--c-bg` · `--c-bg-deep` · `--c-card` · `--c-card-hover` · `--c-border` · `--c-border-strong`
  · `--c-fg` · `--c-fg-muted` · `--c-fg-subtle` · `--c-field-bg` · `--c-field-border` · `--c-hairline`
  · `--c-nav-bg` · `--c-bloom` (1 dark / 0 light) · `--c-logo-invert` (invert the black wordmark on dark).
  Build page styles from these, not raw hex.
- **Toggle button:** drop `<button class="gl-theme-toggle" data-theme-toggle>` (with the moon/sun
  SVGs) anywhere — `theme.js` wires every one. `window.GLTheme.set('dark'|'light')` / `.toggle()` too.

### Shared chrome classes
- **Nav:** `gl-nav` › `gl-nav__inner` › `gl-nav__brand` (logo auto-inverts) + `gl-nav__links`
  (`a[aria-current="page"]` marks the active page) + `gl-nav__right`. Keep the link order
  **Components · Portal · Dashboard · Slides · Assets · About** on every page.
- **Shell:** wrap content in `gl-page`; add `gl-page__bloom gl-page__bloom--1` / `--2` for the two
  gradient blooms (auto-hidden on light). `gl-wrap` centers content (max 1160px) above the blooms.
- **Hero:** `gl-hero` › `h1` + `gl-lede`; use `gl-grad-text` for the gradient headline fragment.
- **Section:** `gl-block` › `gl-block__head` (`gl-kicker` + `h2` + `p`). **Footer:** `gl-footer`.

> Decks (`slides.html`) stay dark by design — presentations are a fixed dark surface and don't
> carry the toggle.

---

## Caveats

- **Fonts.** This public repo uses **Space Grotesk** (Google Fonts, OFL) for display and
  **Switzer** (Fontshare, free for self-host) for body — both are safe to redistribute,
  so any consumer (internal team, ecosystem builder, hackathon project) can use this repo
  unchanged. The brand's original display face is **F37 Lineca** (foundry37.com,
  commercial). GenLayer Foundation holds a license that covers **Foundation, Labs, and
  Foundation-commissioned work** — those teams can drop the real Lineca in (`.ttf`
  available via the Foundation's private asset store or existing internal repos like
  `genlayer-foundation/points`) using the recipe in `LICENSE-FONTS.md` for a
  pixel-faithful match. Third-party / ecosystem projects must stay on Space Grotesk.
- **Portal UI** was reconstructed from the `genlayer-foundation/points` codebase (Svelte 5 +
  Tailwind), the source of truth for anything not represented here.
- **"Data-handling" 3D renders** (chrome/glass hero objects) are bespoke per campaign and not
  bundled. Use placeholders and flag when one is needed.
