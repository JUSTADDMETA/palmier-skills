---
# H3 Max style templates (verbatim)

Copy a block into `generate_video` with `model: "hailuo-03-max"`, matching duration/aspect. Swap only the string allowlist + accent when adapting for a brand — keep the animation grammar.

## S01 Swiss grid slow

Duration **15s** · Aspect **16:9**

```
TEMPLATE: Swiss Grid Reveal. Flat graphic motion design only. No people, no photos, no 3D gloss.

subject_definitions:
Accent: international orange #FF4500.
Strings only (exact, no others): "GRID" "SYSTEM" "MODULE" "ALIGN" "SPACE" "TYPE" "CLEAR" "RATIO" "END".
Map: S1=GRID S2=SYSTEM S3=MODULE S4=ALIGN S5=SPACE S6=TYPE S7=CLEAR S8=RATIO S9=END.
Colors: off-white, ink black, accent only. One bold geometric grotesque.

Pacing: SLOW / editorial. Long holds. Soft wipes. Almost no shake. Think Swiss poster, not trailer slam.

Animation grammar: A modular grid of thin black rules draws itself first. Words appear by horizontal wipe or column-slide into locked grid cells — never slam, shatter, or collide. Background stays flat off-white for most of the piece; one mid-piece flip to black with inverted type; final return to white.

Shot clock (different compositions):
0–2s: empty grid lines animate in, no words.
2–4s: GRID wipes into top-left cell, holds.
4–6s: SYSTEM slides into center cell.
6–8s: MODULE + ALIGN occupy two lower cells simultaneously via vertical wipe.
8–10s: BG flips black; SPACE and TYPE appear as large outline words, slow drift.
10–12s: CLEAR then RATIO snap into baseline row (soft, not impact).
12–15s: END locks bottom-right; everything static; grid remains visible.

Middle layer: only hairline registration marks and crop ticks, barely moving.
Sound: quiet ticks, soft whooshes — no bass drops.
```

## S02 Strobe rave

Duration **12s** · Aspect **16:9**

```
TEMPLATE: Strobe Rave Type. Flat graphic only. No people.

subject_definitions:
Accent: neon lime #C6FF00.
Only strings: "DROP" "NOW" "GO" "HARD" "LOUD" "FAST" "CUT" "FLASH" "BOOM".
Exact spelling. Bold condensed sans. Colors: black, white, accent only.

Pacing: ULTRA FAST. ~8 hard cuts in 12 seconds. Stroboscopic freezes. RGB split flashes between cuts. Every word appears for <0.8s then hard cut.

Animation grammar: Full-bleed single word per cut, centered, filling frame. Alternate black/white BG every cut. Accent used only for every 3rd word. Impact shake on every cut. No smooth motion between cuts — only hard cuts + 1-frame white flashes.

Clock:
0.0 DROP (black BG)
1.2 NOW (white)
2.2 GO accent on black
3.2 HARD white
4.2 LOUD black
5.4 FAST white
6.6 CUT black
7.8 FLASH accent
9.0–12.0 BOOM holds with continuous strobe flicker then hard freeze last 1s.

No floating particles that look like extra letters. No soft dissolves.
```

## S03 Liquid morph type

Duration **15s** · Aspect **16:9**

```
TEMPLATE: Liquid Morph Typography. Flat-to-semi-liquid graphic. No people. No photoreal water.

subject_definitions:
Accent: electric blue #00B0FF.
Only strings in order: "SOLID" → morphs into "MELT" → "FLOW" → "POOL" → "RISE" → "FORM" → "LOCK" → "EDGE" → "HOLD".
Each morph keeps letter count readable; never invent extra glyphs mid-morph. Exact target spelling at each settle.

Pacing: MEDIUM. Continuous morphs, almost no hard cuts. One continuous take feel.

Animation grammar: Letters behave like viscous ink or liquid metal on a flat plane. SOLID melts downward into MELT, flows sideways into FLOW, pools into POOL, rises into RISE, reforms into FORM, snaps hard into LOCK (only hard moment), edges sharpen to EDGE, final HOLD freezes sharp.
BG: slow gradient flip black→white→black once. Accent used as liquid highlight rims only.

Decorative mid-layer: soft ink blotches and thin ripples — never readable as text.
15s continuous. Final 2s fully sharp HOLD.
Sound: liquid whooshes + one hard click on LOCK.
```

## S04 Data HUD counter

Duration **15s** · Aspect **16:9**

