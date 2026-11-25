---
name: editorial-evidence-style
description: Editorial "evidence beat" style for vertical short-form explainers - put a real article, blog post, study or official announcement on screen as proof of a spoken claim (full-screen paper, numbered "Reason" label, the real page in a dark window, two-step zoom, blue ring + highlighter on the exact phrase, one big takeaway number above, source line under), cut to illustrative B-roll one cut per spoken item, then return to the proof. Use when the user wants research or articles shown as evidence in a short or Reel, asks for "article style", "evidence style", "Reason 1 / Reason 2 style" or "like the Seedance vs Omni video". Built on the short-editing engine (board `screen` item); load short-editing too.
---

# Editorial evidence style

A reusable method for showing **real sources as proof** of what the speaker says. It was extracted from the two
"Reason" beats of the creator's Reel *Google Omni vs Seedance* (1080x1920, 38.27 s). It is built entirely with the
`short-editing` engine. This skill says **what to build, where, and on which word**; `short-editing` holds the engine
itself (prepare, render, captions, mix). Load both.

How this file is organised:
- **Part A**: observed in the reference, with timestamps. Facts only.
- **Part B**: reusable principles derived from Part A.
- **Part C**: the engine vocabulary this style needs, with exact parameter meanings (verified in the engine source).
- **Part D**: step-by-step recipe, templates and checks.
- **Part E**: suggestions that are NOT in the reference. Present them to the creator as options, never as the style.

**Precedence note.** `short-editing`'s design notes prefer rebuilding press sources natively ("a press source rebuilt
natively, never a page screenshot", the `press` card scene). This style deliberately uses the **real page screenshot**
inside a `screen` window, which the creator approved for this kind of evidence beat. When this skill is in use, follow it
for evidence beats; everywhere else keep `short-editing`'s defaults. If a page is unreadable as a screenshot even after
zooming (see D2), fall back to the `press` card. The window is never a still image sitting there: it zooms, rings and
highlights on the spoken words, which keeps `short-editing`'s "every beat is an action" rule.

**Dependencies (the only ones):**
- The `short-editing` skill and its engine at `.claude/skills/short-editing/engine/short.mjs` (Node, Chrome, ffmpeg): spec
  format, `prepare`, `stills`, rendering, captions, mix. Every parameter used here was checked against its source (Part C).
- ffmpeg for the B-roll montage (D4); a browser for the screenshots (D2).
- Font paths in D4 are Windows paths (`C:/Windows/Fonts/...`); change them on another OS.
- The reference project paths in Part A are provenance only. Nothing in Parts B-D reads them.

---

## Part A - Observed in the reference

Reference: `video-agent/edits/short-tech-reel-v3cl/output/short-tech-reel-v3cl-p15-hq.mp4`. Editing timeline:
`video-agent/edits/short-tech-reel-v3cl/source/spec.js`. Screenshots and their boxes: `video-agent/edits/short-tech-reel-v3cl/assets/images/`
(`src-seed25-blog.png`, `src-tnw-head.png`, `src-tnw-10s.png`, `src-shots.json`). These files are provenance only: the
recipe below does not need them.

Times: "reel s" = time in the exported video (checked on the MP4 at 15 fps). The spec is authored in SOURCE seconds
(the raw take); silence cuts shift everything, see C1.

### A1. Reason 1 - ByteDance Seed blog (reel 7.87 -> 14.30)

Speech: "Here's the first reason. Seedance 2.0 takes text, audio, images, video, up to 50 references."

