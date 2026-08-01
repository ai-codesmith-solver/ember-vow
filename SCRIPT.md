# "EMBER VOW" — 1-minute anime teaser

## Why this premise (tied to the reference videos + your VFX report)

| Reference | What it contributes here |
|---|---|
| KÖK BÖRÜ (Higgsfield/Seedance 2.0) | Teaser-trailer pacing, lone figure against a vast cold landscape, sparse subtitled lines instead of full dialogue |
| Jujutsu Kaisen / Tanjiro clips | The energy-aura VFX language to aim for — cursed-energy red glow, flame-breathing orange, cel-shaded line quality |
| Bang Bang! / sucker | Deliberately **not** used as the template — both depend on multi-character banter and contact-heavy interaction, which your VFX report flags as the two least reliable capabilities (shot-to-shot continuity, contact/impact) |

**One protagonist, no second named character with physical contact, no punches landing.** The
"action" is entirely elemental VFX (aura ignition, embers, ground cracking, shockwave) — all in your
report's "Go" column — rather than combat contact, which is the "hard no-go" column. This is the
single biggest lever for actually holding consistency across 14 independent generations.

## Logline

A lone swordswoman walks a dead, snow-buried battlefield at dusk, searching for something buried in
the ash. She finds her fallen mentor's blade — and as she grips it, a dormant ember-spirit inside
her ignites for the first time.

## Style bible (paste verbatim into every shot's prompt, per `ai-anime-prompting` §3)

> 2D cel-shaded anime, hand-drawn line work, muted cold blue-grey palette breaking into warm
> ember-orange as power ignites. Not 3D CG, not photoreal, not generic anime — Studio
> Ghibli-weight backgrounds with Jujutsu-Kaisen-style energy aura effects. Anamorphic lens flare,
> 35mm film grain, shallow depth of field, restrained grade.

## Character sheet lock (before shot 1)

- **Name:** Yura. One character only for the full 60 seconds.
- **Fixed:** dark braided hair with a single red cord, weathered grey-blue traveling cloak over
  dark armor wraps, one sheathed straight sword, pale scar over left brow, amber eyes.
- **Negatives to repeat every shot:** "not a different outfit, not a different hair length, no new
  characters in frame, no visible second person."

Generate this as a multi-angle character lineup first (front / 3-4 / side, neutral expression) and
feed it as the reference image into every one of the 14 shots below.

---

## Shot list — 14 shots × ~4s = 56s (inside your 40–60s window)

| # | Time | Shot | Camera | VFX report tie-in |
|---|---|---|---|---|
| 1 | 0:00–0:04 | Wide: Yura walks alone across a snow-buried battlefield at dusk, broken banners half-buried, wind blowing ash-snow sideways | Slow push-in, low angle | Environment — Go |
| 2 | 0:04–0:08 | Medium: she stops, looks down at something glinting in the snow | Static, slight handheld | Continuity anchor shot |
| 3 | 0:08–0:12 | Close-up: her face, wary, breath visible in the cold | Locked-off close-up | Identity-lock shot — judge every later shot against this face |
| 4 | 0:12–0:16 | Medium: she kneels, brushes snow off a half-buried sword hilt | Slight downward tilt | Ordinary contact (hand on hilt) — Go per report §4.4 |
| 5 | 0:16–0:20 | Insert: hand closes around the hilt, snow falling off the blade in slow motion | Macro, static | Element-only insert, no continuity risk |
| 6 | 0:20–0:24 | Wide: she pulls the sword free, stands, wind picks up harder | Slow pull-back | — |
| 7 | 0:24–0:28 | Close-up: her eyes widen slightly, a faint ember-orange light reflects in them | Locked-off | Reflection **onto** an already-lit surface — Go per §4.2 qualifier (not reflection *inside* an object) |
| 8 | 0:28–0:32 | Medium: a thin thread of ember-light spirals up from the blade along her arm | Static | Element generation — Go |
| 9 | 0:32–0:36 | Wide: the ember-light expands into a full aura around her, snow melting outward in a ring at her feet | Slow orbit | Element generation — Go (this is your JJK/Tanjiro-aesthetic beat) |
| 10 | 0:36–0:40 | Close-up: hair and cloak whipping in the self-generated wind, aura reflected on her face | Locked-off, slight push-in | Identity-check shot — compare against shot 3 |
| 11 | 0:40–0:44 | Medium: ground cracks beneath her in a circle, embers drifting upward like snow in reverse | Low angle, static | Element + environment — Go |
| 12 | 0:44–0:48 | Wide: she raises the sword overhead, aura flaring to full brightness, ash-snow blown back in a ring | Slow low-angle push-in | Shockwave/expanding-ring VFX — confirmed Go in your report (§4.1) |
| 13 | 0:48–0:52 | Extreme wide: the battlefield lit orange around her single silhouette, mountains behind her in the dusk light | Static, epic wide | Cinematic finishing pass — your report's single strongest lever (§4.6) |
| 14 | 0:52–0:56 | Final hero pose: close-medium, she looks directly toward camera, ember-light steady now, faint smile or resolve | Locked-off, slow fade | Closing identity-lock shot |

**Optional single caption line**, KÖK BÖRÜ-style, appearing once around shot 3 or shot 9 — e.g.
*"They said it died with him."* Keep it to one line for the whole film; no back-and-forth dialogue
(dialogue/lip-sync across shots is not something either your report or the researched sources
verified as reliable).

---

## Before production: the pilot test (flagged earlier, now is when to run it)

Before generating all 14: take shots 1–3 and generate them **two ways** —
(a) three separate `image_to_video` calls, one character-sheet reference each, vs.
(b) one single Omni Flash call with all three timecoded in one prompt (0–4s / 4–8s / 8–12s), per the
batching pattern found in `gemini-omni-prompts/SKILL.md`.

Compare Yura's face/hair/outfit across the three shots in both versions, frame-extracted and
cropped per `ai-vfx-prompting` §6 ("inspect frames, never playback"). Whichever holds identity
better decides how you generate the remaining 11 shots.

## Per-shot prompt template (fill in from the table above)

```
[CONTINUITY]  Same character as reference sheet: Yura — dark braided hair with red cord,
              grey-blue cloak, dark armor wraps, one sheathed straight sword, pale brow scar,
              amber eyes. Not a different outfit, not a different hair length, no second
              character in frame.
[SUBJECT]     <shot description from the table>
[MOTION]      <camera + character motion from the table, separated from style>
[STYLE]       2D cel-shaded anime, hand-drawn line work, muted cold blue-grey palette breaking
              into warm ember-orange as power ignites. Studio-Ghibli-weight backgrounds,
              Jujutsu-Kaisen-style energy aura. Not 3D CG, not photoreal.
[FINISHING]   Anamorphic lens flare, 35mm film grain, shallow depth of field, restrained grade.
[NEGATIVES]   No new characters, no visual reset of design, no impact/contact with another body.
```
