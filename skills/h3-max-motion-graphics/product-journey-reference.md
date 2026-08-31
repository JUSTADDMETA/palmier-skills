# Product journey reference

Generic pattern for ~10s **scripted product films** (Job B). Fill in the user’s product, script, accent, and chrome — do not assume a specific brand.

## 1. Intake

Ask or infer:

- **Product** — what it is (editor, agent, notes, CRM, …)
- **Script** — exact lines to show (or VO that must appear as type)
- **Logo(s)** — brand and/or partner mark (file or generate a clean mark plate)
- **Accent** — one hex, used rarely
- **Aspect / length** — default 16:9, 10s

## 2. Script → acts

Split the script into **phrase beats**. Map to the arc (times are guides, keep them unequal):

| Arc | Role | On-screen | UI motif (pick for *this* product) |
|-----|------|-----------|-------------------------------------|
| Inhale | Hook / question | Line 1 | Near-empty field + logo if needed |
| Door | Promise lands | Line 2 | Core canvas appears (timeline / doc / board / …) |
| Climb | How it works | Lines 3–4 | Interaction / connect / tools; energy rises |
| Hit | Payoff | Line 5 | Statement plate; one accent flash |
| Exhale | Brand | Product name | Sparse lockup + tiny live chrome footer |

**Climb** may contain 2–3 lines with **accelerating** staggers (each shorter than the last).

## 3. Product-native chrome cheatsheet

Reuse the *idea*, swap the surface:

| Product type | Door | Climb | Footer (exhale) |
|--------------|------|-------|-----------------|
| Video editor | Timeline + playhead | Clip rearrange, razor, waveform shapes | Mini scrubbing track |
| AI agent | Empty → linked canvas | Agent card docks, pulse link, autopilot edits | Status chip |
| Docs / notes | Blank page / editor | Blocks insert, cursor sprint | Thin ruler |
| Design board | Empty artboard | Frames dock, connectors | Zoom chip |
| Inbox / CRM | Empty list | Rows cascade, filters chip | Unread pip |

Always **change motif between acts**. Never the same card loop for 10s.

## 4. Copy-paste prompt template

```
Flat Apple-glass kinetic product film. Orthographic 2D only.
Frosted translucent panels, hairline borders, soft speculars.
Almost NO drop shadows. Accent <HEX> RARE only (one tip / one flash / one underline).

@Image1 = <LOGO_NAME> mark — preserve exact geometry when it appears; ignore @Image1 background.
(If Max-only: omit @Image1; use start frame logo plate + “keep that exact mark.”)

Only strings (exact, no others):
"<LINE_1>"
"<LINE_2>"
"<LINE_3>"
"<LINE_4>"
"<LINE_5>"
"<PRODUCT_NAME>"

TEMPO ARC (musical, not metronome):
0–~1.6s INHALE — <LINE_1> on sparse dark glass; soft restless drift; logo if needed.
~1.6–3.2s DOOR — frosted swipe reveals <CORE_UI>; <LINE_2>.
~3.2–5.0s CLIMB — <INTERACTION_UI>; <LINE_3>; canvas stays alive.
~5.0–7.0s ACCELERATE — denser product motion; <LINE_4> then next climb line if any; speed peaks.
~7.0–8.6s HIT — payoff plate <LINE_5>; one sharp accent flash.
~8.6–10s EXHALE — <PRODUCT_NAME> lockup; tiny live footer chrome; decelerate, never freeze.

Switch UI each act. Always moving. No people. No photoreal devices. No fake digits/paragraphs.
```

### Generate

**Logo-accurate (preferred)**

```
generate_video({
  model: "hailuo-03",
  prompt: "<filled template>",
  duration: 10,
  aspectRatio: "16:9",
  resolution: "2K",
  referenceImageMediaRefs: ["<logoId>"],
  folder: "H3-Max/UI-Kinetic",
  name: "<Product> journey vN"
})
```

**Max fallback**

```
generate_video({
  model: "hailuo-03-max",
  prompt: "<filled template without @Image1>",
  duration: 10,
  aspectRatio: "16:9",
  resolution: "768p",
  startFrameMediaRef: "<logoId>",
  folder: "H3-Max/UI-Kinetic",
  name: "<Product> journey Max vN"
})
```

## 5. Filled example (illustrative only)

Replace freely — this is not a brand skill.

- Product: AI notes app “North”  
- Accent: `#5B8CFF`  
- Lines: “What if your notes…”, “wrote themselves?”, “Connect your agent”, “and watch it draft”, “on autopilot”, “Shipped in one prompt”, “North”  
- Door UI: empty doc → outline blocks  
- Climb: agent card + blocks inserting  
- Hit: “Shipped in one prompt”

## 6. Iteration checklist

When a take fails, fix the *pattern* that broke:

| Feedback | Fix |
|----------|-----|
| “No rhythm / every beat the same length” | Unequal arc; accelerate climb |
| “Too much accent color” | Accent only at 1–3 punctuation moments |
| “UI is repetitive” | New motif every act; product-native |
| “Too 3D / plasticky” | Orthographic frosted 2D; kill extrusion |
| “Shadows look fake” | Hairlines + speculars; almost no drops |
| “Wrong / melted logo” | H3 + `@Image1` clean mark plate |
| “Not dynamic enough” | Stronger climb density + hit; keep inhale/exhale contrast |
| “Copy is wrong” | Re-lock allowlist from user script verbatim |

## 7. What this reference is not

- Not a brand kit for any one company  
- Not a substitute for Job A style stings (S01–S12 / tasteful)  
- Not captioning / VO edit craft (`caption-templates`, `ugc-editing`)