| Reel s | Observed | Spec (source s) |
|---|---|---|
| 7.87 -> 7.93 | Hard cut from the previous B-roll to the split layout (speaker below, title above). | broll `hard: true` |
| 7.93 -> 8.33 | The paper sheet rises over the speaker until it fills the frame (about 0.4 s). The speaker stays hidden for the whole beat. | `layout.keys [8.46, 1920]` |
| 8.13 | A blue, tilted "REASON 1" label pops in at the top. | `label` size 80, x 540, y 245, at 8.46 |
| 8.13 -> 8.27 | The article screenshot rises in, inside a dark rounded window (lower half), showing the top of the page at full width. The source line is already under it. | `screen` w 960, h 680, x 540, y 1110, at 8.50. Text "Source · ByteDance Seed blog · Seedance 2.5 · Jul 31, 2026", size 34, ink2, y 1478, at 8.50 |
| 8.28 | A grey subtitle says what the evidence measures: "Reference inputs per generation". | text size 44, ink2, y 372, at 8.88 |
| 8.28 -> 9.60 | On the spoken model name, the window springs from 1x to 1.45x onto the page headline. A blue ring draws around the headline and the rest of the page dims. | `viewKeys [[8.50,[1000,760,1]],[8.88,[840,520,1.45]]]`, `viewW 5`; `rings [{at:8.88, to:10.30, rect:[65,342,1550,202]}]` |
| 9.67 -> 12.50 | Full-screen B-roll on "takes text": four hard cuts, one per spoken word (text / audio / images / video), each cut on its word, a tap on each cut, an "Illustrative · Pexels" tag top right. | broll from 10.33 to 13.40, `hard: true`; `sfx.extra` taps at 11.30, 11.75, 12.55 |
| (under the B-roll) | While hidden, the window keeps zooming to 2.2x onto the sentence that proves the number. | `viewKeys [12.00,[1345,1921,2.2]]` |
| 12.47 -> 12.53 | Hard cut back to the paper, already framed on the proof phrase "up to 30 images, 10 video clips, and 10 audio clips". | broll ends |
| 12.54 -> 13.20 | On "up", a blue highlighter sweeps left to right across that phrase. | `marks [{at:13.44, rect:[973,1900,745,42], dur:.7}]` |
| 12.84 | On "50", a ring draws on the same phrase, and the takeaway lands above the window: "30 + 10 + 10 = up to **50** references" (50 in blue). | `rings {at:13.74, rect:[973,1900,745,42], pad:12}`; text size 52, y 480, `emph:'50'`, at 13.74 |
| 12.84 -> 14.30 | Hold until the next beat (split layout, new board). Captions keep running on their fixed line. | board `to: 15.60` |

### A2. Reason 2 - TNW article (reel 17.24 -> 19.90)

Speech: "Second, Omni flash caps maximum length that can be generated..."

| Reel s | Observed | Spec (source s) |
|---|---|---|
| 17.30 -> 17.70 | The previous split board fades out while the paper rises to full screen (about 0.4 s). | `layout.keys [20.00, 1920]` |
| 17.43 -> 17.56 | "REASON 2" label pops. The article headline screenshot rises in its window with "Source · TNW · May 20, 2026" under it. | `label` at 20.04; `screen` at 20.06 |
| 18.37 -> 18.57 | On "caps", the window cross-fades to a second crop of the same article (the paragraph with the fact) while springing from 1x to 2.2x onto the sentence. | `imgKeys [[20.06,'src-tnw-head.png'],[21.26,'src-tnw-10s.png']]`; `viewKeys [[20.06,[1000,760,1]],[21.26,[1300,1218,2.2]]]`, `viewW 10` |
| 18.57 -> 18.83 | Callout above the window: grey "Omni Flash at launch" and a large "10 s" counter (unit in blue). A ring and a highlighter land on "capped at 10 seconds at launch". | text size 44, ink2, y 360; `counter value:10, suffix:' s', size:150, y:480, countFrom 21.50, countTo 21.95`; `rings {at:21.50, rect:[1177,1199,475,38], pad:12}`; `marks {at:21.60, dur:.6}` |
| 19.77 -> 19.90 | On "that", a short cross-fade (about 0.13 s, not a hard cut) to full-screen B-roll: the creator's own screen recording of the product's Duration slider, with its own source line. | broll from 23.40, `offset 1.4`, no `hard` |

Observed timing defect: the ring, highlighter and counter in A2 were authored 0.1 to 0.45 s apart (source 21.50, 21.60,
21.95), but that stretch is a removed silence, so in the export all three land on the same frame (reel 18.57). Rule C1
prevents this.

### A3. Constants of both beats

- **Frame**: full paper, speaker hidden for the whole beat; the beat lasts 2.6 s (Reason 2, 17.30-19.90) to 6.4 s
  (Reason 1, 7.93-14.30).
- **Positions** (1080x1920 canvas, centre-anchored x/y): label y 245; subtitle y 360-372; takeaway y 480; window 960x680
  centred at (540, 1110), i.e. y 770-1450; source line y 1478.
- **Caption lane**: one fixed caption line at y 700 for the whole video; captions keep running over the evidence; no
  board item uses y 620-780.
