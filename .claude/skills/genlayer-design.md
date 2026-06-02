---
name: genlayer-design
description: GenLayer Design System expert guidance — brand tokens, components, theming, voice, and assets. Use when building, styling, or reviewing UI for GenLayer Foundation, GenLayer Labs, or any GenLayer ecosystem project (websites, dashboards, decks, marketing pages, hackathon submissions). Covers the gl-* component library, color/type/spacing tokens, dark/light theming, brand voice, and where to fetch logos, icons, and UI kits on demand.
---

# GenLayer Design System

You are working on a project that should follow the **GenLayer Design System**. This
system is a portable visual reference maintained at:

**Repo:** <https://github.com/genlayer-foundation/genlayer-design>
**Single source of truth:** <https://raw.githubusercontent.com/genlayer-foundation/genlayer-design/main/DESIGN.md>

## How to use this skill

1. **Fetch `DESIGN.md` once at the start of the task** (use WebFetch on the raw URL
   above). It contains every token value, voice rule, layout pattern, and component
   inline — so you can produce on-brand work *without fetching anything else*.
2. **Only download other files when you actually need to render them** — the manifest in
   `DESIGN.md` §1 lists every file with its `{RAW_BASE}/...` URL (fonts, SVG logos &
   icons, hex-blur textures, ready-to-fork UI kits).
3. **For visual ground-truth**, the repo has five cross-linked reference pages —
   `components.html`, `dashboard.html`, `slides.html`, `assets.html`, `about.html` — open
   them in a browser if you need to *see* a component or pattern. They all share one nav
   and a dark/light toggle.

## Wire-up (drop-in)

```html
<link rel="stylesheet" href="https://raw.githubusercontent.com/genlayer-foundation/genlayer-design/main/colors_and_type.css">
<link rel="stylesheet" href="https://raw.githubusercontent.com/genlayer-foundation/genlayer-design/main/components.css">
<link rel="stylesheet" href="https://raw.githubusercontent.com/genlayer-foundation/genlayer-design/main/theme.css">
<script>(function(){var t=localStorage.getItem('gl-theme')||'dark';
  document.documentElement.classList.toggle('on-dark',t==='dark');})()</script>
…
<script src="https://raw.githubusercontent.com/genlayer-foundation/genlayer-design/main/theme.js"></script>
```

