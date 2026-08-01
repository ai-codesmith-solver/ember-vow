# Prompts 02 — The 14 shot still images

**Skills used:** `video-prompting/references/workflows/character-sheets.md` (scene-still-from-sheet
guidance), `ai-anime-prompting` §3–4 (reused style-anchor string + per-shot structure),
`ai-vfx-prompting` §3 (cinematic finishing block).

---

## Read this first (plain English)

- These are **14 still pictures** — one frozen frame for each shot. Later we turn each one into a
  4-second moving clip. Right now we just need the pictures.
- **Every single time**, attach `yura_character_sheet.png` as a reference image in Gemini, THEN
  paste the prompt. The sheet is what keeps Yura looking like the same person. Don't skip it on any
  shot.
- Generate each in **Gemini** (the model we picked). Save them as `shot_01.png`, `shot_02.png` …
  `shot_14.png` in `assets/image-scene/`.
- **How to judge each one:** does she look like the same Yura from the sheet — same braid + red
  cord, same grey-blue cloak, same face? If yes, keep it. If she drifts, regenerate. A plainer
  correct image beats a prettier wrong one.
- Every prompt already ends with the same style + quality + rules line — just copy the whole box.
- The story goes cold-and-grey for shots 1–6, then the orange fire-power switches on from shot 7
  onward. That's why the later prompts add the glow.

Each box below is complete. Copy the whole thing.

---

### shot_01 — Wide: walking the dead battlefield
```
Attached image is the character Yura — keep her face, braid with red cord, brow scar, grey-blue
cloak, dark armor wraps and sheathed sword exactly the same.

Wide establishing shot, low camera angle. Yura walks alone across a vast snow-buried battlefield at
dusk, seen from a distance so she is small in the frame. Broken war banners and shattered spears
half-buried in grey snow around her, cold wind blowing ash and snow sideways, heavy overcast dusk
sky, distant mountains.

2D cel-shaded anime, hand-drawn line work, muted cold blue-grey palette, Studio-Ghibli-weight
painted background. Cinematic: shallow depth of field, subtle anamorphic lens flare, 35mm film
grain, restrained natural grade, atmospheric haze. 16:9. Not 3D CG, not photoreal. Do not change
her outfit or hairstyle, no second character in frame.
```

### shot_02 — Medium: she stops and notices something
```
Attached image is the character Yura — keep her face, braid with red cord, brow scar, grey-blue
cloak, dark armor wraps and sheathed sword exactly the same.

Medium shot, eye level. Yura has stopped walking and looks down toward the snow at something
glinting near her feet, one hand slightly raised, guarded expression. Snowy battlefield around her,
broken banners behind, cold wind, dusk light.

2D cel-shaded anime, hand-drawn line work, muted cold blue-grey palette, Studio-Ghibli-weight
painted background. Cinematic: shallow depth of field, subtle anamorphic lens flare, 35mm film
grain, restrained natural grade, atmospheric haze. 16:9. Not 3D CG, not photoreal. Do not change
her outfit or hairstyle, no second character in frame.
```

### shot_03 — Close-up: her face, wary
```
Attached image is the character Yura — keep her face, braid with red cord, brow scar, amber eyes
exactly the same.

Tight close-up of Yura's face, eye level. Wary, alert expression, amber eyes sharp, faint visible
breath in the cold air, a few strands of hair moving in the wind. Softly blurred snowy battlefield
behind her.

2D cel-shaded anime, hand-drawn line work, muted cold blue-grey palette. Cinematic: shallow depth
of field, subtle anamorphic lens flare, 35mm film grain, restrained natural grade. 16:9. Not 3D CG,
not photoreal. Do not change her face or hairstyle, no second character in frame.
```

### shot_04 — Medium: kneeling, uncovering a buried sword
```
Attached image is the character Yura — keep her face, braid with red cord, brow scar, grey-blue
cloak, dark armor wraps exactly the same.

Medium shot, slight high angle looking down. Yura kneels in the snow and brushes snow away from a
half-buried sword hilt sticking out of the ground in front of her, focused expression. Grey snow,
broken battlefield debris, dusk light.

2D cel-shaded anime, hand-drawn line work, muted cold blue-grey palette, Studio-Ghibli-weight
painted background. Cinematic: shallow depth of field, subtle anamorphic lens flare, 35mm film
grain, restrained natural grade, atmospheric haze. 16:9. Not 3D CG, not photoreal. Do not change
her outfit or hairstyle, no second character in frame.
```