- **Sources**: genuine page screenshots, page text untouched, cookie banners declined. Each source has a URL, a date and
  the CSS boxes of the phrases used, saved in `src-shots.json`. Screenshots are 2000x2800 px (viewport 1000x1400 CSS px at
  device pixel ratio 2).
- **Source line** `Source · <publication> · <title or product> · <Mon D, YYYY>` visible from the window's first frame.
- **Colour**: blue is the only accent (label, ring, highlighter, the key number or unit, the key caption word). Secondary
  text is grey (`ink2`).
- **One takeaway** per beat, above the window: an equation line or a counter.
- **Structure**: proof -> illustration -> proof (A1), or proof -> demonstration (A2).
- **Illustrative footage** is tagged "Illustrative · Pexels" (top right); the creator's own recordings carry
  "Screen recording · <site>" and the source line of the number they show.
- **Sound**, from the spec plus the engine's automatic cue rules (not isolated by ear; these cues were already in the spec
  of the approved P1 version, whose mix P15 reuses: the P1 track re-encoded, with extra effects layered only at reel
  26-29 s and a -1.5 dBFS limiter over the whole track): swish 0.12 when the paper takes the full
  screen (source 8.46, 20.04); swish 0.08 when the B-roll takes over (10.33); tap 0.12 on each B-roll cut (11.30, 11.75,
  12.55); swish 0.08 when returning from B-roll (13.40); swish 0.06 when the window changes crop (21.26); swish 0.12 into
  the demo B-roll (23.40). Automatic: a paper "land" when the `screen` enters, a pop on the `label`, a soft
  tick when each ring starts, and on the `counter` a rising run of 14 ticks from `countFrom` to `countTo` that ends with
  a thud and a bell on `countTo` (the engine default `sfx.tick: 'tick'`; the reference sets no `sfx.tick`; Reason 2's
  `countTo` 21.95 falls in a removed silence, so that end hit lands at reel 18.57 with the other cues). The highlighter
  (`marks` on a `screen`) is silent: the engine's marker sound only covers actor and tweet highlights, and the reference
  added none.

---

## Part B - Principles (derived from Part A)

1. **Claim -> proof on the word.** The beat starts on the claim word ("first reason", "second"). Each emphasis lands on
   the word it proves: the name -> headline ring; "caps" -> fact crop; "up to" -> highlighter; the number -> ring +
   takeaway.
2. **Give the proof the whole frame.** Full paper, speaker hidden, 2.5-7 s per beat.
3. **Fixed anatomy, fixed positions.** Label -> subtitle -> window -> source line, same coordinates every time, so the
   viewer learns where to look.
4. **Zoom in two steps.** 1x on the page head (credibility: the real site and its title), then a spring zoom to the
   proof (1.45x on a headline, 2-2.2x on a phrase). If the proof is far down the page, cross-fade to a second crop
   (`imgKeys`) instead of scrolling.
5. **Point, then read.** A ring points (and dims the rest); a 0.6-0.7 s highlighter reads along. Both on one short phrase,
   never a paragraph.
6. **Say it bigger than the page.** The screenshot is the proof; the line above it is the message (one number, blue).
7. **Illustrate, then return framed.** For a list, cut to full-screen B-roll, one hard cut per spoken item; return to the
   window already zoomed on the proof.
8. **Credit everything, fake nothing.** Real URL, real date, page untouched, source line on screen, illustrative footage
   tagged. No real source -> no evidence beat.

---

## Part C - Engine vocabulary for this style (verified in `short-editing/engine`: `lib/scenes-life.js`, `lib/runtime.js`, `lib/timeline.js`, `lib/cues.js`, `short.mjs`)

### C1. Time: source seconds vs reel seconds

Spec times are source seconds. `prepare` cuts silences and writes the kept ranges to `assets/timeline.json`. Pick cue
words from `assets/words-scribe.json` (source times), then check every cue time falls inside a KEPT range. A cue inside a
removed silence snaps to the next kept frame, and several cues collapse onto one frame (A2). To author in reel seconds,
put this helper at the top of the spec. `KEEPS` is exactly the `keeps` array of `assets/timeline.json`
(`[sourceStart, sourceEnd, reelStart]` per kept range) and `SPEED` is its `speed` field; copy both in, because the spec
is a plain script with no file access.

