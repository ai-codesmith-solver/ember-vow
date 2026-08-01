---
name: ai-anime-prompting
description: Write prompts for generating anime-style video with Gemini Omni Flash (Google Flow), especially for multi-shot short films built from a script → per-shot image → image-to-video pipeline. Use when styling a shot as anime, building a character sheet for cross-shot consistency, or planning a multi-clip film where identity/style drift between shots is the main risk.
---

# Prompting Omni Flash for anime-style shorts

Research synthesis, not a hands-on evaluation — unlike [[ai-vfx-prompting]], which is derived from a
20-generation frame-by-frame test. Treat the claims here as **hypotheses to verify** against your
own 14-shot run, not settled findings. Where this skill and the VFX report agree, trust it more;
where your own results disagree, trust your results and update this file.

## 0. The determinism problem is the whole experiment

[[ai-vfx-prompting]] and its underlying report found, from direct testing: **shot-to-shot character
continuity is a no-go — identity drifts every pass**, because the model re-renders the entire frame
on every invocation with no layer isolation. Independent practitioner testing (Greg Preece, testing
Omni Flash shorts on Higgsfield) corroborates the same shape of problem one level down — audio
repeating/dropping words on a *single* 7-second clip, i.e. non-determinism even within one
generation, not just across generations.

A 14-shot, 1-minute film is 14 independent chances for the model to reinterpret your character.
**Expect drift. The question worth answering is how much, and whether the mitigations below reduce
it or just mask it.** Budget review time per shot for exactly this, not just for "does it look
anime enough."

---

## 1. What Omni Flash actually offers for consistency

- `reference_to_video`: accepts **up to 7 reference images** and **up to 3 short video clips**,
  using them as visual ground truth rather than suggestions. Documented uses: **style transfer,
  character consistency, storyboard guidance** — exactly your three needs.
- Conversational/iterative editing: "make this anime, keep everything else the same" holds the
  rest of a *single* shot steady. This does not extend across separate shot generations — each new
  4-second clip is a fresh invocation, not a continued edit.
- Official 6-part prompt framework: **shot framing/motion, style, lighting, location, action, text
  rendering.**

The practical implication: **your character sheet is the only cross-shot memory the model has.**
Nothing else persists between the 14 generations except what you feed back in as a reference image
or repeat verbatim in text.

---

## 2. Build a character sheet before shot 1

This is the single highest-leverage step for your experiment, and it's a distinct discipline from
building the film itself:

1. Generate a character lineup / model sheet first: front, 3/4, side, one or two expressions —
   same character, same outfit, same palette, plain background.
2. Lock in writing what must not change: **face shape and eyes, hairstyle and hair colour, outfit
   and accessories, body proportions, colour palette, art style/line quality.** This exact list,
   repeated verbatim, is what recurring practitioner guidance converges on.
3. Pick the single best sheet (your ChatGPT-vs-Gemini bake-off) before generating any of the 14
   shots. Re-rolling the sheet mid-production re-rolls every downstream shot's anchor.
4. Feed that sheet image into every one of the 14 `reference_to_video` calls, not just shot 1.
   Re-supplying it every time is the mechanism, not a one-time setup step.

## 3. Style anchor: one string, reused verbatim

Keep a single style-anchor sentence in a snippets file and paste it into **every one of the 14
prompts**, unchanged:

> 2D anime, cel-shaded flat colour, hand-drawn line work, [your palette]. Not 3D CG, not
> photoreal, not generic anime — [name the specific reference: e.g. "Studio Ghibli hand-painted
> backgrounds" or the visual language of your 5 reference clips].

"Hand-drawn line work" or a named reference (Ghibli, Satoshi Kon, 90s shōnen) prevents the default
drift toward generic anime CG. Vague adjectives ("anime style," "cool," "epic") are exactly what
causes inconsistent faces and mid-clip style jumps, per multiple independent sources.

## 4. Per-shot prompt structure

Adapted from [[ai-vfx-prompting]]'s five-block structure for the anime/no-real-plate case:

```
1. CONTINUITY   same character, outfit, hairstyle, face, proportions, palette, art style as
                the reference sheet — carried from the previous shot, no visual reset
2. SUBJECT      what happens in this shot, concretely — not mood adjectives
3. MOTION       camera + character motion, separated from style: "she turns toward the window,
                hair drifting" — not "beautifully animated"
4. STYLE ANCHOR the exact string from §3, verbatim, every shot
5. NEGATIVES    "not 3D, not photoreal, no visual reset, no new character design"
```

Multi-shot risk factors called out across sources, worth flagging per-shot before you generate:
fast turns, complex gestures, fighting/action, crowds, and long single-shot duration all raise
drift risk more than a static or slow-moving shot. Your fight-adjacent shots (if any, per the VFX
report's §4.5 finding on force response) are the highest-risk shots in the film — plan review time
accordingly.

## 5. Workflow for the 14-shot pipeline

> **Animate stills SEPARATELY — one vetted still → one clip. Do NOT batch multiple shots into one
> timecoded call.** Tested directly (Ember Vow, shots 1–3): separate image-to-video clips held
> identity (each starts from an approved still) and were reliable on the first try; batched
> timecoded generation was non-deterministic (dropped a directed beat, drifted the eyes) and
> ignored the vetted stills. See [[gemini-omni-prompts]] "Batched timecoding vs. separate clips".

1. Script → 14 shot descriptions (you're already doing this).
2. Character sheet first (§2), picked once, reused everywhere.
3. Per shot: generate stills in both ChatGPT and Gemini, pick the best **against the character
   sheet, not against the shot description alone** — a beautiful image that drifts off-model is a
   worse pick than a plainer one that holds.
4. Per shot: image-to-video from the shot's approved still (Method A), prompting **motion + camera
   only, not identity** (the still already locks identity). Use Flow's "Frames" mode so the clip
   starts from the exact still.
5. Keep every intermediate still and clip. If shot 9 drifts, you need shot 8's clean state to
   re-anchor from, per [[ai-vfx-prompting]] §6's "keep every intermediate generation" rule.
6. Cut in CapCut. Judge on frames, not playback — extract a frame from each clip at the same
   relative timestamp and lay them side by side before trusting the edit.

## 6. Before you generate, per shot

- Does this shot's prompt include the character sheet as a reference image, not just in text?
- Is the style-anchor string pasted verbatim, not paraphrased?
- Is this a high-drift shot (fast motion, fighting, crowd)? If so, plan for 2-3 attempts, per
  Greg Preece's practitioner finding of budgeting multiple generations per clip.
- What's the pass condition for "this shot matches the character sheet," stated before you look —
  not "does this look good."

## 7. After the film is cut

Compare every shot's character frame against the character-sheet frame, side by side, not just
against its own neighbours. This is the actual data point your experiment is producing: a
measured drift curve across 14 independent generations, corroborating or refuting
[[ai-vfx-prompting]]'s finding from a different domain.

---

## Sources

- Own prior work: [[ai-vfx-prompting]], `VFX_AI_CAPABILITY_REPORT.md` (20-generation frame-by-frame
  test of Omni Flash, same model)
- Greg Preece, "Gemini Omni on Higgsfield: I Tested One-Click Shorts" — gregpreece.com
- Runware docs, "Reference-driven video with Gemini Omni Flash" — runware.ai
- Truescho, "Gemini Omni Character Consistency 2026" — truescho.com
- AutoWeeb, "How to Create Multi-Character Anime Scenes Using Character Sheets" — autoweeb.com
- Elser AI, "Best Character Consistency Prompts for AI Video" — elser.ai
- promptslove.com, "Google Omni Prompting Guide"
