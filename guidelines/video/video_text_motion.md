# Video: animating text by letter or word

How to animate text piece by piece without it vibrating or losing its kerning. Splitting text into pieces is what breaks it: the browser re-shapes each piece and snaps it to the pixel grid on its own, so kerning goes and letters drift out of step. Both methods here keep the whole line's layout from the first frame to the last.

## Pick by where the text runs

| The text runs | Use |
| --- | --- |
| Live on a web page, real HTML | [Full copies of the text](#live-web-full-copies-of-the-text) |
| In a video rendered frame by frame (SVG per frame, or anything that seeks and screenshots) | [Glyph outlines](#rendered-video-glyph-outlines) |

Don't swap them. In a frame-by-frame render, HTML text with no layer of its own rises in whole-pixel steps. On a live page, outlines aren't real text.

## Never do this

Each of these was measured at 80 px, against the intact line, in Chrome.

| Method | What goes wrong |
| --- | --- |
| `display: inline-block` per letter | All kerning across letters is lost: the last letter of a test line ended 26.7 px off |
| `<text>` per letter, then a whole `<text>` when it settles | Letters jump up to 2.6 px sideways at the swap |
| A letter's x taken from the width of the text before it | Misses the kerning pair between that text and the letter |
| A separate copy per letter at its measured x, with `will-change` | Each copy snaps on its own: letters up to about 0.5 px off |
| `will-change` on text you later swap back | At a fractional position the layer shifts it, and it jumps at the swap |
| Moving letters with `top`, `margin` or `position: relative` | Whole-pixel steps, out of step between letters |

By word, `inline-block` costs less (under 0.3 px at the spaces) but still isn't exact, so use the same two methods.

## Live web: full copies of the text

Nothing is split. Each piece gets a full copy of the text, in the same box, with everything but that piece transparent. The browser shapes each copy exactly like the original.

1. Wait for the font: `await document.fonts.ready`.
2. Leave the original text in place and make it transparent. It holds the layout, and stays selectable and readable by screen readers.
3. For each letter or word, add an `aria-hidden` copy of the contents with `position: absolute; inset: 0; color: transparent`, and wrap that one piece in a span with its real colour.
4. Animate each copy's `transform` and `opacity`. Don't add `will-change`.
5. When the last piece lands, remove the copies and restore the original's colour.

```js
copy.animate(
  [{ transform: 'translateY(.42em)' }, { transform: 'none' }],
  { duration: 600, delay, easing: 'cubic-bezier(.45, 0, .15, 1)', fill: 'both' }
);
```

Wrapped lines, centred text and coloured spans all work. Call it on a block element, once per heading, not per span. A new reveal on the same element must restore the text first, or the copies read a transparent colour. Respect `prefers-reduced-motion`.

Measured over 48 combinations of size, density and layout, the text after finishing matched the intact line exactly.

## Rendered video: glyph outlines

Paths can't snap, and every letter keeps one x from first frame to last, so nothing shifts when a letter or word settles.

1. Parse a static font with opentype.js. Instance a variable font first, because opentype.js only reads its default master: `fonttools varLib.instancer Lineca.ttf wght=500`.
2. Lay out the whole line once, with the font's kerning:

   ```js
   const sc = fs / font.unitsPerEm, xs = [];
   let x = 0, prev = null;
   for (const ch of text) {
     const g = font.charToGlyph(ch);
     if (prev) x += font.getKerningValue(prev.index, g.index) * sc;
     xs.push(x);
     x += g.advanceWidth * sc + track * fs;
     prev = g;
   }
   ```

3. Draw each letter as a path at its `xs[i]`. A moving letter gets `translate(0, y)` only. Settled letters share one path.

Measured against Chrome's own shaping, positions matched within 0.1 px, and letters moved 0.09 px sideways at most while rising.

## Motion to pair it with

The defaults are the explainer films' reveal: 0.6 s on `cubic-bezier(.45, 0, .15, 1)`, a 0.42 em rise, letters 18 ms apart, and a short fade over the first 40 %. Launch videos use no fade: see `video_launch_motion.md`.

## Limits

- On Retina, Chrome moves live HTML text one physical pixel at a time (0.5 CSS px), whatever the method. Only outlines move continuously there.
- When the original text returns, a few letters can change slightly: median 1 % of the ink, worst 9.9 % (32 px at 2x), never more than 25 % of a pixel's intensity.
- Measured in Chrome on macOS only. Safari, Firefox and Windows are untested.

## Check your work

1. Render the settled line and the intact line, and diff them.
2. Follow one letter frame by frame while it rises: x must not change, and y must come down in continuous values.
