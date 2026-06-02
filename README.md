# GenLayer Design System

A portable, drop-in visual system for building branded **GenLayer** interfaces, prototypes,
mocks, and slides — the adjudication layer for the agentic economy.

---

## Use this in your project

Pick whichever fits how you work. **The skill is the simplest path for any Claude-Code-
assisted project** — install it once and it applies everywhere.

### Option A — Install the Claude Code skill (recommended)

The repo ships a [Claude Code skill](https://docs.claude.com/en/docs/claude-code/skills)
at [`.claude/skills/genlayer-design.md`](./.claude/skills/genlayer-design.md). Install it
once at the user level — it auto-applies whenever you ask Claude to design, build, or
review UI in **any project** on your machine. No per-project `CLAUDE.md` edit, no copy-
paste, no maintenance.

```bash
# one-time install — run on any machine where you use Claude Code
mkdir -p ~/.claude/skills
curl -fsSL https://raw.githubusercontent.com/genlayer-foundation/genlayer-design/main/.claude/skills/genlayer-design.md \
  -o ~/.claude/skills/genlayer-design.md
```

That's it. From now on, just describe the work — *"design a hackathon landing page
following the GenLayer design system"*, *"build a leaderboard table for the Portal"*,
*"review this React component for brand fit"* — and Claude will pick up the
`genlayer-design` skill, fetch `DESIGN.md`, and apply tokens, `gl-*` components, theming,
brand voice, and asset rules automatically.

To update later, re-run the same `curl` command. To uninstall, delete
`~/.claude/skills/genlayer-design.md`.

### Option B — Paste a snippet into the project's CLAUDE.md / AGENTS.md

If you'd rather not install the skill globally (e.g. you share the project with people
who use a different AI tool, or you want the design-system reference checked into the
repo itself), paste this into the project's `CLAUDE.md` / `AGENTS.md`:

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

### Option C — Wire the stylesheets directly (no AI involved)

If you're hand-building UI and just want the tokens + components in your page, link the
three stylesheets and load the theme script:

```html
<link rel="stylesheet" href="https://raw.githubusercontent.com/genlayer-foundation/genlayer-design/main/colors_and_type.css">
<link rel="stylesheet" href="https://raw.githubusercontent.com/genlayer-foundation/genlayer-design/main/components.css">
<link rel="stylesheet" href="https://raw.githubusercontent.com/genlayer-foundation/genlayer-design/main/theme.css">
<script>(function(){var t=localStorage.getItem('gl-theme')||'dark';
  document.documentElement.classList.toggle('on-dark',t==='dark');})()</script>
…
<script src="https://raw.githubusercontent.com/genlayer-foundation/genlayer-design/main/theme.js"></script>
```

For production sites, **vendor those files into your project** — don't hot-link to
`raw.githubusercontent.com` on a live deployment (it's not a CDN, no caching guarantees).
For prototypes, mocks, and internal tools, hot-linking is fine.

---

## What's in here

`DESIGN.md` is the single source of truth — every design token, the brand voice, layout
patterns, theming, and a download manifest. Read that first if you want the full picture.

Five reference pages — one consistent set, same nav, same chrome, all dark/light themeable:

| # | Page | What it is |
| --- | --- | --- |
| 1 | **[`components.html`](./components.html)** | Every component + charts **and** the composed Portal patterns (hexagon role icons, profile header, stat cards, role sections) |
| 2 | **[`dashboard.html`](./dashboard.html)** | Full Portal-style dashboard (sidebar app shell, KPIs, charts, leaderboard) |
| 3 | **[`slides.html`](./slides.html)** | 11-slide GenLayer ecosystem-update deck (always dark) |
| 4 | **[`assets.html`](./assets.html)** | Every brand asset (logos, mark, icons, badges, textures) with usage notes |
| 5 | **[`about.html`](./about.html)** | Text-only editorial page |

Every page carries the same nav (**Components · Dashboard · Slides · Assets · About**)
and a theme toggle, so you can browse and theme the whole system from any one. `DESIGN.md`
§10 documents every component class; §11 covers theming and the shared chrome.

```
DESIGN.md                        ← single source of truth
.claude/skills/genlayer-design.md ← Claude Code skill (Option A above)
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
LICENSE-FONTS.md                 ← font licensing (Space Grotesk public, F37 Lineca internal)
```

---

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
