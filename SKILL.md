---
name: higgsfield-ugc-playbook
description: End-to-end framework for producing UGC and marketing video ads using Claude + Higgsfield MCP. Covers the 6-phase pipeline (brief → assets → ideation → prompt construction → generation → iteration), the MCSLA prompt structure (Model·Camera·Subject·Look·Action), 6 hook archetypes, model selection, QA checklist, common failure fixes, and cost guidance. Optimized for Pantree and Mr. White (404Studios) but applies to any app product.
when_to_use: "make a Higgsfield ad", "generate UGC video", "write a Higgsfield prompt", "create an ad for Pantree", "create an ad for Mr. White", "Soul ID", "Seedance prompt", "MCSLA", "hook variation", "video ad brief"
allowed-tools: Read
---

# Higgsfield UGC Playbook — Claude + Higgsfield MCP

> **The rule:** Brief first, then assets, then prompts. Skipping Phase 1 is the #1 reason AI ads underperform. Garbage in, beautiful-looking garbage out.

## The 6-Phase Pipeline

| Phase | Output | Frequency |
|-------|--------|-----------|
| 1. Strategy Brief | `BRIEF.md` Claude can reason from | Once per product |
| 2. Asset Library | Soul ID + product refs + brand kit | Once per product |
| 3. Ideation | 6 hook variations × 3 angle directions | Per ad batch (5 min) |
| 4. Prompt Construction | Production-ready MCSLA prompt | Per ad (5 min) |
| 5. Generation & QA | 4 variants screened against checklist | Per ad (10 min) |
| 6. Iteration | AI feedback loop, rework, re-test | Ongoing |

---

## Phase 1 — Creative Brief Template

Fill once per product. Paste at the start of every generation session.

```markdown
# Product Creative Brief

## 1. Product
- Name:
- One-line pitch (10 words max):
- Landing page / App Store URL:
- Pricing: free / freemium / paid

## 2. Audience
- Primary persona (age, role, location, device):
- Secondary persona:
- Emotional state when they discover this product:
  (e.g. "frustrated with X", "bored on couch", "curious about Y")

## 3. The "Why Care"
- Pain or job being solved:
- Cost of NOT having it:
- What changes in their life after using it:

## 4. Competitive Context
- What do they currently use instead?
- Why is this better (one sentence):

## 5. Visual & Brand
- Brand colors (hex):
- Tone of voice (e.g. "warm friend", "deadpan funny", "expert reviewer"):
- Aesthetic refs (3 TikToks / ads / IG accounts):

## 6. Platform & Format
- Primary: TikTok / Reels / Shorts / Meta feed
- Aspect ratio: 9:16 / 1:1 / 16:9
- Duration: 8s / 12s / 15s
- Sound on / sound off priority:

## 7. CTA
- Where do they go after watching:
- Exact words (phonetic spelling if AI mispronounces):
```

**Save as:** `BRIEF.md` in a Claude Project alongside `BRAND_VOICE.md`.

---

## Phase 2 — Asset Library (once per product)

### Soul ID Character — Highest-Leverage Asset

One trained character → unlimited ads without re-prompting facial features.

**Strategy:** Train one Soul ID per brand persona.
- Pantree → relatable home-cook spokes-character
- Mr. White → consistent host/MC party persona

**Photo requirements:**

| Rule | Reason |
|------|--------|
| 15–20 photos minimum (Soul 2.0 needs 20+) | More angles = better dimensionality |
| Recent photos (last 4–5 months) | Most true-to-life output |
| Consistent lighting across the set | Prevents lighting inference errors |
| No sunglasses, masks, heavy shadows | Face must be clearly visible |
| Variety of angles + expressions | Avoids locking onto single pose |
| No extreme expressions | Neutral baseline = more flexibility |
| Quality > quantity | 15 great shots beats 30 noisy ones |

**Training prompt:**
```
Train a Soul Character named "[name]" using the photos I'm uploading.
This character is the brand persona for [Pantree / Mr. White].
Confirm the soul_id when ready.
```
Training: ~3–10 min background. Don't wait — move on.

**Credit cost:** ~40 credits one-time per character.

### Product Reference Images
- 3–5 clean app UI screenshots (no notification bars)
- 1–2 logo/icon assets (transparent PNG)
- Brand color hex codes
- Phonetic spelling of product name documented

