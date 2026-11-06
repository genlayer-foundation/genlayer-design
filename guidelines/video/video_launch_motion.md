# Video: launch motion

The motion language for launch videos and motion graphics: always fast cuts, nothing is still, nothing fades. Apply the rules to the **whole** video, not just the scene you're touching. Explainers follow `video_explainer_films.md` instead.

## The five rules

### 1. No fades or blurs to enter or leave

Every element is whole and opaque on its first frame. The only thing that animates is its movement (`y`, `x`, `scale`).

- **Enter:** `opacity` to 1 with a `set` on the entry frame and, at that same instant, a move on `expo.out`.
- **Leave:** either a hard cut (`opacity` to 0 with a `set`) or a fast move out of frame on `expo.in` that ends exactly on the cut frame, where the element is removed.
- Forbidden: `opacity` tweens 0→1 or 1→0, `autoAlpha`, animated `filter: blur()`, veils that fade, colour changes by fading (colours also change on a cut).

**Why:** a fade or a blur hides the hit of the entrance. Without them, the expo curve does the work and the shot reads sharp on every frame.

### 2. Cuts instead of soft transitions

Between phrases, states and scenes, cut. If you need continuity, use a **cut with movement** (match cut): what leaves goes up fast (`expo.in`, ending on the cut) and what enters arrives from below (`expo.out`, starting on the cut). Never a crossfade, a fade to white or black, or a blur pass.

**Why:** a crossfade has frames where two things are half there. With a cut, every frame shows one whole thing.

**Never finish, hold, and hard-cut everything at once.** Every shot gets a way out inside it that mirrors its way in and ends exactly on the cut, so the next shot's entrance picks up the gesture:

- The way out follows the direction of the way in. Text typed left to right leaves left to right: each letter goes, in its place, in reading order. Never send letters flying up when they were typed in from the left.
- A line typed with a caret is deleted forward, the caret at its head.
- Figures go digit by digit, left to right; lines drawn from the left draw away to the right.
- Order: secondary things first (labels, captions), the main text last, so its last letter leaves on the cut frame and the next main text arrives on the next one. Inside a group, what came in first leaves first.
- The ground (background colour) still changes on the cut.

### 3. Short burst, long tail

The movement happens almost entirely in the first frames, then settles slowly. That's why it uses exponential curves and long durations, with no bounce or overshoot (`back`, `elastic`, overshoot).

Standard scale (30 fps). Don't invent loose durations; use these constants:

```js
const F = 1 / 30;
const D = 30 * F;       // 1.0 s: every entrance, exit, press and half cut
const D_LONG = 40 * F;  // 1.33 s: camera moves, opening windows, going full screen
```

Since there's no fade, the entrance has to read through movement, so distances are a bit larger than usual:

| What | Enter (`expo.out`, `D`) | Leave (`expo.in`) |
| --- | --- | --- |
| Word in a phrase | `y: 24 → 0` | hard cut |
| Block or headline in a match cut | `y: 160 → 0` | `y: 0 → -300` over `D`, ends on the cut |
| Card, pill or button | `y: 40 → 0` + `scale: 0.92 → 1` | whip `y: 0 → -80` in 10F + cut |
| Logo | `y: 24 → 0` + `scale: 0.88 → 1` | cut, or leaves upward in a match cut |

For more rhythm or more calm, change `D` and `D_LONG` in every scene at once and leave the curves alone. This scale has already been tuned twice (0.4 s felt too fast; 1.0 s was right).

### 4. Nothing still

While a shot holds, it keeps moving: a slow, linear, continuous push-in on an outer "camera" layer that wraps the shot. **It restarts on every cut.**

- Values: `scale 1 → 1.04` across the shot, or `1 + 0.008` per second with a `4 px/s` drift in `x`. Always `ease: "none"`.
- If the scene already has its own camera (zooms or pans), the push-in goes on a layer **outside** it. They don't fight.
- It's subtle: you should feel the shot is alive, not notice a zoom.
- If a brand forbids slow zooms, keep the shot alive with what's inside it instead (typing, counting, lines drawing).

**Why:** in a video cut like this, a still shot looks like a pause or a render bug.

