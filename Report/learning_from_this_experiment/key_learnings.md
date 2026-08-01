# Key learnings — making an AI anime short (Ember Vow)

Practical, reusable lessons from building a 14-shot, ~1-minute cel-shaded anime short with Gemini
(images) + Google Flow *Omni Flash* (image-to-video) + ffmpeg (edit). Written as "if you do this
again, know these." Analysis behind each point: [`../analyses/`](../analyses/).

---

## Character consistency (the whole ballgame)

1. **Lock a character sheet BEFORE anything else, and attach it to every generation.** It is the
   only cross-shot memory the model has. This single move is what recovered character continuity
   from "impossible" to "mostly works."
2. **Pick the sheet by consistency, not beauty.** Judge candidates on "is it the *same person* in
   every panel," not on which looks nicest. A plainer, on-model sheet beats a prettier one that
   drifts. Choosing the sheet also picks your image model for the whole project.
3. **Name identity anchors explicitly and repeat them verbatim** in every prompt (e.g. "dark braid
   with red cord, brow scar, amber eyes, grey-blue cloak"). Anchors survive; unnamed details drift.
4. **Expect fine-detail drift anyway** — scar size/side, hair tone under coloured light, background.
   Anchors hold; small stuff wobbles. Decide per shot whether it's worth a re-roll.

## Prompting the models

5. **Separate motion from identity in video prompts.** The still already locks the look; the
   image-to-video prompt should describe *only* what moves (subject + camera) plus a "keep the
   design and art style exactly the same" anti-drift line.
6. **Negative constraints are the main anti-drift tool.** The fixes that worked were negations:
   "do not make the face heavier/older/masculine as the clip plays," "the glow must NOT fade or turn
   cold." State what must *not* happen, explicitly.
7. **Generate stills separately and animate them separately.** Do NOT batch multiple shots into one
   timecoded call — batching was non-deterministic and discarded the vetted stills (see the pilot).

## How the model behaves

8. **Every generation is a fresh roll.** The same prompt gives materially different results run to
   run. Budget 2–3 attempts for hard shots and re-roll *isolated* shots — never trust one pass, and
   don't assume a re-roll is an improvement (ours went better → worse → worse).
9. **Close-ups + heavy VFX = highest drift risk.** Wide/medium shots of a resting or slow subject
   are safest. Plan review time where the face is big and the effects are loud.
10. **Design the story around the model's strengths.** One character (not crowds), elemental/energy
    VFX (not physical contact/impact), a "power awakening" arc (not a fight). This sidesteps the two
    things generative video is worst at: cross-shot continuity and physical consequence.
11. **Keep clips short.** ~4s per shot holds clean motion; longer clips get "mushy." Cut long from
    short beats. (Omni Flash caps at 10s and charges ~15 credits per 10s clip.)

## Editing / finishing (the biggest single quality jump)

12. **The "film look" is a finishing pass, not the raw clips.** Color grade + cinemascope letterbox
    + film grain + vignette turns "AI clips" into "film footage" more than any transition does.
13. **A title card + a sparse text arc gives a silent piece its story.** With no dialogue, a few
    well-placed caption cards (setup → turn → title) carry the narrative and give it film identity.
14. **Watch grain vs. file size.** Temporal film grain makes every frame unique and can balloon the
    export (one render hit 400 MB); dial grain down and it dropped to 35 MB with no visible loss.
15. **Know your toolchain's limits.** An old ffmpeg lacked `drawtext` and `xfade` — captions were
    solved with PIL-rendered PNG overlays, transitions with dip-to-black via segment concat. Verify
    filters exist before designing around them.

## Process

16. **Keep every intermediate** (stills, clips, versions). When a later shot fails, an earlier clean
    version is your recovery path.
17. **Inspect frames, never playback.** Extract and crop into the point of interest (a face, a
    contact point). Drift is invisible at thumbnail size and obvious cropped in.
18. **Watermarks and IP:** generated clips carry a provider watermark (removable in-place if it sits
    over low-detail areas). Study references may be copyrighted — keep them out of a public repo.
