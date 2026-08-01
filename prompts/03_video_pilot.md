# Prompts 03 — Video PILOT (shots 1–3, two methods to compare)

**Skills used:** `ai-anime-prompting` §1, §4 (image-to-video, motion-vs-identity), `gemini-omni-prompts`
(image-to-video examples + batched-timecode pattern), `ai-vfx-prompting` §6 (short clips stay clean),
`video-prompting` character-sheets.md "After the sheet" (motion-and-camera only).

---

## Read this first (plain English)

- We are NOT re-describing Yura here. The still already locks her look. These prompts only say
  **what moves** — her body and the camera. That's deliberate.
- We're testing two ways of making shots 1–3, then comparing, before doing all 14.
- Do this in **Google Flow**, model **Omni Flash**, **image-to-video** mode, **16:9**.
- The little phrase "keep the character design and art style exactly the same" is the anti-drift
  seatbelt — it tells the model not to reinvent her mid-clip.

---

## METHOD A — Separate clips (the normal way)

Make three clips. Each one: load the matching still as the starting image, paste the prompt, generate
4 seconds. Save as `vedio_clips/A_shot_01.mp4`, `A_shot_02.mp4`, `A_shot_03.mp4`.

### A — shot_01 (load `shot_01.png`)
```
Animate the attached image. She walks slowly forward across the snow, cold wind blowing ash and
snow sideways, the torn banners flapping in the wind, light snow drifting. Camera slowly pushes in
toward her. Keep the character design, outfit, and 2D cel-shaded anime art style exactly the same,
no change to her appearance. 4 seconds.
```

### A — shot_02 (load `shot_02.png`)
```
Animate the attached image. She has stopped walking; she lowers her gaze and turns her head
slightly to look down at the glinting object near her feet, her cloak and hair shifting gently in
the wind, light snow falling. Camera holds steady with a very subtle handheld sway. Keep the
character design, outfit, and 2D cel-shaded anime art style exactly the same, no change to her
appearance. 4 seconds.
```

### A — shot_03 (load `shot_03.png`)
```
Animate the attached image. Subtle close-up motion: her eyes shift and narrow with wary focus, a
faint cloud of breath leaves her lips in the cold, loose strands of hair drift across her face in
the wind. Camera holds locked with an almost imperceptible slow push-in. Keep her face, design, and
2D cel-shaded anime art style exactly the same, no change to her appearance. 4 seconds.
```

---

## METHOD B — Batched single clip (the pattern I found)

Make ONE clip covering all three beats. Omni Flash maxes at **10 seconds**, so the three beats are
compressed to fit: 4s + 3s + 3s. Load `shot_01.png` as the starting image (its look carries the
sequence), set duration to **10s**, paste the single timecoded prompt below, generate. Save as
`vedio_clips/B_shots_01to03.mp4`.

### B — one prompt, three timecoded beats, 10s total (load `shot_01.png`)
```
Animate the attached image as one continuous shot with the same character throughout — identical
face, braid with red cord, grey-blue cloak, and 2D cel-shaded anime art style in every second, no
visual reset:
0-4s: wide shot, she walks slowly forward across the snowy battlefield, wind blowing snow sideways,
      torn banners flapping, camera slowly pushing in.
4-7s: medium shot, she stops and looks down at a glinting object near her feet, cloak and hair
      shifting in the wind.
7-10s: close-up of her face, wary and alert, faint breath in the cold air, hair drifting.
Muted cold blue-grey palette, cinematic film grain, keep her appearance identical the whole time.
```

---

## Then compare (this is the actual experiment)

Put Method A's three clips in order next to Method B's one clip, and judge on TWO questions:

1. **Consistency:** In which version does Yura stay most clearly the same person — same face, hair
   colour, scar, braid? (Extract a frame from the start and end of each and compare, per
   `ai-vfx-prompting` "inspect frames, never playback".)
2. **Flow:** Which feels more like a real continuous film vs. three disconnected pieces?

Watch for the known risk from `ai-vfx-prompting` §6: Method B's 12-second clip may get "mushy" in
the middle because coherent motion holds best for ~2–3s. If it does, that's a real result — note it.

**Whichever wins becomes the method for shots 4–14.** Tell me the outcome and I'll write the rest.
```
