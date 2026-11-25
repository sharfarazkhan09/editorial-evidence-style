# editorial-evidence-style

**Version 1.0.0** · A Claude Code skill for short-form video editing

Show real articles, blog posts and official announcements on screen as **proof** of what a speaker says, in vertical short videos (Reels, Shorts, TikTok). The skill teaches Claude Code exactly how to build these "evidence beats": which words to time them on, where to place each element, and how to check the result before rendering.

---

## What it does

When the speaker makes a claim ("Seedance gives you up to 30 seconds"), the skill has Claude build a short sequence that proves it with the real source:

1. A paper sheet covers the screen and a numbered label pops in ("REASON 1").
2. The real web page appears inside a dark window, with a source line underneath (publication and date).
3. The window zooms onto the page headline, then onto the exact phrase that contains the number.
4. A blue ring and a highlighter land on that phrase as the speaker says it.
5. The key number appears large above the window ("30 s", or "30 + 10 + 10 = up to 50").
6. Optionally, quick B-roll cuts illustrate a spoken list, then the edit returns to the proof.

<a id="layout"></a>

```
 ┌───────────── 1080 x 1920 ─────────────┐
 │            [ REASON 1 ]   label        │
 │     what the evidence measures (grey)  │
 │              30 s         big number    │
 │ - - - - - - caption line - - - - - - - │
 │  ┌──────────────────────────────────┐  │
 │  │ real page screenshot, zoomed,    │  │
 │  │ ringed + highlighted phrase      │  │
 │  └──────────────────────────────────┘  │
 │   Source · Publication · Mon D, YYYY   │
 └────────────────────────────────────────┘
```

## Main features

- **Observed, not invented.** The style was extracted frame by frame from a finished, approved video. The skill keeps what that video actually does (with timestamps) separate from general rules and from optional suggestions.
- **Word-accurate timing.** Includes a tested helper that converts "time in the final video" to "time in the raw take" after silences are cut, so effects never land on a removed pause.
- **Exact engine settings.** Every zoom, ring, highlight, counter, label, B-roll and sound setting is documented with its default value, checked against the rendering engine's source code.
- **Readability rules.** How wide a highlighted phrase may be, how far to zoom, and where to centre the view so the ring is never cut off.
- **Sound cues documented.** Which effects play automatically (and how to silence them) and which ones you add.
- **Honesty rules.** Real pages only, real dates, a source line on screen, illustrative stock footage labelled as such.

## Requirements

This skill is a **recipe**. It does not render video by itself. You need:

| Requirement | Why |
|---|---|
| [Claude Code](https://docs.claude.com/en/docs/claude-code/overview) | Runs the skill. |
| The **`short-editing`** skill, installed in the same project | Its rendering engine (`short.mjs`) builds the video. **Not included here**; get it from wherever you obtained it, and check its own license. |
| Node.js, Google Chrome and ffmpeg | Required by the `short-editing` engine. |
| A web browser | To capture page screenshots of your sources. |

The ffmpeg example in the skill uses a Windows font path (`C:/Windows/Fonts/...`). On macOS or Linux, change it to a font that exists on your machine.

## Installation

1. Make sure the `short-editing` skill is installed (see Requirements).
2. Copy this whole folder, `editorial-evidence-style/`, into **one** of these places:
   - **For one project:** `<your-project>/.claude/skills/editorial-evidence-style/`
   - **For all your projects:** `~/.claude/skills/editorial-evidence-style/`
     (on Windows: `C:\Users\<you>\.claude\skills\editorial-evidence-style\`)
3. The folder must contain `SKILL.md` directly inside it:

```
.claude/
└── skills/
    ├── short-editing/              (installed separately)
    └── editorial-evidence-style/
        ├── SKILL.md
        └── README.md
```

4. Start (or restart) Claude Code in your project. The skill appears in Claude Code's list of available skills.

## How to use it

**Automatic:** describe what you want, and Claude Code picks the skill from its description. For example:

- "Show the TechCrunch article as proof when I say the model costs $10."
- "Make this claim an evidence beat with the official blog post, like a Reason 1 / Reason 2 style."
- "Highlight the number in the study when I mention it."

**Manual:** type `/editorial-evidence-style` in Claude Code, followed by your request.

**What to give Claude:**

- A project already prepared with `short-editing` (your recording transcribed and cut).
- The claim you want to prove, and the URL of a **real, dated** source that states it.

**What Claude will do, following the skill:**

1. Find the exact words in your transcript (claim, name, number) and check they were not cut out.
2. Capture the source page and measure the phrase to highlight.
3. Write the evidence beat into your project's `spec.js`.
4. Render still frames at each key moment and show them to you **before** rendering the video.

If no real source states the number, the skill tells Claude to stop and ask you, instead of faking one.

## Testing status

Tested on Windows, on one real project, in a separate scratch copy (the original project was never modified):

| Test | Result |
|---|---|
| Skill discovered and loaded by Claude Code | Pass |
| Every documented setting checked against the engine source code | Pass |
| Time-conversion helper checked against the engine (3,826 samples, max error 0.00003 s) | Pass |
| One evidence beat built from the recipe and checked in still frames | Pass |
| One 5-second beat rendered with audio; sound cues measured | Pass |
| Defects found during testing (overlapping text, zoom timing, counter sound, ring clipping) | Fixed in v1.0.0 |

## Known limitations

- Requires the separate `short-editing` skill and its engine; it does nothing on its own.
- Tested on one project and on Windows only.
- Sound cues were measured from the audio data, not listened to by a person.
- The browser snippet for measuring phrase positions is documented but was not run during testing.
- Which requests trigger the skill automatically was checked against its description, not by live testing.
- The time helper does not cover inserted pauses, inserts or an outro (see section C1 in `SKILL.md`).
- Default positions assume a 1080 x 1920 video with the caption line around y 700. Other layouts need the positions moved (explained in `SKILL.md`).
- `SKILL.md` names the files of the original project it was extracted from (section A). Those files are **not included**; they are listed only to show where each observation came from. The recipe does not need them.

## Files

| File | What it is |
|---|---|
| `SKILL.md` | The skill itself: observations (Part A), principles (B), engine settings (C), step-by-step recipe (D), optional suggestions (E), test log and limitations. |
| `README.md` | This guide. |

## Version history

- **1.0.0**: first public version. Audited and validated; four documentation fixes from the validation are included.

## License

Not chosen yet. See the repository's `LICENSE` file once it is added.