For production, vendor those files into the project (don't hot-link to raw.githubusercontent
on a live site — it's not a CDN). For prototypes, mocks, and internal tools, hot-linking is
fine.

## Non-negotiable rules

These come from `DESIGN.md` §3–§9. Read them as hard constraints when generating UI:

### Use tokens, never raw values
- Colors: `var(--gl-blue)`, `var(--gl-orange)`, `var(--fg-1)`, `var(--c-bg)`, never `#110FFF`.
- Spacing: `var(--space-6)`, never `24px`.
- Radii: `var(--radius-lg)` for cards, `var(--radius-xl)` for **pill buttons**.
- Type: `var(--font-display)`, `var(--font-body)`, `var(--font-mono)`. Sizes via
  `var(--fs-display-2)`, `var(--fs-h2)`, `var(--fs-body)`, etc.

### Use the component library, don't reinvent
- Buttons: `gl-btn gl-btn--primary | --white | --ghost | --outline` (40px pills).
- Cards: `gl-card` (+ `__header`/`__body`/`__footer`, modifiers `--hover` / `--bloom`).
- Forms: `gl-field` › `gl-label` + `gl-input | gl-select | gl-textarea` + `gl-help`.
- Tables / leaderboards: `gl-table` (with `.num`, `.rank`, `gl-cell-user`).
- Charts: `gl-bars`, `gl-linechart`, `gl-donut`, `gl-spark` (dependency-free).
- Badges: `gl-badge gl-badge--builder | --validator | --community | --success | --warn | --danger`.
- Alerts: `gl-alert gl-alert--info | --success | --warn | --danger`.
- Full catalogue in `DESIGN.md` §10.

### Theming
- Default is dark. Add `class="on-dark"` to any container to force dark; toggle the
  `.on-dark` class on `<html>` for whole-page theming (`theme.js` does this with
  `localStorage["gl-theme"]`).
- Build *page* styles from the surface tokens — `--c-bg`, `--c-card`, `--c-border`,
  `--c-fg`, `--c-fg-muted`, `--c-fg-subtle`, `--c-nav-bg`, `--c-bloom`, `--c-logo-invert`
  — so they flip with the theme. Never hard-code `#fff` or `#060606`.

### Brand identity rules
- **The Mark** (triangle/hexagon, `assets/GenLayer_Mark_{Black|White}.svg`) is the center of
  gravity. Use the SVG — never recreate the wordmark in CSS text.
- **The gradient** (`--gl-gradient`: pink → purple → blue, 135°) is for **dark surfaces**,
  often gently animated. Never flood a large surface with flat purple or pink.
- **Category colors** are scoped: Builder = `--gl-orange`, Validator = `--gl-sky`,
  Community = `--gl-purple`, Steward = `#19A663`. Don't invent new categories.
- **No emoji**, anywhere. If an icon is missing, fall back to **Lucide** (thin, geometric)
  and flag the substitution.
- **No glassmorphism** as a system pattern. Surfaces are opaque.
- Shadows stay subtle — max is `0 8px 24px rgba(0,0,0,.08)`. Hairlines are **1.2px**.

### Voice & copy (from `DESIGN.md` §6 + `guidelines/brand_voice.md`)
- Intelligent, precise, slightly grand — a civic institution for AI.
- Display/hero titles in **sentence case**. Proper nouns (Intelligent Contracts, Testnet
  Bradbury, Deepthought DAO, Optimistic Democracy) in **Title Case**.
- Labels/CTAs: **UPPERCASE, `letter-spacing: 0.1em`**.
- Triads of self-X verbs are a device ("self-interpreting, self-executing, self-enforcing").
- Arrow `→` at the end of CTAs is idiomatic ("View all →").
- **Never** say: leverage, unlock value, revolutionary, game-changing, generic web3 buzzwords.

## Fonts

- This system uses **Space Grotesk** (Google Fonts, OFL) for display and **Switzer**
  (Fontshare) for body. Both are loaded automatically by `colors_and_type.css`. No setup
  needed.
- The brand's original display face is **F37 Lineca** (commercial). If the project is
  inside **GenLayer Foundation**, **GenLayer Labs**, or a **Foundation-commissioned**
  engagement, you may layer in the real Lineca for a pixel-faithful match. Recipe and
  permitted-users list: see `LICENSE-FONTS.md` in the repo. **Never** commit
  `F37Lineca-VF.ttf` to a public repo.

## When the user asks you to design or build UI

1. Fetch `DESIGN.md` (and `LICENSE-FONTS.md` if fonts come up).
2. Confirm the target surface: dark (default) or light? If unclear, ask.
3. Build with tokens + `gl-*` classes. If you need a pattern not in the library, search
   the `components.html` / `dashboard.html` source for a close match before inventing one.
4. For any assets (logos, mark, badges, hex-blur textures, track icons, role hexagons),
   fetch the SVG/PNG from the repo's `assets/` (manifest in `DESIGN.md` §1) — don't
   recreate them.
5. For a full app shell, fork `ui_kits/portal/Dashboard.html` (Portal-style) or
   `ui_kits/marketing/HackathonPage.html` (dark marketing page). They reference
   `colors_and_type.css` via `../../`.

## When the user asks you to *review* UI for brand fit

Check, in order: (1) tokens are used (no raw hex / px that has a token), (2) `gl-*`
component classes are used where applicable, (3) surface tokens are used so the page
themes, (4) the Mark is from the SVG (not recreated), (5) voice rules are followed
(sentence case, no banned words, no emoji), (6) shadows / radii / hairlines match the
system. Cite `DESIGN.md` section numbers when flagging issues.