### Brand Voice File (`BRAND_VOICE.md`)
```markdown
# [Product] Brand Voice
- Tone: [e.g. friendly, casual, slightly nerdy]
- Phrases we use: [specific language]
- Phrases we avoid: [corporate-speak, etc.]
- Pronounce: [PHONETIC-SPELLING] (rhymes with "[word]")
```

### Marketing Studio Setup (one-time per product)
```
Use Marketing Studio to fetch my product page: https://[url]
Type: webproduct
```
Higgsfield crawls and stores the metadata for reuse across all future ads.

---

## Phase 3 — Ideation: Hook Framework

**The rule:** The first 3 seconds decide the ad's fate. Generate one variation per archetype, then test which wins with your audience.

### 6 Hook Archetypes

| # | Archetype | Pattern | Pantree Example | Mr. White Example |
|---|-----------|---------|-----------------|-------------------|
| 1 | **Bold claim** | "[Surprising fact flipping default assumption]" | "I haven't thrown away food in 3 months." | "My friends have no idea they're all liars." |
| 2 | **Called-out problem** | "If you've ever [frustration], this is for you" | "If your fridge is a graveyard, watch this." | "If your friends are competitive, don't get this." |
| 3 | **Curiosity gap** | "I tried this for [time period] and..." | "I tried this app for 30 days and stopped grocery shopping twice as often." | "We played this for 6 hours. Someone cried." |
| 4 | **Question** | "Why do [audience] always [behavior]?" | "Why do we keep buying spinach we never eat?" | "Why do friend groups always need one more game?" |
| 5 | **Disqualifier** | "Don't [try this] if [filter]" | "Don't get this app if you actually like wasting $40 a week." | "Don't play this if you have trust issues." |
| 6 | **Demo-first visual** | Cold open showing product working | Tap → recipe appears → fridge updates | Word reveal on TV → group erupts → no dialogue yet |

### Body Structure (after hook)

```
Hook    (0–3s):   pattern interrupt
Problem (3–5s):   the pain, named specifically
Solution (5–10s): show the app doing the thing
Outcome (10–13s): what changes / proof
CTA     (13–15s): spoken + phonetically spelled
```

### Ideation Prompt to Claude

```
Brief: [paste BRIEF.md]

Generate 6 hook variations for a UGC ad for [product name].
Use one of each archetype:
1. Bold claim
2. Called-out problem
3. Curiosity gap
4. Question
5. Negative/disqualifier
6. Demo-first visual

For each hook, also give:
- Body beat (3–10s)
- Outcome moment (10–13s)
- CTA (13–15s)

Keep each script under 50 spoken words.
Conversational tone, no corporate speak.
Phonetic spelling for any mispronunciation risk.
```

Pick 1–2 to generate. The rest are backlog.

---

## Phase 4 — Prompt Construction: MCSLA Framework

Every Higgsfield prompt is structured in 5 layers:

| Letter | Layer | What goes here |
|--------|-------|----------------|
| **M** | Model | Which Higgsfield model |
| **C** | Camera | Angle, motion, lens |
| **S** | Subject | Soul ID + outfit + expression + props |
| **L** | Look | Aesthetic, color grade, lighting, film stock, aspect ratio |
| **A** | Action | Beat-by-beat with timestamps |

### Model Selection

| Model | Best For | Default? |
|-------|----------|---------|
| **Seedance 2.0** | Multi-shot cinematic UGC, identity consistency | ✅ Default for UGC |
| **Soul 2.0** | Realistic portrait stills, product hero shots | For reference frames |
| **Kling 3.0** | Multi-shot + audio, best lip-sync | When dialogue quality critical |
| **Veo 3.1** | Top-quality realism + native audio | Hero campaigns only |
| **Nano Banana 2** | 4K sharp text/UI, product screenshots | UI close-ups with readable text |
| **Marketing Studio Video** | URL-driven product ads | When Higgsfield pulls context from URL |

**Rule for 404Studios:** Seedance 2.0 by default. Upgrade to Veo 3.1 only for the *winning* variant of a hero campaign.

### Universal Prompt Template

