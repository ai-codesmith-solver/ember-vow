# Analysis — Ember Vow (AI anime short, consistency experiment)

**Date:** 1 Aug 2026
**Analyst view:** stage-by-stage breakdown of where AI held and where it drifted, across a
14-shot / ~1-minute anime short generated with Gemini (images) + Google Flow *Omni Flash*
(image-to-video), then finished in ffmpeg.

Companion documents: the method pilot in [`pilot_method_A_vs_B.md`](pilot_method_A_vs_B.md), the
practical takeaways in [`../learning_from_this_experiment/`](../learning_from_this_experiment/), and
the publishable write-up in [`../detail_report/`](../detail_report/).

---

## 1. The question, restated as a measurement

Prior black-box testing (the VFX capability study in `../detail_report/`) rated **shot-to-shot
character continuity a flat "no-go."** This experiment re-tested that claim in a new domain
(animation, not VFX compositing) and under a specific mitigation: **a locked character sheet fed
into every generation.** The measurable question: *how many of 14 independent shots keep the
protagonist recognizably the same person?*

---

## 2. Stage-by-stage analysis

### 2.1 Character sheet — the consistency anchor
Three image models were tried for the sheet (ChatGPT, Gemini, a third). **Gemini won** on the one
axis that matters for a reference: the *same person* in every panel, cleanest linework, highest
resolution, and clearly-female read. Two rejected candidates each failed a specific way — one read
masculine/androgynous, the other was low-res and drifted between expression panels. **Choosing the
sheet also chose the image model for the whole project**, and Gemini's per-panel consistency was
predictive of its downstream behaviour.

### 2.2 Shot stills (14) — consistency held, drift lived in details
Reviewing all 14 stills against the sheet: **~12 of 14 read as clearly the same character.** This
already contradicts the "no-go" prior — the reference-image workflow measurably recovered identity.
Where it broke, it broke *small*:

| Shot | Drift observed |
|---|---|
| 03 | Brow scar rendered much larger and on the mirrored side vs. the sheet |
| 10 | Hair drifted to brown, face rounder — required regeneration |
| 14 | Background wandered to stone ruins instead of the battlefield |

The pattern: **identity anchors (braid + red cord, amber eyes, cloak) survived; fine details (scar
size/side, hair tone under coloured light, background) drifted.**

### 2.3 Video clips (14) — motion held, close-ups under heavy FX were the weak point
Animating each vetted still into a ~4s clip (image-to-video) inherited the still's identity well.
Wide and medium shots were reliably on-model on the first generation. **The failures clustered in
close-ups combined with intense effects:**

- **Shot 10** exhibited *within-clip drift*: the face started as Yura and **masculinised over 4
  seconds** (heavier jaw, thicker brow). A negative-constraint rewrite ("do not make the face
  heavier/older/more masculine as the clip plays") fixed the *morphing* but left the face harder
  than neighbouring close-ups. Two further re-rolls came back worse — a direct demonstration of
  non-determinism.
- **Shot 14** (the closing shot) drifted *emotionally/photographically*, not in identity: the warm
  glow **faded to cold** and the intended smile relaxed to neutral — undercutting the ending. A
  rewrite forcing the glow and smile to "hold, not fade" fixed it in one pass.

### 2.4 Method pilot — separate beats batched (see companion doc)
Same prompt, three batched runs, three different outcomes (fail / pass / fail). Separate
image-to-video from vetted stills won decisively on consistency, control, and reliability. Detailed
in [`pilot_method_A_vs_B.md`](pilot_method_A_vs_B.md).

### 2.5 Edit / finishing — the largest single quality jump
The raw assembled clips read as "stitched AI clips." A one-pass ffmpeg finish — **color grade +
cinemascope letterbox + film grain + vignette + caption cards + dip-to-black act breaks + fades** —
was the moment it started reading as a *film*. This corroborates the VFX study's strongest positive
finding (§4.6 there): the finishing/DI vocabulary is a bigger quality lever than subject content.

---

## 3. Quantified outcome

- **Identity retention:** ~12/14 shots on-model without intervention; 2 required targeted
  regeneration (shots 10, 14), 1 cosmetic drift left as-is (shot 3).
- **Non-determinism, observed directly:** shot-10 re-rolls (better → worse → worse) and the batched
  pilot (fail → pass → fail) both show the same prompt yields materially different results run to run.
- **Highest-risk shot type:** tight close-up + intense VFX (shots 10, 14).
- **Lowest-risk shot type:** wide/medium with a resting or slow subject.

---

## 4. Interpretation

The headline is not "AI can now make consistent films." It is narrower and more useful: **a locked
reference image converts character continuity from a hard "no-go" into a *manageable, mostly-solved*
problem — with a residual, irreducible drift in fine details and a real per-generation variance that
you manage by re-rolling isolated shots, never by trusting a single pass.** The remaining failure
surface is exactly what the earlier VFX study predicted (details drift; every invocation
re-renders), now confirmed from a second, independent direction.
