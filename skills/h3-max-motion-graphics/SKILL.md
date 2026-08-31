---
name: h3-max-motion-graphics
description: Kinetic titles, stings, Apple-flat glass UI motion, and scripted product promos in Palmier Pro with MiniMax H3 Max / H3 — exact string locks, journey tempo (not metronome chops), sparse accents, switching UI motifs, product-native chrome, and logo refs. Use for H3 Max / MiniMax motion graphics, kinetic type, cold opens, bumpers, glassy UI explainer spots, Introducing product films, agent demos, or recreating H3 MG bakeoff looks. Prefer over video-prompting for readable on-screen type. Sibling: video-prompting for cinematic multimodal; caption-templates for timeline captions.
---

# H3 Max motion graphics

Default stack for **flat kinetic type + UI motion graphics** in Palmier. H3 family can hold **exact short strings** when the prompt locks them — unlike most video models (those should use `generate_image` / `add_texts`).

Confirm live caps with `list_models({ type: "video" })` before every generate. Ids and caps drift.

## Two jobs (pick one)

| Job | Length | Optimize for |
|-----|--------|----------------|
| **A. Style sting / title** | 5–15s | One motion grammar (swiss, strobe, typewriter…). |
| **B. Product journey promo** | ~10s | User script + **musical arc** + **switching product UI**. |

Do not mix: a swiss grid sting is not a product film, and a product film is not twelve equal title cards.

## Model routing

| Need | Model | Notes |
|------|-------|--------|
| Flat MG / titles / glass UI (default) | `hailuo-03-max` | First/last frame only. **No** refs. Prefer **768p**. 5–15s. |
| Exact brand / partner **logo** in-shot | `hailuo-03` | `@Image1` = logo mark only. 2K. |
| Logo open only on Max | `hailuo-03-max` + `startFrameMediaRef` | Weaker than `@Image1`; mark may drift after frame 0. |
| True trim V2V edit | Aleph / Happy Horse Edit / … | `sourceVideoMediaRef` + `sourceClipId`. |

**Max ≠ H3.** Max cannot take `referenceImageMediaRefs` / `referenceVideoMediaRefs`. Hybrids on Max → `startFrameMediaRef` + “animate from the start frame.”

## Session gates

1. `get_timeline` — if `canGenerate` is false, ask sign-in / subscribe.
2. `list_models({ type: "video" })`.
3. Propose prompt + duration + aspect + resolution (+ logo/start frame). **Wait for confirmation** — paid, not undoable.
4. `generate_video` into a folder (`H3-Max/Styles`, `H3-Max/Tasteful`, `H3-Max/UI-Kinetic`, …). Do not busy-poll.
5. Assemble with `create_timeline` + `add_clips`. Retry failures once.

---

## Hard rules (all jobs)

1. **Exact string allowlist** — every on-screen phrase listed. “Exact spelling. No other glyphs / invented words / fake paragraphs / fake digits.”
2. **Accent is rare** — one hex as *punctuation* (one underline, playhead tip, peak flash). Not a full-video color wash.
3. **Flat / orthographic first** — Apple frosted UI or flat type. Ban heavy 3D extrusion and tumbling letter slabs unless asked.
4. **Glass clean** — frosted fills, hairline borders, soft speculars. Prefer almost no drop shadows over muddy umbras.
5. **Always moving** — no dead freeze on the last frame; land with residual drift.
6. **One job per generate** — separate sting vs ambient vs end card; cut on the timeline.

---

## Job A — style stings

### Prompt skeleton

```
TEMPLATE: <Style name>. Flat graphic. No people / no photos / no 3D gloss.

subject_definitions:
Accent: <hex> (rare).
Only strings: "<A>" "<B>" … Exact.
Type look: <…> — flat vector.

Pacing: <SLOW swiss | ULTRA strobe | …>.
Animation grammar: <how type moves>.
Shot clock: 0–Xs: …
```

### Style picker (grammar must change)

| Id | Energy | Use when |
|----|--------|----------|
| S01 Swiss grid | Slow / tasteful | Editorial, explainer |
| S02 Strobe rave | Bam / hard cuts | Drops only — not default product |
| S03 Liquid morph | Continuous | Morph titles |
| S04 Data HUD | Techno | SaaS shipping |
| S05 Letterpress | Heavy stamp | Craft / print |
| S06 Z-tunnel | Rush | Trailer enter |
| S07 Luxury ribbon | Quiet prestige | Fashion |
| S08 Comic kinetic | POW | Youth — not default product |
| S09 Split duel | Versus | A/B |
| S10 Orbit sculpture | 3D slabs | Only if user wants depth |
| S11 Sticker stack | Social 9:16 | Vertical |
| S12 Typewriter | Terminal | Draft / write |

**Tasteful set:** soft fade, whisper tracking, paper curtain, breathing scale, soft-focus resolve, lowercase drift, stacked quiet lines, silk rise — when the user wants calm / elegant / not slam. Write from the skeleton with SLOW pacing + dissolves.

