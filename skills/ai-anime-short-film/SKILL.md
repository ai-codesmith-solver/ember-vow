---
name: ai-anime-short-film
description: End-to-end workflow for producing a short anime film (or any character-driven short) from AI-generated stills and image-to-video clips, then cutting it into a finished film. Use when the task is "make an anime short / AI short film / multi-shot character video" with Gemini (or similar) for images + Google Flow / Omni Flash (or similar) for video + a video editor. Covers script → character sheet → per-shot stills → clips → edit, and the consistency discipline that makes it hold together.
---

# Making an AI anime short film, end to end

Distilled from the *Ember Vow* experiment (14 shots, ~1 min, one character). This is the process
skill — the "what order, and why." For the two sub-crafts it depends on, see [[ai-anime-prompting]]
(per-shot prompt structure) and [[cinematic-video-finishing]] (the edit). Empirical findings behind
every step are in the project's `Report/`.

## The one idea that makes it work

The video model has **no memory between shots** — every clip is generated from scratch, so left
alone the character looks like a different person each shot. The entire method is built to defeat
that: **lock one character reference image, and feed it into every single generation.** Everything
below serves that.

## The pipeline

```
1. STUDY + SKILLS   Gather references; load prompting skills. Decide the style anchor string.
2. SCRIPT           14-ish shots, ~4s each. Design AROUND model weaknesses (see §Design rules).
3. CHARACTER SHEET  Generate one locked reference (turnaround + expressions). Pick by CONSISTENCY.
4. SHOT STILLS      One still per shot. Attach the character sheet to EVERY still. Judge vs sheet.
5. CLIPS            Animate each still separately (image-to-video). Motion-only prompts. Re-roll bad ones.
6. EDIT + FINISH    Assemble, then apply the cinematic finishing pass. Add captions/title. Add music.
```

## Design rules for the script (route around the failure modes)

These are not creative preferences — they are what keeps the project feasible:

- **One character.** Crowds and multi-character continuity are the least reliable thing these models
  do. A single protagonist minimises the drift surface.
- **Elemental / energy VFX, not physical contact.** Fire, aura, embers, shockwaves are a strength;
  punches landing, bodies reacting, force transfer are a hard failure. Build the "action" from
  effects, not impact.
- **~4s per shot.** Coherent motion holds ~2–3s; 4s is clean, longer gets mushy. Cut long from short.
- **A silent, visual arc.** Assume no dialogue. Tell the story in pictures + a few caption cards.

## Step 3 — character sheet (the anchor)

- Generate a turnaround (front / 3-4 / side / back) **plus an expression row**, on a plain evenly-lit
  background. See [[ai-anime-prompting]] §2 and the `video-prompting` character-sheet workflow.
- Try 2–3 image models; **pick the winner on "same person in every panel," not on beauty.** A plainer
  on-model sheet beats a prettier one that drifts. This choice also picks your image model for the
  whole film.
- Save it; this file is attached to every downstream generation.

## Step 4 — shot stills

- One still per shot. **Attach the character sheet every time.** Name the identity anchors verbatim
  in the prompt (hair + accessory, eyes, outfit) and add the fixed style-anchor string.
- Cinematic finishing vocabulary belongs on the shot stills (lens/DOF/grain phrasing), **not** on the
  character sheet (which must stay clean and evenly lit).
- Judge each still **against the sheet**, not just the shot description. Regenerate off-model stills.

## Step 5 — clips (animate stills separately)

- **Separate, not batched.** One vetted still → one clip, using the editor's first-frame / "Frames"
  mode so the clip starts from the exact still. Tested: batching multiple shots into one timecoded
  call is non-deterministic and discards the vetted stills (`Report/analyses/pilot_method_A_vs_B.md`).
- **Prompt motion + camera only** — the still already locks identity. Add a "keep the design and art
  style exactly the same, no change to appearance as the clip plays" line.
- **Close-ups + heavy VFX are highest risk.** Budget 2–3 attempts; re-roll *isolated* shots. Don't
  assume a re-roll improves things — verify by frame inspection (see below).
- **Negative constraints are your main fix.** When a face hardens or a glow fades, negate it
  explicitly: "do not make the face heavier/older/masculine," "the glow must NOT fade or turn cold."

## Step 6 — edit and finish

Hand off to [[cinematic-video-finishing]]. In short: assemble in order, then a single finishing pass
(grade + letterbox + grain + vignette), a sparse caption arc + title card, dip-to-black act breaks,
open/close fades. This is the largest single quality jump in the whole project — treat it as a
production stage, not a formality.

## Verification discipline (applies at every stage)

- **Inspect frames, never playback.** Extract frames, crop into the face/contact point, judge there.
  Drift is invisible at thumbnail size.
- **Keep every intermediate** (stills, clips, versions). A failed later shot is recovered from an
  earlier clean one.
- **Declare the pass condition before you look:** "same person as the sheet, yes/no" beats "does this
  look good."

## What to expect (calibration)

- ~12 of 14 shots on-model without intervention is a realistic outcome with the sheet workflow.
- Residual drift is detail-level (scar, hair tone under colour, background), not wholesale.
- Non-determinism is structural — plan for re-rolls, not for one-pass perfection.