```markdown
**Model:** Seedance 2.0
**Total:** 12s / 4 shots / 9:16

**Look (applied to all shots):**
Cinematic lighting, photorealistic, 35mm film quality, natural color grading,
sharp focus, subtle film grain, depth of field, ARRI ALEXA aesthetic.
[Brand-specific: "warm domestic kitchen light" for Pantree /
 "dimmed living room, ambient string lights" for Mr. White]

**Subject:**
[Soul ID name], [outfit], [expression/emotion arc].
[Product visible: phone showing app UI / friends playing game on TV]

**Camera (UGC default):**
Selfie handheld, slight natural shake, eye-level, vertical 9:16,
phone-camera lens (slight wide).

**Action:**

Shot 1 (0–3s) — HOOK:
[exact dialogue line in quotes]
[facial expression + body language + framing]

Shot 2 (3–7s) — PROBLEM/CONTEXT:
[dialogue]
[@image1 showing [product] home screen, finger taps on "[action]"]

Shot 3 (7–11s) — SOLUTION:
[dialogue]
[product reveal beat]

Shot 4 (11–14s) — OUTCOME + CTA:
[dialogue with phonetically-spelled CTA]
[end frame gesture]

**Audio:**
Native Seedance dialogue, no music, ambient room tone.

**Anti-slop guard:**
No 3D, no cartoon, no plasticky skin, no AI sheen, no logo glitches.
```

### Worked Example: Pantree "Called-Out Problem" Ad

```
Brief context: Pantree (PAN-tree) is a food/pantry tracking app.
Audience: 25–35 home cooks frustrated with food waste in Singapore.
Soul ID: "Mei" (Pantree spokesperson).

Model: Seedance 2.0
Total: 12s / 4 shots / 9:16

Look: warm domestic kitchen light, soft window backlight, natural color
grading, sharp focus, 35mm film quality, subtle grain, photorealistic.

Subject: Mei (Soul ID), late 20s, casual oversized tee, hair in messy
bun, mild-frustration-to-relief emotional arc. Holding phone showing
Pantree app UI throughout.

Camera: Selfie handheld, eye-level, slight natural shake, vertical 9:16,
phone-camera lens.

Shot 1 (0–3s) — HOOK:
Mei opens fridge dramatically, peers in, deadpan to camera.
"If your fridge is a graveyard, this is for you."
Beat. She closes the fridge.

Shot 2 (3–6s) — PROBLEM:
Holds up wilted spinach bag, slight cringe.
"I throw out forty bucks of food every week. Maybe more."

Shot 3 (6–10s) — SOLUTION:
Cut to phone — @image1 showing Pantree home screen, finger taps
"Scan receipt", inventory updates with expiry dates.
Voiceover: "This app tracks what's in my fridge and tells me what's
about to die. Recipes pop up using only what I have."

Shot 4 (10–12s) — CTA:
Back to Mei with fresh spinach, smiling, raising phone.
"It's called pan-tree dot food. Link's right there."
Points down toward bio link.

Audio: native Seedance dialogue, no music, kitchen ambient tone.

Anti-slop: no plasticky skin, no AI sheen, no UI glitches on phone
screen, accurate spelling of "Pantree" if text appears.

Generate 4 variants.
```

### Worked Example: Mr. White "Demo-First Visual" Ad

```
Model: Seedance 2.0
Total: 12s / 4 shots / 9:16

Look: dimmed living room, ambient string lights, soft warm color grade,
slightly desaturated, late-evening party feel, 35mm, natural grain.

Subject: Group of 4 friends mid-20s on couch, Soul ID "Daniel" hosting.

Camera: Mix of selfie handheld + one wider tripod shot. Vertical 9:16.

Shot 1 (0–3s) — HOOK (demo-first visual):
Cold open: TV screen shows Mr. White word reveal moment.
Group erupts — gasps, pointing, face-palm. No dialogue. Chaos.

Shot 2 (3–6s) — CONTEXT:
Daniel (Soul ID) deadpan smirk to camera:
"My friends are all liars. We have the receipts."

Shot 3 (6–10s) — DEMO:
Quick cuts: secret word reveal on phone, dramatic accusation, friend
caught in lie.
Daniel VO: "It's Mr. White. Everyone gets the same word — except one
person. They have to lie their way through. We've been playing for six
hours. Help."

Shot 4 (10–12s) — CTA:
Group all facing camera, Daniel holds up phone with App Store icon.
"Mister White. Free on the app store. You're welcome. Or sorry."

Audio: ambient room tone, natural group laughter, no music.

Anti-slop: no plastic faces, real friend-group energy, no forced
laughter, no glitched phone screens.

Generate 4 variants.
```

