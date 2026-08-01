# Pilot finding: separate clips vs. batched timecoding (Omni Flash, image-to-video)

**Date:** 1 Aug 2026
**Project:** Ember Vow (14-shot anime short)
**Test:** shots 1–3, Google Flow / Omni Flash, image-to-video, 16:9, 720p, ~4s (separate) / 10s (batched)
**Method:** frame-by-frame inspection (2 fps extraction), eyes cropped and compared against the
character sheet's amber-eye anchor.

## Question

Does batching multiple shots into one timecoded Omni Flash call (0–4s / 4–7s / 7–10s from one
reference image) hold character identity **better** than animating each pre-vetted still separately?
The `gemini-omni-prompts` skill hypothesised batching might win.

## Result — separate clips win, decisively for this pipeline

| Method | Beat-2 directed action | Close-up identity | Reliability |
|---|---|---|---|
| **A — separate** (still → 4s each) | ✅ performed (looks down at object) | ✅ amber eyes, scar, braid held | All 3 good on first try |
| **B — batched** roll 1 | ❌ dropped entirely | ❌ eyes drifted amber→brown, scar gone | Bad |
| **B — batched** roll 2 | ✅ performed | ✅ amber eyes held | Good, but 2nd attempt |
| **B — batched** roll 3 | — | ❌ wrong again | Bad |

## Why separate wins here

1. **It inherits vetted work.** Each Method-A clip starts from a still we already reviewed and
   approved against the character sheet, so identity is locked before motion is added. Method B
   regenerates from scratch and reintroduces drift already eliminated at the image stage.
2. **Directability.** Method A produces exactly the designed framing (wide → medium → close-up) and
   the directed action. Method B collapsed beats 1–2 into one static wide and skipped the look-down.
3. **Isolated fixes.** A bad Method-A shot is re-rolled alone; a bad beat in a Method-B batch forces
   re-rolling the whole 10s, risking the beats that were already good.

## The transferable finding

Same batched prompt, three runs, three different outcomes (fail / pass / fail) — **non-determinism**,
the same defect the [VFX capability report](../detail_report/VFX_AI_CAPABILITY_REPORT.md) found in a
different domain. Notably, character consistency across shots — a flat "no-go" in the VFX report —
was **substantially recovered here** by (a) locking a character sheet and (b) animating pre-vetted
stills separately. That is the headline: the reference-still workflow is a real, measurable
mitigation for the identity-drift problem, not a guaranteed fix.

## Decision

Production method for shots 4–14: **Method A (separate, one still → one 4s clip)**, Flow "Frames"
mode, motion+camera-only prompts.
