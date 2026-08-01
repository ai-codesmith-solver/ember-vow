---
name: gemini-omni-prompts
description: Curated example prompts for Google Gemini Omni Flash covering anime/stylized animation, image-to-video, and multi-scene character-consistent stories. Use as a phrase bank when drafting a new Omni Flash prompt, or when you need a concrete worked example rather than an abstract rule.
---

# Gemini Omni Flash — example prompt bank

Pulled from [Anil-matcha/Awesome-Gemini-Omni-API-Prompts](https://github.com/Anil-matcha/Awesome-Gemini-Omni-API-Prompts).

**Caveat on the source:** that repo is written to promote a paid third-party API reseller (MuAPI) for
programmatic Omni Flash access — the `pip install` / Python code samples in the original README are
for that reseller's SDK and don't apply if you're using Omni Flash through Google Flow's own UI
directly. Only the **prompt text itself** was pulled here; it transfers regardless of which surface
you're generating from.

## Batched timecoding vs. separate clips — TESTED, result below

The "Three-Scene Mini-Film" example below asks Omni Flash to generate **multiple timecoded shots in
one single call** (e.g. 0–4s, 4–7s, 7–10s), from one reference image, expecting identity to hold
because of the model's long context. The hypothesis was that batching might hold consistency *better*
than separate calls.

**This was tested directly (Ember Vow project, 1 Aug 2026, shots 1–3, Omni Flash image-to-video).
The hypothesis was WRONG for an image-to-video pipeline that starts from pre-vetted stills. Separate
clips won.** Findings:

- **Separate (one vetted still → one 4s clip each):** all 3 clips good on the first try. Identity
  held (amber eyes, scar, braid all correct) because each clip *starts from a still you already
  approved*. Framing is exactly as designed. A bad shot can be re-rolled in isolation.
- **Batched (one still → one 10s timecoded clip of 3 beats):** **non-deterministic and lossy.** Roll
  1 silently dropped an entire directed beat (the "look down at the object" action never happened)
  and drifted the close-up eyes from amber to brown. Roll 2 (same prompt) fixed both — so it *can*
  work, but takes 2–3 attempts to land, and re-rolling to fix one bad beat also re-rolls the good
  ones. Roll 3 was wrong again. It also **ignores your vetted stills entirely**, regenerating from
  scratch and reintroducing drift you already eliminated at the image stage.

**Rule of thumb:** if you have already generated and approved per-shot stills, animate them
**separately** (image-to-video, one still per clip). Batched timecoding only makes sense when you do
NOT have per-shot stills and want the model to invent a continuous sequence — and even then, budget
2–3 re-rolls per keeper. This corroborates [[ai-vfx-prompting]]'s non-determinism and "generate
short, cut long" findings from a new angle.

---

## Anime & Stylized Animation

### Studio Ghibli Countryside Open
> Studio Ghibli style hand-painted animation, a young girl in a yellow sundress runs through a
> sunlit field of tall grass, hair and dress flowing behind her, dandelion seeds float through the
> air, fluffy clouds drift across a deep blue sky, the camera pulls back to reveal a wooden
> farmhouse in the distance. Soft pastel palette, gentle orchestral score, 10 seconds, 16:9,
> traditional cel-animation aesthetic.

### 90s Shōnen Transformation Sequence
> Late-90s shōnen anime aesthetic, dramatic transformation: a teenage hero stands on a cliff, wind
> whipping his hair, energy aura begins to glow blue around him, ground cracks, debris floats
> upward, he yells skyward, lightning streaks across the storm clouds, his hair shifts to gold in a
> final freeze-frame pose. Hand-drawn linework, speed-line streaks, cel-shaded color, 8 seconds, 4:3.

### Cyberpunk Anime City Drift
> Anime in the style of Satoshi Kon's "Paprika": dense neon Tokyo skyline at night, camera floats
> forward between skyscrapers, holographic koi fish swim through the air, rain falls upward in slow
> motion, a girl in a red coat stands on a rooftop watching it all, her scarf trailing in the wind.
> Painterly backgrounds, vivid saturated palette, dreamlike pacing, 10 seconds, 21:9 cinemascope.

### Pet Portrait → Anime Episode (image-to-video)
> Convert the attached pet photo into a cel-shaded anime snapshot, then animate it as if it were a
> clip from a Studio Ghibli film. The pet looks toward camera, ears twitch, tilts its head
> curiously, blinks, soft wind ruffles fur. Hand-painted background of a sunlit meadow with floating
> dandelion seeds. Maintain the pet's exact coat colors and markings. 6 seconds, 16:9.

---

## Character Consistency & Multi-Scene Stories

### Three-Scene Mini-Film From One Reference
> The attached image is the main character. Generate three connected shots, same person in all
> three, identical face, hair, and wardrobe throughout:
> 1) 0–4s: Wide shot, she walks out of a Parisian apartment building into morning light, holding a
>    paper coffee cup, slow dolly-back
> 2) 4–8s: Medium shot, she stops at a flower stall, smiles at the vendor, picks up a bouquet, soft
>    sidelight
> 3) 8–12s: Close-up, she sits on a bench by the Seine, looks off-camera contemplatively, gentle
>    breeze in hair
> Cinematic 35mm look throughout, consistent color grade, no cuts to other characters.

### Hero Walking Cycle Across Environments
> Using the attached character as the subject, generate a continuous walking sequence where the
> environment transitions seamlessly every 3 seconds: 0–3s desert dunes at sunset, 3–6s
> neon-soaked rainy city street, 6–9s misty alpine forest, 9–12s clean white infinity studio.
> Character stride, outfit, and proportions remain identical throughout. Camera tracks alongside in
> profile, locked focal length, 12 seconds, 16:9.

### Same Outfit, Five Locations Lookbook
> Use the attached photo as the model. Generate a fashion lookbook video where she wears the exact
> same outfit across five locations, 2 seconds each: rooftop helipad, marble museum hallway, neon
> arcade, desert highway, foggy pine forest. Static elegant pose with subtle wind motion, identical
> outfit details and styling in every shot, smooth match-cuts on her silhouette between locations.
> 10 seconds, 9:16 vertical.

*Pattern across all three: name the fixed identity elements explicitly ("identical face, hair, and
wardrobe" / "stride, outfit, and proportions remain identical"), then timecode each beat within the
same prompt rather than issuing separate prompts.*

---

## Image-to-Video (Animate a Still) — general patterns worth reusing

### Subtle Portrait Reanimation
> Using the attached image as the first frame, animate the subject with subtle natural motion:
> gentle breathing, slight hair movement from a soft breeze, eyes blink twice with realistic
> timing, micro-expression shift from neutral to a faint smile, camera holds locked-off, 5 seconds,
> photorealistic, do not change facial structure or clothing.

### Painting Comes Alive
> Treat the attached painting as a living scene. Animate elements naturally within the original
> brushstroke style — clouds drift, foliage sways, water ripples, fabric moves with the wind, any
> animals breathe. Camera performs a slow 5% push-in. Preserve the original color palette, lighting,
> and painterly texture. 8 seconds, 16:9.

*Both explicitly state a negative constraint ("do not change facial structure," "preserve the
original... texture") — consistent with [[ai-vfx-prompting]] §4's finding that negative constraints
are the main lever against drift.*
