---
name: genlayer-video
description: GenLayer video and motion-graphics guidance — explainer films, launch videos, animated text, storyboards and animatics. Use whenever making, storyboarding, scripting, animating, rendering or reviewing any GenLayer video or motion piece (explainers, launch teasers, social clips, animated diagrams, end cards, GSAP / HyperFrames / Remotion / After Effects timelines, SVG-per-frame renders, letter-by-letter text reveals). Covers the illustration vocabulary (cubes, validators, pills, coins), colour and type for video, the two motion languages (explainer vs launch), voice, sound, and how to state GenLayer consensus facts correctly. Sibling of genlayer-design, which covers interfaces.
---

# GenLayer video & motion

You are making a GenLayer **video or motion piece**. Its rules live in the design repo:

**Repo:** <https://github.com/genlayer-foundation/genlayer-design>
**Raw base:** `https://raw.githubusercontent.com/genlayer-foundation/genlayer-design/main/guidelines/video/`

If the repo is checked out locally (e.g. `projects/design/`), read the files from
`guidelines/video/` there instead of fetching.

## Read before you draw or animate

1. **Always:** `video_illustration.md` — the vocabulary (what each object means), colour,
   type, composition, the don'ts, and *Say it correctly*. Look at
   `video_illustration_sheet.png` too: it is the vocabulary on one board.
2. **Then exactly one motion language** — they are different and must not be mixed:

   | The piece is | Read | Feel |
   | --- | --- | --- |
   | An **explainer film** (voiceover, Fundamentals series) | `video_explainer_films.md` | Calm. `cubic-bezier(.45,0,.15,1)`, tiny match cuts (25 px / 3 %), 1 %/s drift |
   | A **launch video** or fast motion graphic | `video_launch_motion.md` | Sharp. `expo.out`/`expo.in`, `D = 1.0 s`, hard cuts, push-in camera |

   Unsure which? Ask.
3. **If any text animates by letter or word:** `video_text_motion.md` — glyph outlines for
   frame-by-frame renders, full copies of the text for live pages. Never `inline-block`
   per letter.

Interfaces (web pages, dashboards, decks) are **not** video — use the `genlayer-design`
skill and `DESIGN.md` for those.

## Non-negotiables (summary — the files win on detail)

- **Flat 2D.** White ground, `#070707` ink, `#F3F3F3` cards, **one blue `#110FFF`**,
  red `#FF3B30` only for failure. No shadows, gradients, glass, glow, 3D or WebGL.
- **Same object, same meaning, every piece.** Solid rounded cube = GenVM; sharp wireframe
  cube = a model; `[VALIDATOR n]` = a validator; one blue outline round the group =
  agreement; bracket text `[YES]` = a result; `$` coins on curves = money. Don't invent
  a new icon for an existing meaning, and give a new concept its own shape.
- **Words:** F37 Lineca Medium, -3 % tracking, no final full stop or comma, ~80 px on
  1920×1080, centred on the hero object. **Labels:** Suisse Intl Mono, upper case.
- **No fades in transitions**, ever. Cuts and match cuts only.
- **Nothing still:** every shot drifts or pushes in, restarted on each cut.
- **Facts:** a majority *accepts* (not unanimity); no consensus → a new leader retries
  (not an appeal); an appeal is an explicit challenge bringing 2n + 1 validators; *final*
  only after the appeal window closes, and only then do funds move. Engineering checks any
  on-screen code.
- **End card:** white wordmark on `#0A0A0A` — use the shipped SVG
  (`assets/GenLayer_Logo_White.svg`), never recreate it. Tagline "THE FIRST ADJUDICATION
  LAYER / FOR THE AGENTIC ECONOMY", decoded letter by letter.

## How to work

- **Stills before video.** Explore on a numbered board of stills; get a concept picked
  (show ~3) before building. Don't render a full film unasked — render short labelled
  test clips when unsure.
- **Get approval before spending credits** on voice (TTS) or music.
- **Music comes from an AI music model through an API, never synthesised in code.** First
  check whether a [fal.ai](https://fal.ai) key is available (`FAL_KEY`). fal hosts several
  music models behind one key, such as `fal-ai/elevenlabs/music`, `fal-ai/lyria2` (30 s max)
  and `fal-ai/minimax-music`. Each model has its own input schema at
  `fal.ai/models/<id>/api`. Where the model allows it, request section durations that land
  on the film's cuts. Use a new bed for each film, and keep the request (model and prompt)
  next to the track.
- **Notes loop:** map each timestamped note (30 fps) to a shot, fix it, check stills around
  that timecode, re-render, commit.
- **Before showing,** run the checklist at the end of the guide you followed (frames at
  `t−1F / t / t+1F` of every cut, contact sheet, one letter tracked frame by frame, facts
  against *Say it correctly*).

## Fonts

GenLayer holds licences for F37 Lineca, Suisse Intl and Suisse Intl Mono; the files are
**not** in the public repo — get them from the team, and **never commit them to a public
repo**. Variable Lineca must be instanced before opentype.js can read it
(`fonttools varLib.instancer F37Lineca-VF.ttf wght=500`). Third-party / ecosystem projects
use Space Grotesk and JetBrains Mono instead.

## When asked to review a video or timeline

Check, in order: (1) vocabulary — each object means what the sheet says; (2) palette —
one blue, red only for failure, no gradients/shadows; (3) type — Lineca words with no
final punctuation, mono upper-case labels; (4) motion — the right language for the piece,
no opacity tweens / `autoAlpha` / blur in transitions (`grep -nE
"blur|autoAlpha|opacity:\s*0\.[0-9]|crossfade|dissolve"`), no still shots; (5) text —
no per-letter `inline-block`, outlines in renders; (6) facts — *Say it correctly*. Cite the
guide file and section when flagging.
