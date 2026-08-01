# Prompt 01 — Yura character sheet (the reusable "reference photo")

**Skills used:** `video-prompting/references/workflows/character-sheets.md` (turnaround-plus-expressions
pattern, neutral-background / even-lighting rules) + `ai-anime-prompting` §2 (anime identity locks).

**What this is for:** generate ONE reference image of Yura. You'll attach this same image to all 14
shots later so the AI copies her look every time. Make it in both ChatGPT and Gemini, pick the best
one, and save it to `assets/image-scene/` as `yura_character_sheet.png`.

**Why no fire/glow/film-grain here:** a reference photo needs to be clean and evenly lit so the AI
can read her design clearly. The cinematic effects come later, on the shots — not on the sheet.

---

## PROMPT (paste this into ChatGPT and Gemini image generators)

```
Create a clean anime character turnaround sheet for a single 2D cel-shaded anime character,
hand-drawn line work.

The character is Yura, a lean adult female warrior, mid-20s, weathered and battle-worn. Dark hair
in a single long braid tied with one red cord, a pale scar cutting through her left eyebrow, sharp
amber eyes, calm guarded expression.

Outfit: a weathered grey-blue hooded traveling cloak worn open over dark fitted armor wraps and
leather straps, cloth bindings on the forearms, worn boots. One plain straight sword in a dark
sheath at her hip. Muted cold colour palette — grey-blue, charcoal, dark leather — with the single
red hair cord as the only warm accent.

Show the exact same character in full body from four angles: front view, 3/4 view, side profile,
and back view. Above them, include an expression row of the same face showing five clearly distinct
emotions: neutral calm, guarded wariness, fierce determination (open mouth, shouting), wide-eyed
surprise, and a faint quiet smile.

Keep facial proportions, hairstyle, braid, scar, eye colour, outfit, colours, and body proportions
completely identical across every panel. Clean model-sheet presentation, evenly lit, neutral pale
grey studio background, no scene elements, no props other than the sword, no extra characters, high
detail.
```

---

## After you generate it

1. Generate in **both** ChatGPT and Gemini.
2. Pick the best one — judge it on: is she clearly the *same person* in every panel, and is her
   design (braid + red cord, brow scar, grey-blue cloak, amber eyes, sheathed sword) clear and
   readable? A plainer image that stays on-model beats a prettier one that drifts.
3. Save the winner to `assets/image-scene/yura_character_sheet.png`.
4. Tell me it's ready — then I'll write the shot-by-shot image prompts (shots 1–3 first, for the
   consistency pilot test).
```