### 5. Letter by letter without trembling

Follow `video_text_motion.md`. A render that seeks frame by frame (HyperFrames, Remotion) uses glyph outlines. A live page uses full copies of the text. Use no fade, because nothing fades here.

- Don't put `display: inline-block` on each letter to be able to move it: it breaks the kerning.
- Don't place separate copies of each letter with `will-change`: measured, each copy snaps on its own and the letters end up to 0.5 px off.

**Why:** the browser snaps text that moves inside its line to whole pixels, each letter out of step with its neighbours, and the line trembles.

In After Effects this doesn't happen (text layers move in subpixels). It only affects HTML (HyperFrames, scenes rendered in Chrome).

## Recipes (HyperFrames / GSAP)

Always on a single paused timeline that gives the same result when seeking to any point (`immediateRender: false` on the `fromTo`s that don't start at 0).

```js
// Enter: whole on its first frame; only the rise animates.
const enter = (el, t, from = { y: 24 }) => {
  tl.set(el, { opacity: 1 }, t);
  tl.fromTo(el, from, { y: 0, scale: 1, duration: D, ease: "expo.out", immediateRender: false }, t);
};

// Leave: a whip upward that ends on the cut; there it disappears.
const CUT_OUT = 10 * F;
const cutOut = (els, tCut, y = -80) => {
  tl.fromTo(els, { y: 0 }, { y, duration: CUT_OUT, ease: "expo.in", immediateRender: false }, tCut - CUT_OUT);
  tl.set(els, { opacity: 0 }, tCut);
};

// Match cut: A leaves upward until the cut; B arrives from below from the cut.
const matchCut = (out, inn, tCut) => {
  tl.fromTo(out, { y: 0 }, { y: -300, duration: D, ease: "expo.in", immediateRender: false }, tCut - D);
  tl.set(out, { opacity: 0 }, tCut);
  enter(inn, tCut, { y: 160 });
};

// Nothing still: one push-in per shot on the outer layer, restarted on every cut.
const shot = (t0, t1, to = 1.04) =>
  tl.fromTo("#scene-cam", { scale: 1 }, { scale: to, duration: t1 - t0, ease: "none", immediateRender: t0 === 0 }, t0);

```

In CSS, the element starts at `opacity: 0` and the `set` turns it on. Don't leave a CSS `transition` on anything the timeline animates.

## In After Effects

- Opacity only with hold keyframes (*Toggle Hold Keyframe*): 0 → 100 in one frame, no ramp.
- No *Cross Dissolve*, *Fast Blur* or *Gaussian Blur* animated on entrances and exits.
- Expo entrance curve: high speed leaving the first keyframe and ~90 % incoming influence on the last (or an expo preset in Flow). To leave, the other way round.
- One "camera" Null per shot, parent of the whole shot, with linear scale 100 → 104 % and a new start on every cut.

## What stays untouched

- Materials are not transitions: a glass with `backdrop-filter`, a shadow or a gradient stay.
- Depth by layers (what's behind, smaller or darker) stays. But if an element moves from one layer to another, it does so with a cut, not with a blur that opens up.
- Voice timings, word syncs and content. These rules change how something moves, not when it happens.
- If the text has a voice: the lines don't move. Each word appears in its final place, rising from below, when the voice says it. The rises overlap because they last longer than the gap between words.

## Check before showing it

1. Look for leftovers: `grep -nE "blur|autoAlpha|opacity:\s*0\.[0-9]|crossfade|dissolve"` in the scenes, and review any tween that touches `opacity` with a `duration`.
2. Pull frames at `t − 1F`, `t` and `t + 1F` of every entrance and every cut. At `t` the element has to be already **whole and opaque**. No frame may show something semi-transparent or two shots mixed.
3. Look at a contact sheet of the render (`ffmpeg -i v.mp4 -vf "fps=2,scale=384:-1,tile=6x6" sheet.png`) and check that no shot is still.
4. If letters rise one by one, follow one letter frame by frame (its centre in y). It has to come down in continuous values (311 → 307.7 → 304.2 → 301.8…), not in repeated steps (298.9 → 298.9 → 296.9 → 294.9 → 294.9 → 294.9).
