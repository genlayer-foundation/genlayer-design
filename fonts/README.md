# Fonts

The GenLayer system uses two typefaces in this public bundle:

| Family | How it loads | Role |
| --- | --- | --- |
| **Space Grotesk** | Google Fonts (`@import` in `../colors_and_type.css`) | Display / titles. Geometric sans with a touch of warmth — the brand's display voice in the public bundle. Weights **300–700**. |
| **Switzer** | Self-hosted (`Switzer-Variable.ttf` + `Switzer-VariableItalic.ttf`, variable **100–900**) | Body & small titles. |

No setup needed — linking `colors_and_type.css` pulls Space Grotesk from Google Fonts and
defines `@font-face` for Switzer. Reference them in your CSS as `var(--font-display)` and
`var(--font-body)`.

## Licensing

- **Space Grotesk** — [SIL Open Font License](https://fonts.google.com/specimen/Space+Grotesk).
  Free to use, self-host, and redistribute.
- **Switzer** — [Fontshare License](https://www.fontshare.com/fonts/switzer). Free for
  commercial use and self-hosting.

See [`../LICENSE-FONTS.md`](../LICENSE-FONTS.md) for the full note.

---

## Internal GenLayer projects: F37 Lineca

The brand's original display face is **F37 Lineca** (foundry37.com), a commercial typeface
licensed to GenLayer Foundation. **It is not in this public repo** and is **not** for
use by third-party ecosystem projects.

If you are working inside **GenLayer Foundation**, **GenLayer Labs**, or a
**Foundation-commissioned project**, you are covered by the Foundation's license and can
use Lineca. See [`../LICENSE-FONTS.md`](../LICENSE-FONTS.md) for:

- Who is permitted to use it (and who isn't).
- Where to get the `.ttf` (private asset store, an existing internal repo like
  `genlayer-foundation/points`, or direct from Foundry F37 with team credentials).
- The drop-in CSS recipe to layer Lineca on top of this system without touching anything
  else.

> ⚠️ Never commit `F37Lineca-VF.ttf` to a public repository.
