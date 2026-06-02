# Font Licensing

This is a **public** repository, so it ships only fonts that are free to redistribute.
The display face you see here (Space Grotesk) is a deliberate substitute for the brand's
licensed display face (F37 Lineca) — close enough that ecosystem builders get an on-brand
experience, and licensed projects can swap in the real Lineca to be pixel-faithful.

## What this repo ships

| Font | License | Source |
| --- | --- | --- |
| **Space Grotesk** | [SIL Open Font License 1.1](https://openfontlicense.org/) | [Google Fonts](https://fonts.google.com/specimen/Space+Grotesk) — loaded via `@import` in `colors_and_type.css` |
| **Switzer Variable** | [Fontshare License](https://www.fontshare.com/fonts/switzer) — free for commercial use & self-host | [fontshare.com/fonts/switzer](https://www.fontshare.com/fonts/switzer) — self-hosted in `fonts/` |
| **JetBrains Mono** *(optional, code only)* | [OFL 1.1](https://openfontlicense.org/) | [Google Fonts](https://fonts.google.com/specimen/JetBrains+Mono) |

Anyone consuming this repo — internal GenLayer team, ecosystem builders, hackathon
participants, third-party integrators — can link `colors_and_type.css` and get a
type-system that is fully redistributable with no licensing concerns.

---

## Internal use: the real F37 Lineca

The GenLayer brand was designed against **[F37 Lineca](https://foundry37.com)**, a
commercial display typeface from Foundry F37. Its signature horizontal-bar letterforms
(the wide cross-stroke on `e`, `a`, `f`, the architectural `n`, the shortened `l`) are
part of the brand's visual identity — Space Grotesk is a close geometric cousin but does
**not** reproduce them.

### Who is allowed to use F37 Lineca

GenLayer Foundation holds a commercial license for F37 Lineca that covers internal teams
and Foundation-led work. Permitted users:

- **GenLayer Foundation** — official sites, marketing, decks, internal tools, the Portal
  (`portal.genlayer.foundation`), Testnet dashboards, etc.
- **GenLayer Labs** — engineering, research, and product work produced by the Labs team.
- **Foundation-commissioned work** — agencies, contractors, and partner studios producing
  deliverables *for* GenLayer Foundation or Labs under a written engagement.

**Not permitted:**

- Third-party / ecosystem projects building on GenLayer (use the public Space Grotesk
  stack — it's why this repo defaults to it).
- Hackathon submissions, community sites, fan projects, forks of GenLayer code.
- Any work where the resulting artifact is owned by someone other than GenLayer
  Foundation / Labs.

If you're unsure which bucket you fall into, default to Space Grotesk. The Foundation
brand team (see contacts at the end of this file) can confirm.

### How permitted users get the font

F37 Lineca is **not** in this public repo. Permitted users can obtain `F37Lineca-VF.ttf`
through one of these channels:

1. **GenLayer Foundation private asset store** — the canonical, up-to-date copy lives in
   the Foundation's internal asset distribution (private GitHub repo or 1Password vault,
   gated by GitHub team / SSO membership). Ask the brand team for access if you don't
   already have it.
2. **Your project repo** — if you're inside an existing GenLayer Foundation project
   (e.g. `genlayer-foundation/points`, the Portal), the font is already present in that
   repo's `frontend/public/fonts/` or equivalent. Use it from there.
3. **Direct from F37** — the Foundation's license entitles the team to download original
   copies from `foundry37.com`. The brand team can share download credentials with team
   members on request.

> ⚠️ **Never** check `F37Lineca-VF.ttf` into a public repo, paste it into a public CDN, or
> share it outside the Foundation/Labs perimeter. Foundries (F37 in particular) actively
> monitor for unauthorized distribution and a DMCA takedown or license revocation would
> affect the entire team.

### Wiring Lineca into a project that uses this design system

Assume you've placed `F37Lineca-VF.ttf` somewhere your app can serve it (e.g.
`public/fonts/F37Lineca-VF.ttf` in a Next.js / SvelteKit / Vite project). Then, after
linking `colors_and_type.css` from this repo, add a small override stylesheet:

```css
/* fonts-internal.css — load AFTER colors_and_type.css */
@font-face {
  font-family: "F37 Lineca";
  src: url("/fonts/F37Lineca-VF.ttf") format("truetype-variations"),
       url("/fonts/F37Lineca-VF.ttf") format("truetype");
  font-weight: 100 900;
  font-style: normal;
  font-display: swap;
}

:root {
  /* Lineca leads, Space Grotesk is the graceful fallback if the font 404s. */
  --font-display: "F37 Lineca", "Space Grotesk", ui-sans-serif, system-ui, sans-serif;
}
```

```html
<link rel="stylesheet" href="https://raw.githubusercontent.com/genlayer-foundation/genlayer-design/main/colors_and_type.css">
<link rel="stylesheet" href="/fonts-internal.css">
```

Everything else — tokens, components, theme — stays unchanged. The headlines just snap
into the real brand display face.

### What "pixel-faithful" looks like

- Lineca's tracking is tighter than Space Grotesk's; the system's `--track-tight: -0.04em`
  is tuned for Lineca first. Space Grotesk holds up at this tracking but reads slightly
  looser. If you swap in Lineca and headlines look airy, no other change is needed —
  they're now correct.
- Hero headlines (`display-1`, `display-2`) at 64–160px are where the Lineca character
  really appears (the horizontal bars, the architectural `n`). Body and small UI text is
  set in Switzer and is identical in both stacks.

---

## Everything else in this repo

CSS, design tokens, docs, SVG logos, brand marks, icons, badges, textures, UI kits, slide
templates — all GenLayer Foundation brand material, released for use across the GenLayer
ecosystem. Build with it.

---

## Contacts

- **Brand & font licensing questions:** the GenLayer Foundation brand team (reach out
  via the Foundation's internal Slack / Notion, or open an issue on this repo for public
  questions).
- **Foundry contact:** [foundry37.com](https://foundry37.com) for direct license
  inquiries (only relevant if you're spinning up a new GenLayer Foundation entity that
  needs its own license).
