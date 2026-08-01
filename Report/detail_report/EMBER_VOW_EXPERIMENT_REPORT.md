# Can Generative AI Make a Consistent Anime Short Film?

### A structured, end-to-end experiment: 14 shots, one character, ~1 minute — built with Gemini, Google Flow (Omni Flash), and ffmpeg

**Date:** 1 August 2026
**Tools under test:** Gemini (image generation) · Google Flow, model *Omni Flash* (image-to-video) · ffmpeg (edit/finishing)
**Deliverable:** *Ember Vow* — a ~1-minute cel-shaded anime teaser, 16:9 cinemascope
**Author profile:** an AI-tools researcher, **not** a professional animator — deliberately, to test how far a non-artist can get with AI alone
**Method:** shot-by-shot production with frame-level inspection against a pre-declared consistency criterion

---

## 1. Executive summary

This experiment set out to answer a pointed question: **can current generative video hold a single
character consistent across many independent shots — enough to assemble one coherent short film?**
A companion study (§7) had rated shot-to-shot character continuity a structural **"no-go."** This
project re-tested that in animation, under one deliberate mitigation: **a locked character
reference sheet fed into every generation.**

The result is more encouraging, and more precise, than the prior "no-go":

- With a locked character sheet, the protagonist read as **the same person in roughly 12 of 14
  shots** without intervention — a measurable recovery of a capability previously called impossible.
- The remaining drift was **small and detail-level** (a scar's size, hair tone under coloured light,
  a background) rather than wholesale identity loss.
