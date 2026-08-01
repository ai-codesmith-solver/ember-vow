# Prompts 04 — Video shots 4–14 (Method A: separate clips)

**Skills used:** `ai-anime-prompting` §4–5 (motion-vs-identity, separate-clip workflow — see the
tested finding in `Report/analyses/pilot_method_A_vs_B.md`), `video-prompting` character-sheets.md
"After the sheet" (motion + camera only), `ai-vfx-prompting` §6 (short clips stay clean).

---

## Read this first (plain English)

- Pilot decided it: **Method A** — one approved still → one 4-second clip. Do all of these that way.
- In **Google Flow**: model **Omni Flash**, **image-to-video**, use **"Frames"** mode (so the clip
  starts from the exact still), **16:9**, **4s**, **x1**.
- For each: load the matching `shot_XX.png` as the frame, paste the prompt, generate, save as
  `vedio_clips/shot_04.mp4` … `shot_14.mp4`.
- These prompts describe **only motion + camera** — the still already locks how Yura looks. The
  "keep the character design and art style exactly the same" line is the anti-drift seatbelt.
- Judge each clip on frames, not playback: does she stay the same Yura start-to-end? If she drifts
  or the motion goes mushy, re-roll that one clip (it's isolated — costs you nothing else).
- Fire/aura shots (8–14) are higher-risk: budget 2–3 attempts each, per the pilot's non-determinism
  finding.

---

### shot_04 (load `shot_04.png`)
```
Animate the attached image. She kneels and reaches out, brushing snow off the half-buried sword
hilt in front of her, her braid and cloak shifting gently in the cold wind, light snow falling.
Camera holds steady with a very slight downward tilt. Keep the character design, outfit, and 2D
cel-shaded anime art style exactly the same, no change to her appearance. 4 seconds.
```

### shot_05 (load `shot_05.png`)
```
Animate the attached image. Extreme close-up: her hand closes and grips the old sword hilt firmly,
loose snow falling off the blade and her forearm wrap, faint cold vapor in the air. Camera holds
locked and still. Keep the hand, wraps, and 2D cel-shaded anime art style exactly the same. 4 seconds.
```

### shot_06 (load `shot_06.png`)
```
Animate the attached image. She rises to standing and pulls the sword free, holding it at her side,
her cloak and hair pushed harder by rising wind, torn banners flapping behind her. Camera slowly
pulls back. Keep the character design, outfit, and 2D cel-shaded anime art style exactly the same,
no change to her appearance. 4 seconds.
```

### shot_07 (load `shot_07.png`)
```
Animate the attached image. Close-up: her eyes slowly widen as a faint warm ember-orange glow rises
onto her face from below, flickering gently, first hint of waking power, a few loose hairs drifting.
Camera holds locked with an almost imperceptible push-in. Keep her face, amber eyes, and 2D
cel-shaded anime art style exactly the same, no change to her appearance. 4 seconds.
```

### shot_08 (load `shot_08.png`)
```
Animate the attached image. A thin thread of glowing ember-orange light spirals upward from the
sword blade along her arm, small embers rising and floating around it, her expression shifting from
awe to focus. Camera holds steady. Keep the character design, outfit, and 2D cel-shaded anime art
style exactly the same, no change to her appearance. 4 seconds.
```

### shot_09 (load `shot_09.png`)
```
Animate the attached image. The ember-orange aura flares up brighter around her, the ring of melted
snow at her feet steaming, embers streaming upward, her cloak billowing in the heat. Camera holds
with a very slow subtle orbit. Keep the character design, outfit, and 2D cel-shaded anime art style
exactly the same, no change to her appearance. 4 seconds.
```

### shot_10 (load `shot_10.png`)
```
Animate the attached image. Close-up: her hair and the edge of her hood whip in the wind her own
power throws off, the ember-orange glow flickering across her fierce determined face, small embers
drifting past. Camera holds locked with a slight push-in. Keep her face, amber eyes, braid, and 2D
cel-shaded anime art style exactly the same, no change to her appearance. 4 seconds.
```

### shot_11 (load `shot_11.png`)
```
Animate the attached image. The cracks in the ground pulse brighter with ember-orange light, embers
drifting upward around her like snow falling in reverse, her aura steady and strong, she stands
firm. Camera holds steady at the low angle. Keep the character design, outfit, and 2D cel-shaded
anime art style exactly the same, no change to her appearance. 4 seconds.
```

### shot_12 (load `shot_12.png`)
```
Animate the attached image. She raises the glowing sword fully overhead as her ember-orange aura
flares to peak brightness, a ring of ash and snow blasting outward around her, embers filling the
air. Camera slowly pushes in from the low angle. Keep the character design, outfit, and 2D
cel-shaded anime art style exactly the same, no change to her appearance. 4 seconds.
```

### shot_13 (load `shot_13.png`)
```
Animate the attached image. Extreme wide: her single silhouette glows warm on the vast snowy field,
the ember light pulsing gently and lighting the snow around her, slow drifting snow, distant
mountains still. Camera holds locked and epic. Keep the silhouette, cloak shape, and 2D cel-shaded
anime art style exactly the same. 4 seconds.
```

### shot_14 (load `shot_14.png`)
```
Animate the attached image. Close-medium: she looks steadily toward camera, her expression settling
into calm resolve with a faint quiet smile, the ember-orange glow around her softening and holding
gently, a few slow embers drifting. Camera holds locked. Keep her face, amber eyes, braid, and 2D
cel-shaded anime art style exactly the same, no change to her appearance. 4 seconds.
```

---

## After all 14 clips exist (shots 1–3 from the pilot's Method A + these 11)

- You'll have `shot_01.mp4` … `shot_14.mp4` in `vedio_clips/` (rename the pilot's `shot_1/2/3.mp4`
  to `shot_01/02/03.mp4` for clean ordering).
- Quick per-clip check: extract the first and last frame of each and confirm Yura doesn't drift
  within the clip.
- Then it's the CapCut edit: lay all 14 in order, trim, and we can add the single caption line and
  crop out the corner ✦ watermark. Tell me when the clips are done.
```
