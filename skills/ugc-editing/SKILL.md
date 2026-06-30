---
name: ugc-editing
description: Covers assembling raw footage, trimming bad takes and silences, choosing and applying a talking-head/b-roll format (straight intercut, stacked split, or masked/floating overlay), and caption placement for UGC-style content — agnostic to whether the clips are AI-generated or real filmed footage. Use whenever the user wants to edit, cut, assemble, format, or caption UGC-style video, or pick a talking-head + b-roll layout. Picks up after ugc-photo-prompts/ugc-video-prompts if the footage is AI-generated, or after real footage is imported via import_media.
---

# UGC Editing

## Core principle

Editing is footage-source-agnostic. The same assemble → trim → layout → caption pipeline applies whether the clips came from `ugc-video-prompts` or a real phone shoot. This skill picks up wherever footage already exists and turns it into a finished vertical post. The single biggest editing mistake in UGC is padding — long pauses, repeated stumbles, slow setups. Cut harder than feels comfortable. Authentically raw ≠ unedited; it means tight pacing with natural-sounding joins.

---

## Step 0: Pre-flight — always run before touching the timeline

1. **`get_timeline`** — check `width`/`height`. Must be 1080×1920 (9:16). If not:
   ```
   set_project_settings({ aspectRatio: "9:16", quality: "1080p" })
   ```
   Do this **before** placing any clips. The project snaps to the first clip's aspect ratio on placement — if you place a 16:9 clip first and then fix it, all transforms recalculate and you lose your layout. Set 9:16 first, always.

2. **`get_media`** — identify A-roll clip(s) and available b-roll. Note durations and sourceWidth/sourceHeight for each.

3. **`inspect_media`** on the A-roll:
   - First pass: `overview: true` — one storyboard, read the segment timestamps.
   - Second pass: `wordTimestamps: true`, full duration — read the entire word list as prose before cutting anything. You're looking for: retakes (same sentence said twice), filler words ("um", "like", "you know", "and I..."), false starts, trailing dead air after the last real word, and sections that are off-topic or weak.

---

## Step 1: Place A-roll only — no b-roll yet

```
add_clips([{
  mediaRef: AROLL_ID,
  startFrame: 0,
  durationFrames: FULL_DURATION_IN_PROJECT_FRAMES,
  trimStartFrame: TRIM_IF_NEEDED   // skip pre-roll silence
}])
// NO trackIndex — auto-creates V1 + A1
```

- **Never specify `trackIndex` here.** Auto-creation puts A-roll on track 0 (V1). B-roll will go above it later on a new auto-created track.
- If the source is 16:9, the project will snap back to 16:9. Reset immediately:
  ```
  set_project_settings({ aspectRatio: "9:16", quality: "1080p" })
  ```
- `durationFrames`: source duration in seconds × project fps. E.g. 69s at 60fps = 4140 frames. With `trimStartFrame: 780` (13s × 60fps), duration = (69 − 13) × 60 = 3360 frames.

---

## Step 2: Cut bad takes and filler — always words, never frames

**Read first, cut once.** Call `get_transcript`, read every word, mark everything to cut, then fire a single `remove_words` call with all indices at once. Do not make multiple sequential `remove_words` calls — indices shift after each cut.

```
get_transcript()
// read entire word list as prose, identify all cuts
remove_words({
  words: [
    [FIRST_RETAKE_START, FIRST_RETAKE_END],   // full duplicate take
    FILLER_INDEX,                              // single filler word e.g. "Um"
    [FALSE_START_START, FALSE_START_END],      // e.g. "and I..."
    [RETAKE_2_START, RETAKE_2_END],            // second retake of same section
    [TRAILING_FILLER_START, TRAILING_FILLER_END], // e.g. "and yeah, that's basically it"
  ],
  cutAggressiveness: "tight"   // "balanced" if joins sound clipped
})
```

**What to cut:**
- Every retake except the best one. Keep the take that's most fluent and energetic — usually the second or third attempt, rarely the first.
- Filler words: "um", "uh", "like", "you know", "so", "anyway", "and I...", "I mean".
- False starts: anything where the speaker restarts mid-sentence.
- Trailing dead air: words at the end that don't add value ("and yeah", "so... yeah", "and that's it I guess").
- Off-topic tangents: sections that break the hook → value → CTA arc.

**What to keep:**
- Natural pauses between genuine thoughts — don't over-tighten to the point it sounds robotic.
- The best single take of each section.
- The outro/CTA even if it feels soft — viewers who made it to the end convert.

**Caption track blocks ripple:** If captions already exist on the timeline, `remove_words` will refuse with a sync-lock error. Always `remove_tracks` on the caption track first, cut, then re-add captions. Never add captions before cutting is finished.

---

## Step 3: Pick a layout format

Read what the b-roll is actually doing before choosing:

**Format 0 — Straight intercut (default)**
Use when: b-roll is supplementary, not simultaneous. The viewer can handle switching fully between the two.
- Just hard cuts alternating A-roll and b-roll on the same track.
- Each clip fills the full 9:16 frame — no compositing needed.
- Cut every 3–5s to maintain pace.
- Use `add_clips` or `insert_clips` on track 0, no layout tool needed.

**Format 1 — Stacked split, b-roll top / talking head bottom**
Use when: b-roll is proof — it's demonstrating the product, result, or location being described. The viewer needs to see both simultaneously to connect the narration to the visual.
- B-roll on top half, talking head on bottom half.
- This is the proven format for product demos, location callouts, and "look at this" UGC.

**Format 2 — Stacked split, talking head top / b-roll bottom**
Use when: b-roll is filler — satisfying visuals, gameplay, or ambient footage that holds attention but isn't load-bearing. The narration is the content; the secondary visual is just retention.
- Talking head on top, filler on bottom.
- Don't default to Format 1 ordering here — it's a different job.

**Format 3 — Full-bleed b-roll, floating talking head**
Use when: the b-roll visual is the hero (a landscape, a product in action, a demo) and the person is commentary. Not yet supported cleanly via `apply_layout` — requires `apply_effect` (chroma key) + manual `set_clip_properties`. Skip unless the user specifically asks for it.

---

## Step 4: Add b-roll to a new track — critical placement rules

```
add_clips([
  { mediaRef: BROLL_1, startFrame: 0,    durationFrames: SEGMENT_1_FRAMES },
  { mediaRef: BROLL_2, startFrame: S1,   durationFrames: SEGMENT_2_FRAMES },
  { mediaRef: BROLL_1, startFrame: S2,   durationFrames: SEGMENT_3_FRAMES },
  // tile to cover totalFrames exactly
])
// NO trackIndex on ANY entry
```

**Critical rules — these caused every redo:**

1. **Never specify `trackIndex`** on b-roll `add_clips` entries. Omitting it auto-creates a new top video track (V2) above V1. Specifying `trackIndex: 0` destroys the A-roll by overwriting it on V1. This is the single most common mistake.

2. **Tile b-roll to cover the full timeline.** Each b-roll clip is N seconds × project fps frames. At 60fps, a 6s clip = 360 frames. Calculate: `totalFrames / 360` → round up → that many tiles. Last tile's `durationFrames` = `totalFrames - (N-1 × 360)`.

3. **Mute b-roll audio immediately** after placement — grab the linked audio clip IDs from the `add_clips` response:
   ```
   set_clip_properties({
     clipIds: [ALL_BROLL_AUDIO_IDS_FROM_RESPONSE],
     volume: 0
   })
   ```
   If you skip this, captions will transcribe b-roll dialogue and generate wrong caption text.

4. **Do not use `insert_clips`** for b-roll. It ripples the A-roll forward, destroying sync. Only use `add_clips`.

---

## Step 5: Apply layout with apply_layout — not set_clip_properties

For **every b-roll clip**, pair it with the A-roll clip it overlaps and call `apply_layout`:

```
apply_layout({
  layout: "top_bottom",   // or "top_bottom" with slots flipped for Format 2
  slots: [
    { clipId: BROLL_CLIP_ID, slot: "top" },
    { clipId: AROLL_CLIP_ID, slot: "bottom" }
  ]
})
```

**Pairing rule:** A b-roll clip at frames [X, X+360] should be paired with whichever A-roll clip contains frame X. Get the A-roll clip list from `get_timeline` and map each b-roll clip to its overlapping A-roll clip.

**"Clips never play at the same time" error:** `apply_layout` refuses if the two clips have no overlapping frames. This happens when a b-roll clip's range falls between two A-roll clips (due to cuts). Fix: pair it with the A-roll clip with the most frame overlap. If there's still no overlap (e.g. a 2-frame A-roll gap clip), use `set_clip_properties` as a fallback:
```
set_clip_properties({
  clipIds: [BROLL_CLIP_ID],
  transform: { centerX: 0.5, centerY: 0.25, height: 0.5, width: 1.616 }
})
set_clip_properties({
  clipIds: [AROLL_CLIP_ID],
  transform: { centerX: 0.5, centerY: 0.75, height: 0.5, width: 1.58 }
})
```

**Verify after all layout calls:** Call `get_timeline` and check every A-roll clip has `centerY: 0.75, height: 0.5` and every b-roll clip has `centerY: 0.25, height: 0.5`. Any clip showing `height: 0.316` was missed — re-apply layout for that pair.

**Never use `set_clip_properties` for layout when `apply_layout` works.** `apply_layout` also sets the correct crop so the subject fills the slot without stretching. Manual transforms skip the crop and can leave black bars.

---

## Step 6: Add captions — scoped, placed, styled correctly