```js
const KEEPS = [/* [srcStart, srcEnd, reelStart], ... from assets/timeline.json "keeps" */];
const SPEED = 1;   // assets/timeline.json "speed" (short-editing's prepare defaults to 1.15; the reference used 1)
const T = r => { for (const [a, b, o] of KEEPS) if (r >= o - 1e-6 && r <= o + (b - a) / SPEED + 1e-6) return +(a + (r - o) * SPEED).toFixed(4);
                 for (const [a, , o] of KEEPS) if (r < o) return a; return KEEPS[KEEPS.length - 1][1]; };
// usage: at: T(12.84)   (reel 12.84 s -> source seconds)
```

How the engine maps the other way (`lib/timeline.js` `mapper`): source t inside a kept range -> `reelStart + (t - a) / speed`;
source t inside a removed silence -> the reelStart of the next kept range. `T` is its exact inverse. Audited 2026-11-25
against `mapper` on the reference timeline (23 keeps, speed 1): worst round-trip error 0.00003 s over 3826 samples. With
speed 1.1 a speed-unaware version was off by up to 0.39 s; this version stays exact. A reel time can never fall in a
removed silence, so `T` never produces a collapsed cue. Hand-typed source times can: the reference's 21.50 / 21.60 / 21.95
all map to reel 18.567.
`spec.pause`, `spec.inserts` and `spec.outro` add time that is not in `keeps`; `T` does not cover them (use the engine's
`'p+X'`, `'iK+X'`, `'o+X'` time strings there).

### C2. Layout

`layout: { keys: [[t, seam], ...] }`. `seam` = height in px of the paper from the top: `1920` = full paper (speaker
hidden), the project's split value (660 in the reference) = paper above the speaker, `0` = full camera. The engine
animates the change with a spring (`layout.w`, default 10; about 0.4 s observed). Put the `1920` key a few frames before the claim word and a key back when the
beat ends. The paper move itself is silent: add the swish in `sfx.extra`.

### C3. Board scene

`{ type: 'board', from, to, pushAt: [540, 800], push: 0, items: [...] }`. `push: 0` keeps the evidence still (no slow
push-in). Items are centre-anchored at (x, y) and enter with a short spring rise/fade at their `at`. They fade out at
`to` if given, or leave with the board. `cut: true` makes the entry and exit instant (not used in the evidence beats).

- `label` `{text, size: 80, x: 540, y: 245, at}`: the blue tag; the engine upper-cases it and slants it by default
  (`rot -.035`, `skew .2`, blue background, condensed heavy font).
- `text` `{text, size, color: 'ink2' | 'ink3' | 'blue', emph: 'word', x, y, at, to}`: `emph` turns that word blue.
- `counter` `{value, suffix: ' s', size: 150, x, y, at, countFrom, countTo}`: counts up to `value`; the suffix is blue.
- `screen` (the evidence window):
  - `img: 'assets/images/x.png'`, or `imgKeys: [[t, img], ...]` (content cross-fades to the next image at t).
  - `w, h` window size (960x680 in this style), `r` corner radius (default 30). Dark background, drop shadow.
  - `viewKeys: [[t, [cx, cy, zoom]], ...]`: (cx, cy) = the image pixel at the window centre; `zoom 1` = the image width
    fits the window width. Visible width in image px = `imageWidth / zoom` (2000 px at 1x, about 910 px at 2.2x). The
    centre is clamped so the view never leaves the image. Moves are springs; `viewW` = spring speed (default 7; 5 = calm,
    10 = snappy).
  - `rings: [{at, to, rect: [x, y, w, h], pad: 10, dim: true, dur: .5}]`: rect in image px. The outline draws on over
    `dur` (blue, 7 px) and the rest of the window dims to 50 %. It follows the zoom. Leaves at `to`.
  - `marks: [{at, rect | rects, dur: .7, color, pad: 8}]`: highlighter swept left to right over the rect(s) in image px.
  - `arrows` also exist (`{at, to, tip, dx, dy, bend, label}`); the reference does not use them.

### C4. B-roll