```
TEMPLATE: Data HUD Counter. Flat UI motion graphic. No people. No fake paragraphs.

subject_definitions:
Accent: terminal green #00FF66 on black (primary). White secondary.
Only allowed on-screen text strings: "LOAD" "SYNC" "BUILD" "SHIP" "0→100" "OK" "LIVE" "CORE" "READY".
Note: "0→100" is one string including the arrow character exactly as written.
Never invent other numbers, units, or words. HUD ticks and barcode stripes are non-textual decoration only.

Pacing: STEADY techno. Rhythmic every ~1.2s. Counting energy without extra digits beyond the one allowed string.

Animation grammar: Dark HUD. Corner registration marks. Thin scan line. Words punch into fixed HUD slots (top-left, center, bottom-right) with digital shutter. Mid piece: the string 0→100 fills a progress bar made of accent blocks (blocks are shapes, not numbers). Final READY locks center with OK badge-shape (shape only, word OK exact).

Clock:
0–2 LOAD
2–4 SYNC
4–6 BUILD
6–8 SHIP
8–11 0→100 progress fill
11–12 OK
12–13 LIVE
13–14 CORE
14–15 READY hold.

No glassmorphism. Flat. Sound: UI blips + rising tone.
```

## S05 Letterpress stamp

Duration **15s** · Aspect **16:9**

```
TEMPLATE: Letterpress Stamp. Print-inspired flat graphic. No people.

subject_definitions:
Accent: deep crimson #9B0000.
Only strings: "PRESS" "INK" "PAPER" "WEIGHT" "GRAIN" "MARK" "PROOF" "FINAL" "PRINT".
Exact. One heavy oldstyle or slab display look — still flat vector, simulated letterpress, not photo of paper.
Colors: warm off-white paper field, charcoal black type, crimson accent stamps.

Pacing: SLOW HEAVY. Each word arrives as a physical stamp impact with ink squash then settle. Long holds.

Animation grammar: Camera locked top-down on paper field. Each word stamps from above with slight overshoot squash, ink micro-bleed edges (graphic, not photoreal). Between stamps, paper grain drifts slowly. Crimson used for every 3rd stamp only.

Clock: ~1.5s per stamp across PRESS→PRINT; last 2s PRINT holds with subtle grain only.
No shatter, no RGB, no wireframe. Sound: deep stamp thuds.
```

## S06 Z-depth tunnel race

Duration **15s** · Aspect **16:9**

```
TEMPLATE: Z-Depth Type Tunnel. Flat graphic in fake 3D perspective. No people. No realistic environment.

subject_definitions:
Accent: hot pink #FF2BD6.
Only strings racing toward camera in order: "ENTER" "DEPTH" "SPEED" "NEAR" "PASS" "CLOSE" "HIT" "STOP" "TITLE".
Exact spelling. Bold condensed. Black void BG. White + accent type.

Pacing: ACCELERATING. Words start far (tiny) and rush to fill frame then exit past camera. Tempo increases.

Animation grammar: Infinite perspective grid tunnel (hairlines only). Each word is a flat billboard flying on Z-axis toward camera. Motion blur on rush. Accent on HIT only. Final TITLE freezes filling frame; tunnel stops.

Clock:
0–2 ENTER far→near exit
2–4 DEPTH
4–6 SPEED faster
6–8 NEAR
8–10 PASS + CLOSE overlapping lanes L/R
10–12 HIT accent smash to camera then vanish
12–13 STOP freezes mid
13–15 TITLE lockup hold.
No extra signage. Sound: whoosh Doppler + final hit.
```

## S07 Luxury ribbon wipe

Duration **15s** · Aspect **16:9**

```
TEMPLATE: Luxury Ribbon Wipe. Elegant flat graphic. No people. No product photos.

subject_definitions:
Accent: champagne gold #C7A26A.
Only strings: "ATELIER" "SILK" "LINE" "QUIET" "GOLD" "FORM" "PURE" "VOGUE" "FIN".
Exact. High-fashion editorial pacing. Refined grotesque or didone-like flat letterforms.
Colors: black, ivory, gold only.

Pacing: VERY SLOW / luxury. Long dissolves. Soft parallax. Almost no hard cuts — mostly ribbon wipes and fades.

Animation grammar: Wide gold ribbon shapes wipe across revealing one word at a time. Words sit with huge margins. Soft horizontal drift. Mid-piece ivory BG; open/close on black. Final FIN small, centered, long hold.

Clock: ~1.6s per reveal ATELIER→VOGUE; FIN holds 12.5–15.
No shake, no strobe, no barcodes. Sound: soft pads, almost silent impacts.
```

## S08 Comic panel kinetic

Duration **15s** · Aspect **16:9**