### 8 Non-Negotiable Prompting Rules (from Higgsfield Seedance 2.0 guide)

1. **Always declare shot structure first.** `Total: [duration] / [N shots] / [ratio]` — without it, the model chooses and usually chooses wrong.
2. **Number every shot with timestamps.** `Shot 1 (0–3s):` — not prose. Seedance is multi-shot only when you signal it explicitly.
3. **Explicitly state what the camera is NOT doing.** For locked POV: `"no cuts, no zoom, natural head movement"`.
4. **Use bracketed VFX cues inline.** `[VFX: phone screen animation showing inventory updating]` — separates effect description from action.
5. **Anti-slop phrase library:** `"no 3D, no cartoon, no VFX"` / `"practical VFX feel, natural imperfections"` / `"strong 35mm film look, heavy film grain, sharp but imperfect focus"`.
6. **Phonetic CTAs always.** Write CTA as it should sound: `"pan-tree dot food"`, `"mister white"` (never "Mr.").
7. **Reference uploaded images via `@image` syntax.** `"@image1 showing the Pantree home screen"` — Seedance recognizes the reference token.
8. **Separate identity from motion.** When using Soul ID, describe character identity once, motion separately. Don't restate facial features — `soul_id` locks them.

---

## Phase 5 — Generation & QA

### Generation Prompt to Claude

```
Use Higgsfield to generate this ad.
Model: Seedance 2.0
Variants: 4
Aspect ratio: 9:16
Soul ID: "[character name]"
Product UI ref: [uploaded screenshots]
Marketing Studio: [product name] webproduct

[paste MCSLA prompt]

Estimate credit cost before generating.
```

### QA Checklist (run on every variant)

```
[ ] Hook lands in first 3 seconds — no slow build, no logo intro
[ ] Audio is intelligible — every word clear
[ ] Brand name pronounced correctly
[ ] CTA spoken AND visible on screen
[ ] Soul ID character looks consistent (no warp between shots)
[ ] Product UI readable, no glitches, accurate text
[ ] No uncanny artifacts (extra fingers, melted faces, plastic skin)
[ ] Pacing feels human, not AI-rushed
[ ] Natural background audio, no abrupt cuts
[ ] Aspect ratio + duration as requested
[ ] Mute test: does visual alone tell the story?
```

**Mute test is critical.** 85% of feed video is watched with sound off (Meta data).

### Failure Mode Fixes

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Hands look weird | Common Seedance limitation | Frame hands out or accept |
| Brand name mispronounced | Model can't tokenize compound word | Hyphenate: `"pan-tree"`, `"mister-white"` |
| Face shifts between shots | Soul ID not passed or mismatch | Verify `soul_id` is in the call; reduce style preset intensity |
| Plastic / CGI skin | Default model bias | Add `"no 3D, no plastic, natural skin texture, visible pores, 35mm film grain"` |
| Boring opening, slow build | Hook not front-loaded | Move dialogue into Shot 1; add `"immediate, no establishing shot"` |
| UI text on phone is gibberish | Model can't render small UI | Switch to Nano Banana 2 for UI close-ups, composite or cut to that frame |
| Lip sync off | Seedance dialogue limit | Switch to Kling 3.0 (better) or Veo 3.1 (best) |
| Flat energy | Underspecified emotion arc | Add explicit beats: `"Shot 1: deadpan → Shot 3: relief"` |

### Credit Cost Reference

| Action | Cost |
|--------|------|
| 4× Seedance 2.0 variants, 12s | ~30–60 credits |
| 4× Veo 3.1 variants, 12s | ~80–150 credits |
| Soul ID training (one-time/character) | ~40 credits |
| Nano Banana 2 image | ~3–8 credits |

**Budget strategy:** Hook variation tests on Seedance 2.0. Upgrade *only* the winning variant to Veo 3.1 for hero placement.

---

## Phase 6 — Iteration