- **Non-determinism is real and unavoidable.** The same prompt produced materially different results
  run to run; two independent sub-tests (a shot's re-rolls, and a batched-vs-separate pilot) both
  showed better→worse variance. The working method is to re-roll *isolated* shots, never to trust a
  single pass.
- The single largest quality jump came not from generation but from **editing**: a cinematic
  finishing pass (grade, letterbox, film grain, vignette, caption cards, transitions) is what turned
  "AI clips" into something that reads as a film.

The honest headline: **a reference-image workflow converts character consistency from a hard "no-go"
into a manageable, mostly-solved problem — with a residual detail-drift and a per-generation variance
you manage by design, not by hope.**

---

## 2. Background and the question

Generative video models render *what things look like*, not *how things behave or persist*. Two
consequences dominate any attempt to make a film with them: (a) **continuity** — a character
re-rendered from scratch each shot tends to drift; and (b) **physical consequence** — impacts,
contact, and force are unreliable. A prior black-box VFX evaluation (§7) found both independently.

Rather than fight those weaknesses, this experiment was **designed around them**:

- **One character**, never a crowd — minimising continuity surface.
- **Elemental / energy VFX** (an igniting ember-aura) instead of physical combat — playing to a
  documented strength and avoiding the "no-go" of physical impact.
- A **"power awakening" arc** — a lone warrior crosses a dead battlefield, recovers a fallen blade,
  and a dormant power ignites — which needs no second character and no contact.

The story is, in effect, a filter that routes around the model's failure modes.

---

## 3. Method — the production pipeline

| Stage | Tool | Output |
|---|---|---|
| 1. Research & skills | web + prompt libraries | reusable prompting "skills" |
| 2. Script | — | 14-shot, ~1-minute shot list |
| 3. Character sheet | Gemini | one locked reference image (turnaround + expressions) |
| 4. Shot stills (×14) | Gemini | one still per shot, **character sheet attached each time** |
| 5. Animation (×14) | Google Flow / Omni Flash | one ~4s clip per still (image-to-video) |
| 6. Edit & finish | ffmpeg | the assembled, graded, captioned film |

**Consistency discipline:** every still was judged against the character sheet (not merely against
its shot description) before use; every clip was inspected **frame-by-frame, cropped into the face**,
against a pre-declared "is this the same person, yes/no" criterion. Drift is invisible at playback
size and obvious cropped in.

---

## 4. Results

### 4.1 Character sheet — the anchor (PASS)
Three image models were compared. Selection was made on **cross-panel sameness**, resolution, and
correct character read — not aesthetics. The winner (Gemini) held the same face across all nine
panels; two rejected candidates each failed distinctly (one read masculine, one was low-resolution
and drifted between panels). This choice also fixed the image model for the remainder of the project.

### 4.2 Shot stills — consistency largely held (PASS, with detail drift)
**~12 of 14 stills read as clearly the same character.** Identity anchors — braid with red cord,
amber eyes, grey-blue cloak — survived reliably. Drift appeared only in fine detail: a brow scar
rendered larger and mirrored (shot 3), hair tone shifting under warm light (shot 10, regenerated),
a background wandering off-location (shot 14). This directly contradicts the prior "no-go" and
isolates the mitigation responsible: the attached reference sheet.

### 4.3 Animation — motion held; close-ups under heavy FX were the failure surface
Wide and medium shots animated on-model on the first generation. The failures clustered in **tight
close-ups combined with intense effects**:

- **Within-clip identity drift (shot 10):** a close-up face **masculinised across 4 seconds**
  (heavier jaw, thicker brow). A negative-constraint rewrite fixed the morphing; two further
  re-rolls returned *worse* results — a clean demonstration of non-determinism.
- **Emotional / photographic drift (shot 14, the closing shot):** identity held, but the warm glow
  **faded to cold** and the intended smile relaxed to neutral, weakening the ending. A rewrite
  forcing both to "hold, not fade" corrected it in one pass.

### 4.4 Method pilot — separate beats batched (decisive)
A controlled sub-test compared **(A)** three separate image-to-video clips versus **(B)** one batched
timecoded generation of the same three beats. **Separate won decisively.** Batched generation was
non-deterministic across three runs (fail → pass → fail), dropped a directed action, drifted a
close-up's eye colour, and discarded the already-vetted stills. Full data in
[`../analyses/pilot_method_A_vs_B.md`](../analyses/pilot_method_A_vs_B.md).

### 4.5 Edit & finishing — the largest quality lever (STRONG PASS)
Assembled raw, the clips read as "stitched AI clips." A single ffmpeg finishing pass transformed
them into film-grade footage:

- **Cinemascope letterbox** — the strongest single "this is cinema" signal.
- **Filmic colour grade** — cool teal in the cold act, rich contrast in the fire act; unifies 14
  separately-generated shots into one graded look.
- **Film grain + vignette** — texture and focus; breaks the flat "digital AI" surface.
- **Caption cards + dip-to-black act breaks + open/close fades** — narrative spine and editorial
  rhythm for a piece with no dialogue.

This mirrors the companion study's strongest positive result: **finishing vocabulary is a larger
quality determinant than subject matter.**

---

## 5. Findings that matter

1. **A locked character sheet is the decisive mitigation.** It converted continuity from "no-go" to
   "mostly solved" — the central, actionable result of this experiment.
2. **Drift is now detail-level and residual, not wholesale.** Anchors survive; scar, hair-tone,
   background wobble. Budget for it; re-roll where it matters.
3. **Non-determinism is structural.** Same prompt, different run, different result — confirmed twice
   over. Manage it by re-rolling isolated shots and keeping every intermediate; never trust one pass.
4. **Close-up + heavy VFX is the high-risk zone.** Concentrate review effort there.
5. **Negative constraints are the primary control surface.** The fixes that worked were explicit
   negations of the observed drift.
6. **Editing is not a footnote.** The finishing pass delivered the largest single jump in perceived
   quality — the difference between "AI output" and "a film."

---

## 6. Limitations

- **Sample size:** one film, 14 shots, one character, one art style. Findings are indicative, not
  statistical.
- **Subjective consistency metric:** "same person, yes/no" by frame inspection, not a quantitative
  identity-distance measure.
- **Toolchain specifics:** results are tied to Gemini + Omni Flash at this date; the editing used an
  old local ffmpeg build whose missing filters (`drawtext`, `xfade`) shaped the technique (PNG-overlay
  captions; dip-to-black transitions).
- **No audio evaluation:** the deliverable was finished silent; scoring/music was out of scope.

---

## 7. Relationship to the prior VFX capability study

This experiment is a deliberate second test of a finding first produced by black-box VFX evaluation
(`VFX_AI_CAPABILITY_REPORT.md`, same folder), which concluded that generative models are
**non-deterministic by construction** and therefore structurally unsuited to deterministic,
continuity-bound finishing work. *Ember Vow* confirms that determinism finding from an independent
direction — **and** demonstrates that a reference-image workflow meaningfully mitigates (not
eliminates) the continuity symptom. The two studies agree on the underlying mechanism and refine the
practical boundary: continuity is manageable with the right anchor; per-generation variance is not.

---

## 8. Conclusion

A non-animator, using only AI tools plus a video editor, produced a ~1-minute anime short in which a
single character stays recognisably herself across 14 independently generated shots and the whole
reads as an intentional film. That was not achievable by naive prompting — it required three
disciplines: **(1) a locked character reference used everywhere, (2) a story engineered around the
model's known failure modes, and (3) a real cinematic finishing pass in the edit.** The value of the
result is not that it is flawless. It is that it maps, concretely and reproducibly, **where AI now
holds and where it still drifts** — which is exactly the knowledge a team needs to decide what to
hand to these tools and what to keep in human hands.

---

## 9. Reproducing this

The full pipeline is public and repeatable: the script (`SCRIPT.md`), every prompt (`prompts/`), the
reusable skills (`skills/`), the character sheet and shot stills (`assets/image-scene/`), the
generated clips (`assets/vedio_clips/`), and the finished film (`assets/full_vedio/Ember_Vow.mp4`).
See the repository README for the step-by-step guide.