`broll: [{ media, from, to, offset: 0, zoom: .03, hard: true }]`, with `media: { name: { src, width: 1080, fill: true } }`.
B-roll covers the whole frame (captions stay on top). `hard: true` = hard cut in and out; without it = a short spring
cross-fade (about 0.13 s observed). If `hard` is omitted, the project-wide `spec.cuts === 'hard'` decides. `offset` =
seconds into the clip at `from`; `zoom` = slow push-in over the shot (default .05). Rerun `short.mjs prepare` after
adding or changing media: `prepare` extracts the media frames, and media it has not extracted does not render.
Learned in the reference project, not part of the two Reason beats: when two B-roll shots touch, give both
`hard: true`; with a soft edge the speaker showed through for about three frames at the join.

### C5. Sound

`sfx: { extra: [[t, 'swish' | 'tap' | 'pop' | 'thud' | 'slide' | 'bell', gain], ...], files: [{at | endAt, src, gain, peak}] }`.
Gains used here: swish .12 (paper takes the frame), .08 (back from B-roll), .06 (crop change); tap .12 (each B-roll
cut). The engine adds the automatic cues listed in A3 (`lib/cues.js`: `screen` -> land + one tick per ring; `label` ->
pop; `counter` -> tick run + end hit); do not double them with `extra`. Highlights on a `screen` make no sound by
themselves.

Counter sound (`lib/cues.js`, verified; measured in the 2026-11-25 validation render, where the end hit was the loudest
cue of the beat, at exactly `countTo`):
- Default, `sfx.tick` unset or `'tick'`: 14 ticks eased from `countFrom` to `countTo` (`countTo` defaults to
  `countFrom + .9`), then a thud (gain .24) on `countTo` and a bell .01 s after it.
- `sfx: { tick: 'mallet' }`: every tick run becomes a short marimba arpeggio (at most 4 notes, at least .22 s apart);
  the counter ends with a soft bell only, no thud.
- `sfx: { tick: 0 }`: no ticks, no thud, no bell. This is project-wide: it also silences the ring ticks and every other
  tick run in the video.
- To silence ONE counter only: `quiet: true` on that counter item. The engine then skips all its automatic cues
  (ticks, thud, bell); other items keep theirs.
- `sfx: { mute: true }` silences all synthesized cues (the `files` still play).

### C6. Captions

Keep the project's caption lane free (reference: `captions.yKeys: [[0, 700]]`, items avoid y 620-780). Put the number
words in `captions.emph` so they get the caption style's emphasis colour (`captions.hi`, else the theme tint; blue in the
reference). Fix misheard names with `captions.replace` (display only).

---

## Part D - Recipe

### D1. Choose the beat (inputs checklist)

From `assets/words-scribe.json` note the source times of:
- `CLAIM`: the word that opens the claim ("first reason", "second").
- `NAME`: the word naming the subject or source -> zoom 1 + headline ring + subtitle.
- `NUMBER_INTRO`: the lead-in to the number ("up to", "caps") -> highlighter. Zoom 2 or the crop change (`ZOOM2`) starts
  earlier, by the lead time in D3 "Zoom-2 lead".
- `NUMBER`: the number itself -> ring + takeaway.
- `NEXT`: where the speaker moves on -> B-roll or next beat.
- Optional `LIST`: the spoken items of a list -> one B-roll cut each.
Check each is inside a kept range (C1). Then confirm a real, dated source states exactly this number. If not, stop and
tell the creator.

### D2. Capture the source

1. Open the official page in a browser at a 1000 x 1400 CSS px viewport with device pixel ratio 2. Decline cookies, close
   pop-ups, change no text.
2. Save `src-<name>-head.png` (top of the page, y0 = 0). If the proof is below 1400 CSS px, scroll so it sits about a
   third down and save `src-<name>-fact.png`; note that scroll offset as y0.
3. Measure the CSS box of every phrase you will ring or highlight (in the browser console):

```js
const box = (txt) => { const r = document.createRange(), w = document.createTreeWalker(document.body, NodeFilter.SHOW_TEXT);
  for (let n; (n = w.nextNode());) { const i = n.data.indexOf(txt); if (i >= 0) { r.setStart(n, i); r.setEnd(n, i + txt.length);
    const b = r.getBoundingClientRect(); return [b.x + scrollX, b.y + scrollY, b.width, b.height]; } } };
box('up to 30 images, 10 video clips, and 10 audio clips');
```

   (This snippet is standard browser DOM code; it is not part of the engine and was not run during the 2026-11-25 audit.
   The reference boxes were measured the same way, with `getBoundingClientRect`.)
