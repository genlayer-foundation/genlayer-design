# GenLayer Design System

A portable, drop-in visual system for building branded **GenLayer** interfaces, prototypes,
mocks, and slides — the adjudication layer for the agentic economy.

## Start here → [`DESIGN.md`](./DESIGN.md)

`DESIGN.md` is the single entry point. It documents every design token, the brand voice,
layout patterns, theming, and a download manifest so an AI agent or developer can pull exactly
the files they need — fonts, CSS, logos, icons, UI kits — on demand.

### Wire it up

```html
<link rel="stylesheet" href="colors_and_type.css"><!-- tokens + fonts -->
<link rel="stylesheet" href="components.css">     <!-- gl-* components -->
<link rel="stylesheet" href="theme.css">          <!-- shared chrome + dark/light -->
<script>(function(){var t=localStorage.getItem('gl-theme')||'dark';
  document.documentElement.classList.toggle('on-dark',t==='dark');})()</script>
…
<script src="theme.js"></script>                  <!-- before </body>: wires the toggle -->
```

That loads the brand fonts, defines every CSS variable (`--gl-blue`, `--font-display`,
`--space-6`, `--radius-xl`, …), gives you ready-made component classes (`gl-btn`, `gl-card`,
`gl-table`, badges, fields, charts), and adds a **dark/light theme toggle** that persists across
pages. Everything is keyed to surface tokens, so one switch flips the whole page.

## What's in here

Six reference pages — one consistent set, same nav, same chrome, all dark/light themeable:

| # | Page | What it is |
| --- | --- | --- |
| 1 | **[`components.html`](./components.html)** | Every component + charts **and** the composed Portal patterns (hexagon role icons, profile header, stat cards, role sections) |
| 2 | **[`dashboard.html`](./dashboard.html)** | Full Portal-style dashboard (sidebar app shell, KPIs, charts, leaderboard) |
| 3 | **[`slides.html`](./slides.html)** | 11-slide GenLayer ecosystem-update deck (always dark) |
| 4 | **[`assets.html`](./assets.html)** | Every brand asset (logos, mark, icons, badges, textures) with usage notes |
| 5 | **[`about.html`](./about.html)** | Text-only editorial page |

Every page carries the same nav (**Components · Dashboard · Slides · Assets · About**)
and a theme toggle, so you can browse and theme the whole system from any one. `DESIGN.md` §10
documents every component class; §11 covers theming and the shared chrome.

```
DESIGN.md                        ← read this first
colors_and_type.css              ← all tokens + @font-face + base styles
components.css                   ← reusable, theme-aware gl-* component classes
theme.css                        ← shared chrome (nav/hero/sections) + dark/light tokens
theme.js                         ← persists + toggles the dark/light theme
components.html                  ← 1 · components + charts + Portal patterns
dashboard.html                   ← 2 · full dashboard (sidebar app shell)
slides.html                      ← 3 · 11-slide ecosystem-update deck
assets.html                      ← 4 · brand-asset reference with usage notes
about.html                       ← 5 · text-only editorial page
starter.html                     ← minimal wired-up example you can copy to begin
deck-stage.js                    ← slide-deck engine used by slides.html
fonts/                           ← Switzer (variable). Space Grotesk loads from Google Fonts.
assets/                          ← logos, mark, icons, badges, textures, backgrounds
ui_kits/portal/Dashboard.html    ← full Portal app shell to fork
ui_kits/marketing/HackathonPage.html  ← full dark marketing page to fork
guidelines/brand_voice.md        ← condensed copy & casing rules
guidelines/brand_guidelines_text.md   ← full brand guidelines extract
LICENSE-FONTS.md                 ← font licensing note (read before forking public)
```

## Using it as a remote reference

Point any project's agent at the raw `DESIGN.md` URL — that's the single entry point:

```
https://raw.githubusercontent.com/genlayer-foundation/genlayer-design/main/DESIGN.md
```

The agent will read all tokens inline (so it can produce on-brand work without fetching
anything else) and only download individual files — Switzer, SVG logos/icons, the UI kits
— when it needs to actually render them. §1 of `DESIGN.md` is the full download manifest
with `{RAW_BASE}/...` URLs.

### Drop-in snippet for a project's CLAUDE.md / AGENTS.md

```md
## Visual design

This project follows the GenLayer Design System.

- **Read first:** https://raw.githubusercontent.com/genlayer-foundation/genlayer-design/main/DESIGN.md
- **Live reference pages:** https://github.com/genlayer-foundation/genlayer-design
  (open `components.html`, `dashboard.html`, `assets.html`, `about.html`, `slides.html`
  in a browser to see every token, component, and pattern in dark and light).
- **Wire-up:** link `colors_and_type.css`, `components.css`, `theme.css`, and load
  `theme.js` before `</body>` (see DESIGN.md §2). Build everything from `var(--*)`
  tokens and `gl-*` component classes — never hard-code a hex or px that has a token.
```

## License

Code, tokens, and docs: released by GenLayer Foundation for use in GenLayer ecosystem
projects and beyond.

### Fonts at a glance

- **Space Grotesk** (display, OFL via Google Fonts) and **Switzer** (body, Fontshare) —
  free to use and redistribute. Anyone can link this repo and ship.
- **F37 Lineca** (the brand's original display face, commercial) — **not** in this public
  repo. **GenLayer Foundation, GenLayer Labs, and Foundation-commissioned projects** are
  covered by the Foundation's license and can layer Lineca on top of this system for a
  pixel-faithful match. The `.ttf` is available through the Foundation's private asset
  store, existing internal repos (e.g. `genlayer-foundation/points`), or directly from
  Foundry F37 — see [`LICENSE-FONTS.md`](./LICENSE-FONTS.md) for who's permitted, where
  to obtain it, and the drop-in CSS recipe.
- **Third-party / ecosystem / hackathon projects:** stay on Space Grotesk — that's
  exactly why this repo defaults to it.