### AI Feedback Prompt

```
Here are the 4 variants: [paste Higgsfield URLs]

Analyze each as a paid social ad targeting [persona].
Score on:
- Hook strength (stops the scroll?)
- Clarity of value prop
- Authenticity (real or AI feel?)
- CTA effectiveness
- Mute-test viability

Tell me: which to ship, which to rework, which to kill.
For rework candidate, suggest specific prompt edits.
```

> For deeper analysis: drop the video into Gemini — free and excellent at video analysis.

### Hook-Level Variation Testing (the scaling move)

Once you have a winning ad, keep Shot 2–4 identical. Only change Shot 1.

```
Take my winning ad [paste prompt]. Keep shots 2–4 identical.
Generate 5 new Shot 1 variants, each a different archetype:
- Bold claim variation
- Question variation
- Disqualifier variation
- Statistic-led variation
- Visual-first variation

Same Soul ID, same lighting, same camera. Only the hook changes.
```

This produces 5 scientifically comparable ads. The winner reveals which *hook type* resonates. Then scale that type.

### Anti-Slop Rules

1. **Reject more than you ship.** 4 generated → 1–2 shipped. If you use all 4, your bar is too low.
2. **Warm up social accounts first.** Engage 10 min/day in your niche for 1–2 weeks before posting AI UGC.
3. **Test organically before paying.** TikTok organic signal first, then Meta paid.

---

## Quick Reference

### One-Liner Generation Request (after setup is complete)

```
Using my [product] brief and Soul ID "[character name]", generate a 12s
9:16 Seedance 2.0 UGC ad with a "[hook archetype]" hook about [pain point],
ending with the CTA "[phonetic CTA]". 4 variants.
```

### Setup Checklist (one-time per product)

```
[ ] BRIEF.md filled in and saved to Claude Project
[ ] BRAND_VOICE.md saved alongside it
[ ] Soul ID trained (15+ photos, status: ready)
[ ] Marketing Studio fetch run on landing page URL
[ ] Product UI screenshots uploaded (clean, no notifications)
[ ] Phonetic spelling documented
[ ] Monthly credit budget decided
```

### Per-Ad Checklist

```
[ ] Brief pasted at top of conversation
[ ] Hook archetype chosen (one of six)
[ ] MCSLA prompt assembled with shots numbered + timestamps
[ ] Anti-slop phrases included
[ ] Phonetic CTA written into Shot 4 dialogue
[ ] 4 variants requested
[ ] Credit estimate confirmed before generating
[ ] QA checklist run on all outputs
[ ] AI feedback gathered before shipping
```

---

## Product Notes (404Studios)

### Pantree
- Pronounce: `PAN-tree` (rhymes with "gantry") — never "Pantree" in prose
- URL: `pantree.food` → CTA: `"pan-tree dot food"`
- Visual context: domestic kitchen, warm light, relatable home cook
- Default Soul ID: train one female persona in early-late 20s
- Marketing Studio: type=webproduct

### Mr. White
- Pronounce: `mister white` (never abbreviate to "Mr.")
- Visual context: dimmed living room, friends on couch, party energy
- Default Soul ID: male host/MC persona, charismatic + deadpan
- Marketing Studio: type=webproduct
- Energy: comedic chaos, real reactions, never staged

---

## TL;DR — 12 Rules

1. Brief first. Always paste `BRIEF.md` at the start of every session.
2. Train one Soul ID per brand. 15+ recent, well-lit, varied-angle photos.
3. Set up Marketing Studio once per product via URL fetch.
4. Generate 6 hooks — one per archetype. Don't generate one ad, generate one framework.
5. Build every prompt with MCSLA: Model · Camera · Subject · Look · Action.
6. Number shots with timestamps. Seedance executes exactly what you write.
7. Add anti-slop phrases. `"no 3D, no plastic, 35mm grain, ARRI ALEXA"`.
8. Spell brand names phonetically. `"pan-tree dot food"`, `"mister white"`.
9. Generate 4 variants, ship 1–2. Reject more than you ship.
10. Test hook variations on the winning body. Same body, 5 different Shot 1s.
11. Run AI feedback before shipping. Claude then Gemini for QA.
12. Warm up accounts before posting. Engage 10 min/day for 1–2 weeks first.