4. Write `assets/images/src-shots.json`: `{ "<name>": { "url", "date", "dpr": 2, "shots": { "head": [0, 0], "fact": [0, y0] },
   "boxes": { "<phrase>": [x, y, w, h] } } }`.
5. Convert to image px for the shot you show: `rect = [x*2, (y - y0)*2, w*2, h*2]`. Widen it to include the spoken
   lead-in word if it is on the same line.
6. **Readability rule (tested):** keep each proof rect to one line, about 500-750 image px wide. At 2-2.2x the window
   shows only about 910-1000 image px, and body text at 1x is unreadable on a phone. If the sentence is longer, ring
   and highlight only the phrase that carries the number. If no box was saved, measure the phrase on a grid crop:
   `ffmpeg -i shot.png -vf "crop=1000:160:0:Y,drawgrid=w=100:h=20:color=red@0.4" grid.png`.
7. **Keep the ring inside the window (ring padding).** Set the zoom-2 view centre to the centre of the proof rect:
   `cx = x + w/2`, `cy = y + h/2`. The engine clamps the centre so the view never leaves the image, so a phrase at the
   page's left margin automatically gets the leftmost view; do not push the centre further right "to show more page".
   The ring (rect + `pad` on each side + 7 px stroke) fits in the window when
   `rectW * k + 2 * pad + 7 <= windowW`, with `k = windowW / imageW * zoom`. So the highest safe zoom is
   `zoom <= (windowW - 2 * pad - 7) * imageW / (windowW * rectW)`; for this style (image 2000 px, window 960, pad 12)
   that is about `1937 / rectW`: a 500 px phrase fits up to 3.9x, the reference's 745 px up to 2.6x, a 1000-1100 px
   phrase only at about 1.8x (1.94x at 1000 px, 1.76x at 1100 px). Use about 1.8x for such a long phrase, or shorten the
   phrase (step 6). At the left margin the clamped view still keeps the ring inside when `x >= (pad + 3.5) / k` image px
   (about 16 px at 2x), which page text always satisfies.
   Verified 2026-11-25 (scratch copy): rect [62, 1616, 495, 46] at 2x. Centre 560 (off the rect centre) put the ring's
   left edge 13.6 px outside the window; centre 310 (rect centre, clamped to 500) left 44 px of margin, ring fully visible.

### D3. Build (template, source seconds)

```js
layout: { keys: [ /* ... */ [CLAIM - .04, 1920], [NEXT, SPLIT] /* ... */ ] },   // SPLIT: see Placeholders

{ type: 'board', from: CLAIM, to: NEXT, pushAt: [540, 800], push: 0, items: [
  { k: 'label', text: 'Reason 1', size: 80, x: 540, y: 245, at: CLAIM },
  { k: 'text', text: 'What the evidence measures', size: 44, color: 'ink2', x: 540, y: 372, at: NAME,
    to: NUMBER - .3 },                                       // ONLY if the counter's grey line (below) is used; see "Slot rule"
  { k: 'screen', img: 'assets/images/src-<name>-head.png',   // or imgKeys: [[CLAIM + .04, head], [ZOOM2, fact]] (D2.2 files)
    w: 960, h: 680, x: 540, y: 1110, at: CLAIM + .04,
    viewKeys: [[CLAIM + .04, [1000, 760, 1]],                // page head, full width
               [NAME, [HEAD_CX, HEAD_CY, 1.45]],             // headline
               [ZOOM2, [FACT_CX, FACT_CY, 2.2]]],            // proof phrase: rect centre (D2.7); ZOOM2 = lead time before NUMBER_INTRO
    viewW: 5,                                                // 10 = snappy (crop change); sets the lead time, see "Zoom-2 lead"
    rings: [{ at: NAME, to: ZOOM2, rect: HEADLINE_RECT },    // `to` = when the view leaves the headline
            { at: NUMBER, rect: FACT_RECT, pad: 12 }],
    marks: [{ at: NUMBER_INTRO, rect: FACT_RECT, dur: .7 }] },
  { k: 'text', text: 'Source · Publication · Title · Mon D, YYYY', size: 34, color: 'ink2', x: 540, y: 1478, at: CLAIM + .04 },
  { k: 'text', text: 'a + b + c = up to N things', emph: 'N', size: 52, x: 540, y: 480, at: NUMBER },
  // or: { k: 'counter', value: N, suffix: ' s', size: 150, x: 540, y: 480, at: NUMBER, countFrom: NUMBER, countTo: NUMBER + .45 },
  //     with a grey line above it: { k: 'text', text: 'Subject at launch', size: 44, color: 'ink2', x: 540, y: 360, at: NUMBER },
] },

sfx: { extra: [ [CLAIM, 'swish', .12], /* only on a crop change (imgKeys): */ [ZOOM2, 'swish', .06] ] },
```