```
add_captions({
  clipIds: [ALL_AROLL_VIDEO_CLIP_IDS],  // ALWAYS scope to A-roll only
  animation: "highlightPop",             // or "wordPop" for simpler style
  centerY: 0.5,                          // at the seam for stacked split; 0.82 for full-frame
  color: "#FFFFFF",
  highlightColor: "#FFD700",             // gold highlight, or brand color
  fontSize: 48,                          // 48 for stacked split; 60+ for full-frame
  isBold: true,
  maxWords: 3,
  textCase: "auto"                       // NOT "upper" — natural case
})
```

**Placement by format:**
- **Format 0 (straight intercut):** `centerY: 0.82` — lower third of full frame, above the UI band.
- **Format 1–2 (stacked split):** `centerY: 0.5` — exactly at the seam between top and bottom halves. This is where eyes land naturally and it clears both the visual proof area and the talking head.
- **Format 3 (floating head):** `centerY: 0.82` — lower third of the full-bleed b-roll frame.

**Critical rules:**
- **Always pass `clipIds`** scoped to A-roll video clips. Omitting `clipIds` transcribes all audio on the timeline including b-roll — this generates wrong caption text pulled from b-roll dialogue instead of the actual speaker. Always explicit.
- **`textCase: "auto"`** — natural sentence case. `"upper"` bakes uppercase into the text string and cannot be changed via `update_text` — you'd have to delete the track and regenerate.
- **Captions are not editable for case after generation.** If the user wants a different case, `remove_tracks` on the caption track and regenerate.
- **`highlightPop`** animates each word as it's spoken with a color highlight — preferred for talking-head UGC. `wordPop` is simpler and works when the highlight feel is too busy.
- To restyle an existing caption group without regenerating: `update_text({ captionGroupId: ID, ... })` — supports color, font, animation, transform. Does not support textCase.

---

## Step 7: Verify with inspect_timeline

```
inspect_timeline({ startFrame: 0, endFrame: totalFrames, maxFrames: 6 })
```

Check every frame sample for:
- ✅ B-roll filling the top half — no black (means b-roll ran short, add another tile)
- ✅ Talking head cleanly in the bottom half
- ✅ Captions visible at the seam
- ✅ No duplicate A-roll (two versions of the talking head stacked) — if visible, `get_timeline` and look for any clip with `height: 0.316` on a video track; that's a phantom, remove it

---

## Common failure modes and exact fixes

| Symptom | Root cause | Fix |
|---|---|---|
| B-roll overwrites A-roll | `trackIndex: 0` on b-roll `add_clips` | Remove b-roll clips, re-add without `trackIndex` |
| "Clips never play at the same time" | B-roll and A-roll don't overlap in frames | Use `set_clip_properties` fallback with explicit transform values |
| Captions show b-roll dialogue | `add_captions` called without `clipIds` | Remove caption track, re-add with `clipIds` scoped to A-roll |
| Project snaps to 16:9 after first clip | Source is 16:9, placed before setting canvas | `set_project_settings` 9:16 immediately after, re-verify layout |
| Duplicate A-roll visible | `apply_layout` with `mediaRef` slots created new clips | Delete phantom clips (height: 0.316 on video track) |
| Captions stuck uppercase | `textCase: "upper"` baked into text content | `remove_tracks` caption track, regenerate with `textCase: "auto"` |
| A-roll clip not bottom-half | Missed in `apply_layout` loop | Check all A-roll clips for `height: 0.316`, re-apply layout |
| `remove_words` refused (sync-lock) | Caption track exists and can't ripple | `remove_tracks` captions first, cut, re-add captions after |
| B-roll audio audible | Forgot to mute after `add_clips` | `set_clip_properties` volume 0 on all b-roll audio clip IDs |

---

## Track order — end state reference

| Index | Label | Content |
|---|---|---|
| 0 | V3 | Captions (text) |
| 1 | V2 | B-roll video (top half, muted audio on A2) |
| 2 | V1 | A-roll video (bottom half) |
| 3 | A1 | A-roll audio |
| 4 | A2 | B-roll audio (volume: 0) |

---

## Pacing benchmarks

- Hook in the first 2–3 seconds — if the first line doesn't earn the next second, nothing after it matters.
- A cut, b-roll switch, or caption change roughly every 3–5 seconds.
- Most UGC ads: 15–60 seconds. Shorter skews better on TikTok; 30–45s is the sweet spot for Instagram Reels with a CTA.
- Captions are not optional — most social video is watched muted.
- If the talking head is AI-generated, go b-roll-heavy — keep talking head segments to 2–3s max before cutting to b-roll. Longer holds on AI faces trigger skepticism.

---

## Guardrails

- Build fictional, generic personas — don't edit in a specific real, identifiable person without their consent, and don't recreate a named public figure's likeness.
- Keep wardrobe/pose/action choices brand-safe and non-sexualizing by default; only go further if the user explicitly asks.
