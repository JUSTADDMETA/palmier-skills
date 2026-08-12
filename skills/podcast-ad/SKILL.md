---
name: podcast-ad
description: Builds converting podcast-style ads in Palmier Pro as an overheard two-person conversation — sceptic arc, believer mechanism, matching start frames, mic-in-shot staging, segmented generate_video beats, timeline assembly, and captions. Use for podcast ads, UGC conversation ads, sceptic-to-believer product spots — not for single-hook talking-head ads (use ugc-video-prompts).
---

# Podcast Ad

The ad is **two people, one room, one conversation**. Viewer overhears; nobody pitches the lens. Script is ~80% of the result.

Palmier does not generate a full multi-speaker room in one pass — **segment the exchange into model-length beats**, keep room/identity locked with matching stills + refs, then assemble on the timeline.

Pair with `video-prompting`. For faces that must match brand talent across variants, run `character-pipeline` first. Finish captions/layout with `ugc-editing` patterns when helpful.

## Why this format works

- She never addresses camera — kills “ad argument in the head”
- His doubts are the buyer’s doubts — conversion when *he* clicks
- Pattern-matches podcast clips in the feed, not classic ad shapes

Break overheard staging (lens eye contact, hard CTA, cheerleading sceptic) and you lose the format.

## Step 1 — Write the exchange (sceptic first)

| Role | Job |
|------|-----|
| Believer | Transformation. Casual certainty. Talks to a mate, not a customer. |
| Sceptic | Doubt → convert on camera. **Write him first — his arc is the ad.** |

**Sceptic’s four beats (required)**

1. **Open doubt** — names the quiet part out loud
2. **Honest question** — hands her the floor without agreeing
3. **The click** — mechanism connects to *his* life (not a parrot of her claim)
4. **Concession** — stops arguing; does **not** gush

**Believer craft:** human comparison over feature list; **disarm the scary version**, then educate with a specific mechanism; callback close; **no hard CTA** inside the dialogue.

Map the script onto generation beats that fit `list_models` durations (often 4–10s). One conversational turn (or half-turn) per clip is safer than cramming.

## Step 2 — Two matching start frames

`list_models({ type: "image" })` → `generate_image` for each speaker.

These stills become shot / reverse-shot anchors. They must read as **the same locked-off podcast camera** with only the subject swapped — not two different setups.

### Hard rules (this is where most generations die)

1. **One person only.** The speaking subject fills the frame. **No** foreground shoulder, back-of-head, OTS silhouette, interviewer blur, or second body in frame — those invent a different “other person” per still and break the cut.
2. **Same camera grammar on both.** Copy-paste identical wording for: camera height, distance (chest-up / mid-shot), lens feel, table edge position, mic position, wall, lights. Only wardrobe + face + gesture change.
3. **Same room word-for-word.** Wall, light, table, mic arm. Different rooms = broken ad.
4. Prefer generating still A, then still B with `referenceMediaRefs: [stillA]` and the prompt saying: preserve camera, room, light, mic, framing exactly — replace only the seated subject.

### Prompt pattern (reuse the locked block)

```
Podcast studio mid-shot, chest-up, eye-level camera, locked framing.
Single subject only — no other people, no foreground shoulder, no over-the-shoulder.
Wooden table edge low in frame, broadcast mic on boom in shot.
Background: <identical room description>.
Warm practicals, normal exposure, real room — not a film set.
Subject looks off-axis toward their conversation partner (not into lens), mid-gesture, mouth slightly open.
```

Then only the subject line differs:

```
Believer: blonde woman in a denim jacket, casual certainty, mid-sentence gesture.
Sceptic: man in striped polo, sceptical listening pose, mid-gesture.
```

Standing negatives on both: `no second person, no foreground silhouette, no OTS, no interviewer, no different camera angle, no wider/tighter crop than the pair, no looking into lens`.

### Frame checklist (reroll until all pass)

- [ ] Exactly one person visible
- [ ] No foreground person / OTS / shoulder
- [ ] Matching camera height, distance, and mic placement across both
- [ ] Matching wall / light / table wording (visually the same set)
- [ ] Mic in shot
- [ ] Off-axis eyeline (not into lens)
- [ ] Mid-gesture / mouth slightly open

`folder: "PodcastAd/Stills"`

## Step 3 — Generate conversation beats

1. `get_timeline` — set delivery aspect (often `9:16`) via `set_project_settings` **before** placing if needed.
2. `list_models({ type: "video" })`.
3. **Iterate** with `seedance-2-mini`, MiniMax H3 (`hailuo-03`), or Flux 3 `draft: true`. For quoted dialogue lip-sync when those miss, Kling V3/O3 or Grok. Multi-ref face+room+voice locks: Mini/H3/Seedance refs (`referenceImageMediaRefs` / `referenceAudioMediaRefs`).
4. **Final:** re-generate approved beats on `seedance-2` or H3, or — if the take was Flux 3 draft — `generate_video({ enhanceDraftMediaRef })` only (**FLUX Enhance** at 1080p; not `upscale_media`; requires `canEnhanceDraft`).
5. Propose each beat; wait for confirmation.
6. Per beat: `generate_video` with `startFrameMediaRef` = that speaker’s still (or shared wide), plus identity refs when using Seedance/H3.

**Prompt extras every beat**

```
[SCENE]
Single speaker at the podcast table in the start-frame room. Overheard conversation —
they talk to the off-screen partner, not the lens.

[GEOGRAPHY]
Match the start frame: one subject, mic in shot, off-axis eyeline. Do not invent a
second person or a foreground shoulder.

[DIALOGUE]
<Speaker> says exactly: "<line>"
Voice: <pitch>, <tone>, <accent>, <pace>   // identical string every beat for that speaker

[ACTING]
Believer: casual certainty. Sceptic: doubt → question → personal click → quiet concession.
No cheerleading. No hard sell to lens.

[CAMERA]
Hold the start-frame angle — subtle handheld or locked-off. No orbit, no new coverage.

[NEGATIVES]
No looking down the lens, no second person, no OTS/foreground silhouette, no room/light
change, no CTA lower-third, no identity swap, no subtitles/watermark.
```


Lock voice across beats: repeat the four voice pins **verbatim**, or pass beat-1 audio as `@Audio1` on later Seedance calls.

If one segment fails, regenerate **that beat** only. `folder: "PodcastAd/Beats"`

## Step 4 — Assemble + finish

```
add_clips({
  entries: [
    { mediaRef: BEAT1, startFrame: 0 },
    { mediaRef: BEAT2, startFrame: END1 }
  ]
})
```

- Product / before-after over the **mechanism** beat: `add_clips` on a higher video track (index 0 = top) or `apply_layout` for PIP
- Captions: `add_captions` — default clean style unless asked otherwise
- Tighten with `remove_silence` / `remove_words` if a beat ran long
- Do not over-polish into “ad grade”

## Turning one into thirty

| Lever | Change |
|-------|--------|
| Objection swap | Only his lines / his beats |
| Product swap | Keep four-beat arc; rewrite mechanism |
| Clip the click | His click beat as a short with a new hook ahead |

## Anti-patterns

- Writing believer first; sceptic has nothing to do
- Sceptic ends as brand ambassador
- Start frames staring at camera
- Mismatched rooms **or camera angles** between speakers
- OTS / foreground shoulder / second person in a “single” still (especially different people per still)
- Vague mechanism (“it just works”)
- Hard CTA inside the dialogue
- One overloaded clip trying to play the whole ad
- Expecting a single generate call to replace timeline assembly