Verbatim S01–S12: [styles-reference.md](styles-reference.md). Short use cases: [use-cases-reference.md](use-cases-reference.md).

---

## Job B — product journey promos

Scripted product / feature spots (agent demos, “what if…”, Introducing…, UI explainers).

### Rhythm (not a metronome)

“Max ~2s per readable hold” is a **ceiling**, not a grid. Build a **musical arc**:

1. **Inhale / hook** — slower, sparse, tension  
2. **Door** — one decisive swipe / reveal  
3. **Climb** — accelerating beats (staggers get *shorter*)  
4. **Hit / reward** — peak kinetic + payoff line  
5. **Exhale** — decelerate into brand lockup, still drifting  

Equal 2s chops kill the piece.

### Switch the UI every act

Do **not** loop the same frosted rounded card for the whole film. Change chrome to match **this product’s surface**:

| Act | Pattern | Examples by product type |
|-----|---------|---------------------------|
| Hook | Near-empty + type (+ logo) | Wordmark field |
| Door | Core canvas appears | Timeline, doc, board, map, inbox |
| Climb | Interaction / agent / tools | Clips rearrange, cards dock, nodes connect |
| Hit | Payoff statement | “Built in one prompt”, “Ship today”, etc. |
| Exhale | Sparse lockup | Product name + tiny live chrome footer |

For **editors**: timeline, playhead, clip blocks, waveform shapes (no fake digits), razor. For **agents**: connect card, pulse link, autopilot rearrange. For **other SaaS**: swap in that app’s real chrome — still switch motifs per act.

### Script → allowlist

User copy becomes the **only** on-screen strings, phrase by phrase. Map each phrase to an act. Do not paraphrase on screen.

### Logo lock

1. Clean mark still (`generate_image` or `import_media`) — mark only, plain field.  
2. Prefer **`hailuo-03`** + `referenceImageMediaRefs` + `@Image1` = exact mark, ignore BG.  
3. Max fallback: `startFrameMediaRef` (weaker continuity).  
4. “Preserve exact geometry — do not redesign the mark.”

### Glass + accent

- Frosted translucent panels, hairline borders, soft highlights  
- Almost no soft-black card shadows  
- Accent hex only at punctuation moments  
- Flat Control-Center energy — not AE 3D glass in perspective

### Product prompt skeleton

```
Flat Apple-glass kinetic product film. Orthographic 2D.
Frosted UI, hairlines, almost no drop shadows. Accent <hex> RARE only.

@Image1 = <partner or brand> logo mark — exact geometry when it appears.
(OR: animate from start frame logo plate.)

Only strings: <user script phrases>. Exact. No other glyphs.

TEMPO ARC (unequal):
0–…s INHALE: …
… DOOR: …
… CLIMB (accelerating): …
… HIT: …
… EXHALE: …

SWITCH UI each act: <product-native motifs>.
Always moving. No dead freeze. No people. No photoreal devices.
```

Worked generic template: [product-journey-reference.md](product-journey-reference.md).

---

## Generate call shapes

**Style / Max default**

```
generate_video({
  model: "hailuo-03-max",
  prompt, duration: 5|8|10|12|15,
  aspectRatio: "16:9"|"9:16",
  resolution: "768p",
  folder: "H3-Max/…",
  name,
  startFrameMediaRef  // optional hybrid / logo open
})
```

**Logo-accurate product**

```
generate_video({
  model: "hailuo-03",
  prompt,  // includes @Image1 authority
  duration: 10,
  aspectRatio: "16:9",
  resolution: "2K",
  referenceImageMediaRefs: ["<logoId>"],
  folder: "H3-Max/UI-Kinetic",
  name
})
```

## Assemble

1. `create_timeline` for the set.  
2. `add_clips` sequential `startFrame` (~`durationSeconds * fps`).  
3. Keep variants on separate timelines or clearly named — do not overwrite.

## Anti-patterns (from iteration)

| Bad | Do instead |
|-----|------------|
| Even 2s title cards | Inhale → climb → hit → exhale |
| Same frosted card all the way | New product-native motif each act |
| Accent color everywhere | Accent as punctuation |
| Heavy 3D glass slabs | Orthographic frosted 2D |
| Muddy drop shadows | Hairlines + speculars |
| Palette-only “style pack” | Change motion grammar |
| Still→I2V when user wanted a video beat | Bake trim / V2V / `@Video1` |
| Invented UI copy / fake % | Allowlist only |
| Text-only “use the logo” | H3 + `@Image1` (or Max start frame) |

## Not this skill

- Timeline captions over dialogue → `caption-templates`  
- Photoreal cinematic / performance → `video-prompting` / `cinematic-motion` / `ugc-video-prompts`  
- Legal static logo end cards → often `generate_image` + `add_texts`

## Sibling handoff

Pure sting/promo reels usually stop at assemble. Full edits → `ugc-editing` patterns after MG plates exist.