### shot_05 — Insert: hand closing on the hilt
```
Attached image is the character Yura — keep her hand, forearm wraps and sleeve exactly the same as
the reference.

Extreme close-up (macro insert) of Yura's hand closing firmly around an old sword hilt half-buried
in snow, loose snow falling off the blade, cold blue tones, fine detail on the worn leather grip
and her forearm bindings.

2D cel-shaded anime, hand-drawn line work, muted cold blue-grey palette. Cinematic: shallow depth
of field, subtle anamorphic lens flare, 35mm film grain, restrained natural grade. 16:9. Not 3D CG,
not photoreal.
```

### shot_06 — Wide: pulling the sword free, standing
```
Attached image is the character Yura — keep her face, braid with red cord, brow scar, grey-blue
cloak, dark armor wraps exactly the same.

Wide shot, eye level. Yura stands and pulls the old sword free from the snow, holding it at her
side, cloak and hair pushed by rising wind, determined stance. Snowy battlefield, broken banners,
darkening dusk sky.

2D cel-shaded anime, hand-drawn line work, muted cold blue-grey palette, Studio-Ghibli-weight
painted background. Cinematic: shallow depth of field, subtle anamorphic lens flare, 35mm film
grain, restrained natural grade, atmospheric haze. 16:9. Not 3D CG, not photoreal. Do not change
her outfit or hairstyle, no second character in frame.
```

### shot_07 — Close-up: ember light first appears in her eyes
```
Attached image is the character Yura — keep her face, braid with red cord, brow scar, amber eyes
exactly the same.

Tight close-up of Yura's face. Her eyes widen slightly as a faint warm ember-orange glow begins to
reflect on her face from the sword below frame, first hint of hidden power waking, the cold blue
light now touched by warm orange on one side of her face.

2D cel-shaded anime, hand-drawn line work, cold blue-grey palette with a first touch of warm
ember-orange glow. Cinematic: shallow depth of field, subtle anamorphic lens flare, 35mm film
grain, restrained grade. 16:9. Not 3D CG, not photoreal. Do not change her face or hairstyle, no
second character in frame.
```

### shot_08 — Medium: ember-thread spiraling up her arm
```
Attached image is the character Yura — keep her face, braid with red cord, grey-blue cloak, dark
armor wraps exactly the same.

Medium shot. A thin glowing thread of ember-orange light spirals up from the sword blade along
Yura's arm, small floating embers rising around it, her expression a mix of awe and focus. Snowy
battlefield behind, dusk turning darker so the glow reads brighter.

2D cel-shaded anime, hand-drawn line work, cold blue-grey palette breaking into warm ember-orange
energy, Jujutsu-Kaisen-style glowing aura effect. Cinematic: shallow depth of field, subtle
anamorphic lens flare, 35mm film grain, restrained grade. 16:9. Not 3D CG, not photoreal. Do not
change her outfit or hairstyle, no second character in frame.
```

### shot_09 — Wide: full aura ignites, snow melts in a ring
```
Attached image is the character Yura — keep her face, braid with red cord, grey-blue cloak, dark
armor wraps and sword exactly the same.

Wide shot, eye level. A full ember-orange energy aura ignites around Yura, glowing bright against
the dark battlefield, the snow melting outward in a visible ring at her feet, embers streaming
upward, cloak billowing. She holds the sword, powerful stance.

2D cel-shaded anime, hand-drawn line work, cold blue-grey environment lit by intense warm
ember-orange aura, Jujutsu-Kaisen-style energy effect. Cinematic: shallow depth of field, subtle
anamorphic lens flare, 35mm film grain, restrained grade, atmospheric haze. 16:9. Not 3D CG, not
photoreal. Do not change her outfit or hairstyle, no second character in frame.
```

