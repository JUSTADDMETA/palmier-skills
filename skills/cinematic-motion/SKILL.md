---
name: cinematic-motion
description: Writes Palmier generate_video motion prompts with documentary handheld texture, timed effects inventories, density maps, and one signature visual moment so Seedance/Kling clips feel captured. Use for photoreal editorial motion, unstable handheld, rack focus, micro-gestures, beauty/product clips, or dialogue-to-camera moments that should look like a real shooter was rolling.
---

# Cinematic Motion

Pretty stills go fake when motion is generic (“slow cinematic push-in”). Treat the clip as a **timed effects score**: ambient texture always on, one signature event, then settle.

Prereqs: approved stills from `character-pipeline` (or an existing plate). Authority rules from `video-prompting`.

## Design principles

1. **Camera has a body** — breathing drift, micro-tilts, corrections. Imperfect, not chaotic.
2. **One signature visual moment** — authenticity earned in 1–2s (subject moves → camera reacts).
3. **Density map** — low → high → low. Stillness is a tool.
4. **Micro over macro** — hair tuck, sun-block, chin ease, gaze lift > orbits.
5. **End on the still** — final frame nearly matches the fidelity image (`@Image3` / start frame).

## Call pattern

```
list_models({ type: "video" })
# Iterate: seedance-2-mini | hailuo-03 (MiniMax H3) | flux-3 with draft: true
# Final:   seedance-2 | hailuo-03 | Flux draft → generate_video({ enhanceDraftMediaRef })
# FLUX Enhance ≠ upscale_media — only works when get_media reports canEnhanceDraft on a Flux 3 draft.
# Ask before Seedance 1080p/4K.

generate_video({
  model: "seedance-2-mini",   // or hailuo-03 / flux-3
  draft: true,               // Flux 3 iteration only; omit for Mini/H3
  prompt: "<effects timeline brief>",
  duration: <allowed seconds>,
  aspectRatio: "<project>",
  resolution: "720p",
  startFrameMediaRef: "<scene or fidelity still>",
  referenceImageMediaRefs: [envOrScene, identitySheet, fidelityCloseup],
  folder: "Character/Takes"
})
```

Map refs in the prompt:

```
@Image1 → environment / opening frame / staging
@Image2 → identity sheet (structure lock)
@Image3 → primary fidelity close-up (match this face/light/proximity)
```

If only `startFrameMediaRef` is used, keep the prompt on **change**: camera + micro-motion + sound — do not re-describe the plate.

## Prompt structure

```
[REFERENCE USE]
…

[EFFECTS TIMELINE]
SHOT / 0:00–0:XX — continuous shot type
  MOMENT blocks: time range, EFFECT line, subject action, camera behavior

[MASTER EFFECTS INVENTORY]
Named effects, when used, role

[EFFECTS DENSITY MAP]
Per range: LOW / MEDIUM / HIGH + effect count

[MOTION FLOW]
Opening → Build → Resolution

[NEGATIVES]
…
```

## Effects library

| Effect | Use for |
|--------|---------|
| Unstable handheld / micro-reframe | Primary “real shooter” texture |
| Operator correction settle | Opening already mid-roll |
| Camera reaction drift | Camera slightly behind subject motion |
| Involuntary grip tilt (2–5°) | Documentary imperfection peak |
| Shallow DoF hold | Face isolation |
| Natural light flicker / canopy pulse | Organic illumination (not strobe) |
| Real-time subject micro-motion | Hair fix, gesture, resettling |
| Rack focus | Hierarchy shift between subjects |
| Rapid unstable push-in + shake spike | Reactive emphasis (once) |
| Handheld stillness / breath hold | Let a line or look land |

**Rule:** one continuous ambient effect (usually handheld) + at most one aggressive move per short clip.

## Density recipes

**6–8s beauty / product:** 0–1s LOW (correcting into frame) → 1–3s HIGH (micro-action + chase + tilt/light) → 3–5s MED (resettle) → 5–6s LOW (hold near fidelity still).

**Dialogue / two-subject:** cold open LOW → speaker A MEDIUM → silence LOW → rack/push HIGH → speaker B button LOW.

**Signature example:** both subjects on lens for a half-beat of silence, then unstable push to the reply.

## Writing subject + camera

- Gaze rule is law: never look at camera **or** direct to lens — pick one.
- Describe **contacts**: chin on forearms, palm blocking sun, hand returning.
- Camera: “3–5° involuntary tilt nudged back”, “4–5cm lateral drift, re-centers”, “brief shake spike then settle”, “breathing rhythm; no gimbal look”.
- Avoid: orbit, drone, perfect dolly, vague “cinematic camera movement”.

## Dialogue

Exact quoted line + voice pins (pitch, tone, accent, pace) every call. Models that bake speech need the line in the prompt; silent video when speech was intended is usually a bug.

## Anti-patterns

- Five big camera moves in 6 seconds
- No signature moment — evenly “alive” = fake
- Subject and camera both idle (slideshow)
- Rack focus with no motivation
- Ending on a different face than `@Image3` / start frame promised
- Defaulting Seedance to 4K without user opt-in
- Using `upscale_media` instead of `enhanceDraftMediaRef` for a Flux 3 draft

## Mini template (single-subject beauty)

```
@Image1 → scene staging
@Image2 → identity sheet
@Image3 → fidelity close-up to match exactly

0:00–0:01 LOW — handheld already correcting into @Image3 framing; subject still
0:01–0:03 HIGH — one real micro-gesture; camera chases late; 2–3° grip tilt; light pulse
0:03–0:05 MED — hand settles; gaze rule held; camera re-finds, slightly tighter
0:05–0:06 LOW — micro chin/shoulder ease; hold near @Image3

Inventory: unstable handheld (all); subject micro-motion (once); reaction drift;
grip tilt; shallow DoF; canopy light pulse.
Negatives: no look-at-camera (if off-axis), no wardrobe change, no face drift,
no stabilized gimbal look, no text/watermark.
```
