---
name: character-pipeline
description: Builds consistent characters in Palmier Pro via hero still to multi-angle identity sheet to scene placement still to generate_video with ranked referenceImageMediaRefs (@Image1/@Image2/@Image3). Use when the same face must survive across shots, casting someone into a new environment, Seedance identity lock, or photoreal consistency before video. Not for quick UGC talking-head anchors — use ugc-photo-prompts for those.
---

# Character Pipeline

Identity dies when you jump straight to `generate_video`. Build **still truth** first, then animate with a ranked reference stack. Pair video prompts with `video-prompting` (and `cinematic-motion` for handheld texture).

## Pipeline (hard order)

```
1. Hero still          generate_image (or import_media / user photo)
2. Multi-angle sheet   generate_image with referenceMediaRefs = [hero]
3. Scene placement     generate_image: env plate + sheet (+ optional wardrobe ref)
4. Fidelity close-up   optional generate_image or capture_frame from the scene still
5. Video               generate_video with ranked referenceImageMediaRefs
```

Do **not** call `generate_video` until sheets (and scene still) show ready in `get_media` — **no** `generationStatus`. Rerolling stills is cheap; video is not.

## Session gates

- `get_timeline` → `canGenerate`
- `list_models({ type: "image" })` then later `type: "video"`
- Propose each paid generation; wait for confirmation
- Folders e.g. `Character/Hero`, `Character/Sheets`, `Character/Scenes`, `Character/Takes`

## Model picks

Confirm via `list_models`:

| Step | Prefer |
|------|--------|
| Hero / sheet / scene stills | `grok-imagine`, `gpt-image-2`, or `seedream-v5-pro` (validated photoreal trio) |
| Video iterate | `seedance-2-mini`, MiniMax H3 (`hailuo-03`), or Flux 3 with `draft: true` |
| Video final | `seedance-2`, MiniMax H3, or Flux 3 **FLUX Enhance** via `enhanceDraftMediaRef` (draft only — not `upscale_media`) |

Trust each model's aspect enums from `list_models` (some image models use labels like `landscape_16_9`).

## Step 1 — Hero still

Clear identity plate. Prefer candid editorial over beauty-retouch.

Include: age/skin/hair/eyes/expression; simple stable wardrobe; repeatable light; **skin texture preserved** (pores — no smoothing).

Avoid: unrepeatable glam grade; extreme pose that fights the sheet; busy BG that leaks into identity.

If the user supplies a photo, `import_media` and skip generation.

## Step 2 — Character sheet

`generate_image` with `referenceMediaRefs: [heroMediaRef]`.

Requirements:

- 4 panels side by side: front, left profile, right profile, back of head (or a fixed multi-panel grid if matching another project skill's proven layout)
- Neutral light grey BG; even studio light; no color cast
- Same crop, height, distance every panel
- Identical simple wardrobe across panels
- Neutral expression unless user wants otherwise
- Exact facial structure from hero
- Clinical identity sheet — not fashion/editorial
- No vignette, no grade, no text watermark

## Step 3 — Scene placement still

Composite the character into a locked environment plate.

```
Preserve the entire scene exactly: <env elements>.
Do not alter camera angle, framing, perspective, or environment.

Insert the subject from the character reference sheet — <short identity anchors>.
Pose: <body contacts, gaze, expression>.
Wardrobe: <from clothing ref if any — fabric behavior in THIS pose>.
Anatomy fully correct — natural joints, no elongation, believable foreshortening.
Match the scene's existing light direction and softness.
Skin texture preserved — no retouching.
```

Example `referenceMediaRefs` order: `[environmentPlate, characterSheet, clothingRef]` — state each ref's job in the prompt.

**Environment plate** wins on camera and set. **Sheet** wins on face.

## Step 4 — Ranked refs for generate_video

| Order in `referenceImageMediaRefs` | Prompt tag | Role |
|------------------------------------|------------|------|
| [0] | `@Image1` | Environment / staging / opening composition |
| [1] | `@Image2` | Identity sheet (structure across angles) |
| [2] | `@Image3` | Fidelity close-up — match this face, light, proximity |

Declare that split in `[REFERENCE USE]`. Optionally also pass the scene still as `startFrameMediaRef` when the model supports first frame and motion should begin on that plate.

## Identity lock block

Keep a short stable paragraph and paste into every later prompt (update to the actual character):

```
Olive-warm skin, dark brown eyes, strong brow, full lips, high cheekbones,
defined jawline, long dark brunette hair with natural wave.
Natural skin texture — visible pores, no smoothing.
```

Consistency > poetry.

## Wardrobe changes

1. Neutral garment on the identity sheet when possible
2. Separate clothing still via `generate_image` / import
3. Scene placement binds fabric behavior to pose
4. Video `[IDENTITY LOCKS]` + negative: no wardrobe change

## Anti-patterns

- Video from one glam selfie, no sheet → angle drift
- Sheet panels with different light/crop → four different people
- Scene placement that “improves” the plate's camera → breaks continuity
- `generate_video` before sheets are ready
- Rewording identity every call → slow face melt

## Handoff

After the scene still is approved → `cinematic-motion` for the effects timeline, keeping this ranking in `referenceImageMediaRefs`.