```
TEMPLATE: Comic Panel Kinetic. Flat comic-graphic motion. No photoreal people.

subject_definitions:
Accent: comic yellow #FFE600 + ink black + white + one red #E10600 for emphasis words only.
Only strings: "POW" "SNAP" "ZOOM" "BANG" "WHIP" "CUT" "PANEL" "HERO" "END".
Exact. Bold comic display lettering, flat.

Pacing: COMIC BEAT. Panel borders slam in. Speed lines. Words are sound-effect style but must match exact spellings above — no extra onomatopoeia.

Animation grammar: Thick black panel frames assemble. Each beat a new panel composition with one word. Speed lines behind. Red only on BANG and WHIP. Final END across full splash page.

Clock: panels every ~1.5s through HERO; END splash 12–15 hold.
Halftone dots decorative only. No readable random SFX bubbles beyond the 9 strings.
Sound: cartoon hits + whip cracks.
```

## S09 Split duel L-R

Duration **15s** · Aspect **16:9**

```
TEMPLATE: Split-Screen Type Duel. Flat graphic. No people.

subject_definitions:
Left accent: cyan #00E5FF. Right accent: magenta #FF2D95.
Left-only strings: "LEFT" "PUSH" "WIN".
Right-only strings: "RIGHT" "PULL" "LOSE".
Shared center strings: "VS" "CLASH" "MERGE".
Only these 9 total. Exact. Never put left words on right or vice versa until MERGE.

Pacing: ANTIPHONAL. Alternating L/R hits then collision.

Animation grammar: Permanent vertical split. Left half cyan world, right magenta. Words hammer from their side toward the center line. VS stamps on the divider. CLASH makes both sides shake. MERGE dissolves the split into one field with MERGE centered; accents become stripes.

Clock:
0–3 LEFT then RIGHT alternate
3–6 PUSH vs PULL
6–8 VS on divider
8–10 WIN vs LOSE
10–12 CLASH full shake
12–15 MERGE hold, split gone.
Sound: stereo L/R hits then center crash.
```

## S10 One-shot orbit sculpture

Duration **15s** · Aspect **16:9**

```
TEMPLATE: One-Shot Orbit Type Sculpture. Flat graphic words as physical slabs in void. No people. No hard cuts at all.

subject_definitions:
Accent: violet #7C4DFF.
Only strings floating as slabs: "ORBIT" "MASS" "SPIN" "AXIS" "HOLD" "TURN" "FACE" "LOCK" "MARK".
Exact. Bold geometric. Black void. White slabs with accent edges.

Pacing: CONTINUOUS CAMERA. One shot 15s. Camera slowly orbits a cluster of word-slabs; words rotate to face camera on beat without cutting.

Animation grammar: Words are thick flat extrusions (graphic, not photoreal metal). Camera arcs 180°. Parallax. No cut. Final camera push-in on MARK; all other words settle in depth behind, slightly out of scale hierarchy.

Clock: continuous orbit; MARK becomes hero at 12–15.
No shatter. No BG flip. Sound: low drone + soft ticks.
```

## S11 Vertical stack stickers

Duration **8s** · Aspect **9:16**

```
TEMPLATE: Vertical Sticker Stack. 9:16. Flat social-graphic. No people faces.

subject_definitions:
Accent: Instagram-like gradient forbidden — use flat coral #FF5A5F only.
Only strings as sticker pills: "TAP" "SAVE" "DUET" "LINK" "BIO" "SHOP" "GO" "LIVE" "NOW".
Exact. Rounded sticker shapes with white fill + coral outline OR coral fill + white type alternating.

Pacing: POP STACK. Stickers pop in from bottom stacking upward every ~0.7s, slight overshoot spring. Final NOW largest on top.

Animation grammar: Soft gray BG. Stickers accumulate. Mild parallax drift. No hard cuts — continuous. Last 1.5s freeze stack.
Sound: UI pops.
```

## S12 Typewriter cascade

Duration **15s** · Aspect **16:9**

```
TEMPLATE: Typewriter Cascade. Flat terminal/editorial hybrid. No people.

subject_definitions:
Accent: amber #FFB300.
Only strings, typed in order as whole words (not per-letter garbage): "WRITE" "LINE" "BREAK" "AGAIN" "FASTER" "CLEAR" "DRAFT" "SEND" "DONE".
Exact. Monospace flat. Black BG, amber + white type.

Pacing: TYPE RHYTHM. Words appear via block caret typing then line-feed cascade upward like a terminal. Accelerates. No random characters — only the listed words.

Animation grammar: Blinking block caret. Each word types then locks. Previous lines scroll up. Mid-piece FASTER accelerates. CLEAR wipes screen once. DRAFT→SEND→DONE end sequence; DONE holds.

Decorative: only a thin amber underscore caret and faint column guide — no extra code text.
Sound: key clacks accelerating.
```
