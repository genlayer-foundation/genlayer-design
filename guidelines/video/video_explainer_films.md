# Video: explainer films

How the GenLayer Fundamentals explainers are made. A film is a list of shots. Each shot is a function of time that returns SVG, timed from the words of the voiceover. The same code drives the live preview and the frame-accurate render.

See `video_illustration.md` for what to draw and `video_text_motion.md` for moving text. Launch videos follow `video_launch_motion.md` instead.

## The process

1. **Concept.** Watch the finished films, then propose three hero-object concepts as key frames. Stop until one is picked.
2. **Script.** Do a rhythm pass, but keep the approved wording. On-screen lines and spoken lines follow the script.
3. **Voice.** Write a performance note, and get approval before spending credits.
4. **Board.** Build frames with notes per shot, then a review page and an animatic with voice, music and effects.
5. **Notes loop.** Reviews come as timestamped comments at 30 fps. Map each note to a shot, fix it, check stills around the timecode, render the new version and commit it.
6. **When unsure,** render a short test clip with the options labelled.

Don't render a full film unasked. Check with stills first.

## How a film is built

- **Picture:** 1920×1080 at 30 fps. Headless Chrome draws each frame as SVG, and ffmpeg encodes them.
- **Timing:** every shot takes its times from word onsets, so `w('L02', 1)` means word 1 of line 2.
- **Word onsets:** faster-whisper (`word_timestamps=True`), aligned to the script, with phrase edges from ffmpeg `silencedetect` at -40 dB.
- **Text:** drawn as glyph outlines, never SVG `<text>`.
- **Sound:** the drawing code registers an event as it draws (`cue('kind', t)`), and the mix turns events into sounds.

## Motion

- **Cuts are match cuts.** One ease-in-out speed ramp spans the cut, with peak speed on the cut. The outgoing shot plays the first half and the incoming shot the second. The move is tiny, 25 px or 3 % each side, so it feels like a continuation, not a transition.
- **Storytelling transitions** are welcome: the last coin keeps going, the frame pans with it, and a match cut lands it next to the next line.
- **Never:** opacity fades in transitions, full-frame slides from the edge, big zooms or double movements.
- **Everything eases in and out,** on `cubic-bezier(.45, 0, .15, 1)`. No overshoot, snappy expo or long tails.
- **Every shot drifts:** a slow push of about 1 % per second.
- **Things appear in place.** Static things don't drift within the shot. A pill's first and only move is its snap into place.
- **One at a time.** Elements appear and leave at their own moments, never all together.
- **The same object never hard-cuts between states.** Grow or morph it with a long move and a slow landing. Moving elements keep their size.
- **Streams never stop.** No dead spots.
- **Words stay long enough to read.** A word naming the concept is on screen from its first syllable and held until read. Delay the next shot rather than cut it short.

## The voice

- **Default:** Algieba, on Google Gemini 2.5 Pro TTS.
- **Direction:** warm and optimistic, and brisk. Speed can't be set, so ask for a brisk read and re-record to go faster.
- **Takes:** record one full take, and one per phrase if needed.
- **Patching lines:** record the new lines as one take and splice them at the silences. Use plain lines: `[pause]` tags can make the model stop after one sentence and output minutes of silence.
- **Ending:** usually "This is GenLayer." or the concept's name, then the end card. Decide whether the tagline is spoken.

## Sound

Animatics always have full sound.

- **Every word** gets one short beep as it appears, from a set of four used in rotation.
- **Every kind of motion** has its own sound: pop, data, fill, lock, typing, outline, agreement, logo, and a short whoosh per cut.
- **Levels:** effects stem about -36 LUFS, music about -7 dB and ducked under the voice, mix -14 LUFS and -1 dBTP.
- **Deliver** the effects as a separate stem with a cue sheet, so they can be edited.
- **Music:** a new bed for each film.

## End card

The white wordmark on `#0A0A0A` (806×192 on a 1920×1080 frame) rises slowly. The tagline decodes letter by letter below it: "THE FIRST ADJUDICATION LAYER / FOR THE AGENTIC ECONOMY".

## Check before showing

1. Render stills at each note's timecode and look at them.
2. Check that the facts follow "Say it correctly" in `video_illustration.md`.
3. Check that every on-screen word appears as the voice says it, with no final punctuation.