**Placeholders** (replace every one; times are source seconds, written as `T(reel)` when authoring in reel time, C1):
- `CLAIM`, `NAME`, `NUMBER_INTRO`, `NUMBER`, `NEXT`: the cue words from D1.
- `ZOOM2`: when the second zoom (or crop change) starts, `NUMBER_INTRO` minus the "Zoom-2 lead" below.
- `SPLIT`: the seam value this project uses for its split layout (C2): copy it from the project's other `layout.keys`
  entries; use `0` instead to return to full camera. Never assume a number from another project.
- `HEADLINE_RECT`, `FACT_RECT`: `[x, y, w, h]` in image px of the headline and of the proof phrase (D2.5).
- `HEAD_CX, HEAD_CY` / `FACT_CX, FACT_CY`: the centres of those rects, `x + w/2`, `y + h/2` (D2.7).
- `<name>`: the source's file name from D2.2; `N`: the number being proved.
- The y positions (245, 360-372, 480, window 770-1450, 1478) assume a caption lane around y 700 (C6). With another lane,
  keep the order and move the items so none crosses it.

**Slot rule (subtitle vs counter line).** The subtitle (y 372) and the counter's grey line (y 360) occupy the same slot
above the takeaway; shown together they overprint (seen in the 2026-11-25 validation). Use one of two forms:
- **Single element (simplest):** keep the subtitle and drop the grey line (the reference's Reason 1), or drop the
  subtitle and use only the grey line (the reference's Reason 2).
- **Hand-over:** give the subtitle `to: NUMBER - .3` and start the grey line at `NUMBER`. Items fade out over .26 s
  after `to` (`leave` in `lib/kit.js`), so the subtitle is fully gone at `NUMBER - .04`, before the line starts rising
  in; never let their visible times overlap.

**Zoom-2 lead.** A `viewKeys` move is a critically damped spring, `1 - (1 + w*t) * exp(-w*t)` with `w = viewW`
(`lib/kit.js`). Time to reach the target: `viewW 5` 0.78 s (90 %), 1.17 s (98 %); `viewW 7` (default) 0.56 / 0.83 s;
`viewW 10` 0.39 / 0.58 s. Put `ZOOM2` at least that far before `NUMBER_INTRO` (the highlighter), in reel seconds after
silence cuts: `viewW 5` -> lead >= 0.8 s, ideally 1.2 s; `viewW 10` -> lead >= 0.4 s, ideally 0.6 s. A per-key spring is
also possible: `[t, [cx, cy, zoom], 10]` (the third element overrides `viewW` for that move). If the speech gives less
time than the lead, start the zoom on an earlier word, or do it while B-roll covers the window.
Reference behaviour (observed, Part A): Reason 1 zoomed under the B-roll (zoom-2 key at reel 11.10, highlight 12.54:
1.44 s lead); Reason 2 changed crop with `viewW 10` 0.18 s before its highlight, so its highlight landed during the
settle. Validation 2026-11-25: zoom-2 on the same word as the highlight (`viewW 5`) landed the highlight and ring while
the view was still moving; moving the key 0.9 s earlier fixed it.

### D4. Illustrative B-roll in the middle (optional, for a spoken list)

One clip per spoken item, each cut lasting from its word to the next word, cover-cropped to 1080x1920, tagged, nothing in
the caption lane, concatenated into one file:

```bash
TAG="drawtext=fontfile='C\:/Windows/Fonts/seguisb.ttf':text='Illustrative · Pexels':fontsize=30:fontcolor=white@0.85:box=1:boxcolor=black@0.45:boxborderw=10:x=w-tw-44:y=236"
fit="scale=1080:1920:force_original_aspect_ratio=increase,crop=1080:1920,fps=30,setsar=1,format=yuv420p"
ffmpeg -y -ss IN_1 -t LEN_1 -i item1.mp4 -ss IN_2 -t LEN_2 -i item2.mp4 -ss IN_3 -t LEN_3 -i item3.mp4 -filter_complex \
 "[0:v]$fit[a];[1:v]$fit[b];[2:v]$fit[c];[a][b][c]concat=n=3:v=1[v0];[v0]$TAG[v]" -map "[v]" -an -c:v libx264 -crf 17 -r 30 assets/media/broll-list.mp4
```

(Use a font path valid on the machine.) `IN_n` = where to start inside clip n; `LEN_n` = the reel gap from spoken item n
to item n+1 (the last one: to the return word). Spec: `broll` from the
first item word to the return word, `hard: true`; `sfx.extra` tap .12 on each internal cut; swish .08 on the return. Move
the window's last `viewKeys` step to a time while the B-roll covers it, so the return lands framed on the proof. Only use
footage with a licence that allows reuse (the reference used Pexels); a watermarked preview is not usable.

### D5. Exit

Either a layout key back to the split or full camera, or a demo B-roll (the creator's own recording) with a short
cross-fade (no `hard`) and its own source line baked in.

### D6. Check before any render

1. `node .claude/skills/short-editing/engine/short.mjs prepare <project>` (after any media change).
2. `node .claude/skills/short-editing/engine/short.mjs stills <project> <reel times...> ` at: claim, name zoom, B-roll
   in and out, highlight, number, exit. `stills` takes reel (short) seconds; add an `s` suffix for source seconds.
3. Read every on-screen fact against the page (number, unit, date). The source line names the publication and the date.
4. The proof phrase is readable at phone size; the ring and the highlight sit exactly on it.
5. No item in the caption lane; no cue inside a removed silence; no speaker frames between two B-roll shots.
6. Show the stills to the creator before rendering. Render under a new file name; never overwrite an approved export.

---

## Part E - Suggestions (NOT in the reference; offer them as options)

- When a claim has two dated sources (e.g. "at launch" vs "today"), show both windows in sequence with their dates.
- For a page in another language, keep the original screenshot and add a one-line translation labelled as such.
- For PDFs or studies, crop to the figure or table, ring the single value, put the study name and year in the source line.
- If a beat runs longer than about 7 s, split it into two evidence beats rather than holding one window.

---

## Test log

- 2026-11-25, on a scratch copy of the reference project (no project file changed): "but Seedance 2.5 gives you up to
  30 seconds" (source 25.52-29.80) built with this recipe on the real ByteDance Seed page. Label "Seedance 2.5", subtitle
  "Max clip length per generation", headline ring on "Seedance" (25.83), highlighter on "up" (28.16), ring and "30 s"
  counter on "30" (28.35), source line from the first frame. Stills checked at every cue; caption lane clear; fact matches
  the page ("Up to 30 seconds per generation"). Found: a full-width 1840 px rect at 1.05x was unreadable; the 495 px
  phrase at 2.0x read cleanly. That result is now rule D2.6.
- 2026-11-25, documentation audit (no video edited or rendered): every Part C parameter checked against
  `short-editing/engine` (`lib/scenes-life.js` board items and `screen`, `lib/runtime.js` layout and B-roll,
  `lib/timeline.js` mapper, `lib/cues.js` automatic sounds, `short.mjs` stills); the time helper made speed-aware and
  round-trip tested; Part A timings re-checked against the export frames and the spec; the sound cues confirmed present in
  the approved P1 spec.
- 2026-11-25, validation render (scratch copy only): one 5 s beat built with this recipe, rendered with audio; cues
  measured by mixing with and without SFX. Four documentation fixes followed: D3 slot rule (subtitle vs counter line),
  D3 zoom-2 lead times, counter end sound in A3/C5, D2.7 ring fit and view centre.

## Known limitations

- Part A's sound cues come from the reference spec and the engine's cue code; they were not isolated in the reference
  export (the 2026-11-25 validation measured the same cue types in a scratch render).
- The D2 box-measuring snippet and the capture steps are documented, not executed during the audit.
- One disposable beat was rendered with audio in a scratch copy (2026-11-25); its sound cues were measured from the mix,
  not listened to. Live skill routing (which prompts trigger it) was checked against the description only.
- `T` ignores `spec.pause`, `spec.inserts` and `spec.outro` time (see C1).
- Positions (label y 245, window y 770-1450, caption lane y 620-780) assume a 1080x1920 frame and a caption lane at
  y 700. For another lane, keep the same order and move the takeaway/window so nothing crosses the lane.