### shot_10 — Close-up: aura on her face, hair whipping
```
Attached image is the character Yura — keep her face, braid with red cord, brow scar, amber eyes
exactly the same.

Tight close-up of Yura's face lit strongly by the ember-orange glow, hair and cloak edge whipping
in the wind her own power is throwing off, fierce determined expression, warm light and small
embers around her.

2D cel-shaded anime, hand-drawn line work, face lit by warm ember-orange energy against cold dark
background. Cinematic: shallow depth of field, subtle anamorphic lens flare, 35mm film grain,
restrained grade. 16:9. Not 3D CG, not photoreal. Do not change her face or hairstyle, no second
character in frame.
```

### shot_11 — Medium: ground cracks, embers rising
```
Attached image is the character Yura — keep her face, braid with red cord, grey-blue cloak, dark
armor wraps and sword exactly the same.

Medium shot, low camera angle. The ground cracks beneath Yura in a circular pattern with
ember-orange light glowing up through the cracks, embers drifting upward like snow falling in
reverse, her aura strong around her, standing firm.

2D cel-shaded anime, hand-drawn line work, dark battlefield lit by warm ember-orange energy,
Jujutsu-Kaisen-style effect. Cinematic: shallow depth of field, subtle anamorphic lens flare, 35mm
film grain, restrained grade. 16:9. Not 3D CG, not photoreal. Do not change her outfit or
hairstyle, no second character in frame.
```

### shot_12 — Wide: sword raised, power flares to full
```
Attached image is the character Yura — keep her face, braid with red cord, grey-blue cloak, dark
armor wraps and sword exactly the same.

Wide shot, low camera angle looking up at her. Yura raises the glowing sword overhead, her
ember-orange aura flaring to full brightness, a ring of ash and snow blown outward around her,
embers everywhere, heroic and powerful.

2D cel-shaded anime, hand-drawn line work, dark battlefield overwhelmed by brilliant warm
ember-orange energy, expanding shockwave ring. Cinematic: shallow depth of field, subtle anamorphic
lens flare, 35mm film grain, restrained grade, atmospheric haze. 16:9. Not 3D CG, not photoreal. Do
not change her outfit or hairstyle, no second character in frame.
```

### shot_13 — Extreme wide: her silhouette lighting the field
```
Attached image is the character Yura — keep her silhouette, cloak shape, braid and raised sword
consistent with the reference.

Extreme wide shot. Yura is a single small silhouette standing on the battlefield, her ember-orange
aura lighting the whole snowy field warm around her against the cold dusk, distant mountains behind
her, dramatic and epic scale.

2D cel-shaded anime, hand-drawn line work, vast cold blue landscape with a warm ember-orange glow
radiating from her single figure, Studio-Ghibli-weight painted background. Cinematic: deep
atmospheric haze, subtle anamorphic lens flare, 35mm film grain, restrained grade. 16:9. Not 3D CG,
not photoreal. No second character in frame.
```

### shot_14 — Close-medium: final hero pose toward camera
```
Attached image is the character Yura — keep her face, braid with red cord, brow scar, amber eyes,
grey-blue cloak exactly the same.

Close-medium shot, eye level. Yura looks directly toward the camera, calm and resolved now, a faint
quiet smile, her ember-orange power steady and glowing gently around her, sword held at her side.
Dark battlefield softly lit warm by her aura.

2D cel-shaded anime, hand-drawn line work, warm ember-orange glow against dark background.
Cinematic: shallow depth of field, subtle anamorphic lens flare, 35mm film grain, restrained grade.
16:9. Not 3D CG, not photoreal. Do not change her face or hairstyle, no second character in frame.
```

---

## After all 14 are generated

- Save them as `shot_01.png` … `shot_14.png` in `assets/image-scene/`.
- Lay them out in order and check two things: (1) is it clearly the same Yura in every one, and
  (2) does the story read cold→ignite→hero as the shots progress.
- Then tell me — the next step is turning these stills into moving 4-second clips with Omni Flash,
  and **that's where we run the consistency pilot** (shots 1–3 made two different ways) we talked
  about earlier.
```
