---
name: ai-vfx-prompting
description: Write effective prompts for AI video generation tools (Google Flow/Omni Flash, Runway, Kling, Veo) when doing VFX or CGI work on real footage. Use when adding creatures or objects to a plate, replacing environments, removing elements, relighting a shot, or making generated footage look like a film plate rather than AI output. Also covers which VFX tasks these tools cannot do, so you stop burning credits on them.
---

# Prompting AI video tools for VFX work

Written by a VFX artist, for VFX artists. Derived from a structured 20-generation evaluation of
Google Flow / Omni Flash on real handheld phone plates. Most of it transfers to any
video-to-video model.

The single most useful thing here: **§1, the go/no-go table.** Read it before you write anything.
Half of wasted credits go on tasks these tools structurally cannot do.

## Where this fits in a real pipeline

Corroborated by a senior compositor at a top-tier Hollywood facility: studios are **already**
using AI tooling in production, but only for the **labour tier** — roto, paint, prep, cleanup,
wire and rig removal. Not for hero shots, and by his account not ever, for a specific reason:

> Finishing work must be deterministic and frame-exact. A generative model regenerates on every
> invocation, so a detail that was right in one version silently changes in the next.

That is a structural incompatibility, not a quality gap a newer model closes. Treat it as the
boundary of the tool:

| Use it for | Don't |
|---|---|
| Prep, cleanup, roto-adjacent removal, element generation, look-dev, previs | Hero shots, physical interaction, continuity across shots, notes-driven revision |

The working model is **AI as an intern, not an artist** — it takes the grinding work so your time
goes to judgment. Which is why §3 matters most: the tool's ceiling is set by *your* craft
vocabulary, not by the model.

---

## 1. Go / no-go: what these tools can actually do

| Task | Verdict | Why |
|---|---|---|
| Insert a creature or object that **rests** in shot | **Go** | Reliable, high quality, holds identity |
| Insert an element that **continuously deforms** around a limb | Risky | Wrapping snakes, tentacles — identity breaks up mid-clip |
| Element generation (fire, plasma, embers, debris, shockwaves) | **Go** | Genuinely good |
| Full environment / background replacement | **Go** | Coherent and stable |
| **Relighting the subject** to match a new environment | **Go** | Works well and is often surprisingly convincing |
| Cinematic finishing (DOF, lens flare, grain, grade) | **Go — biggest quality lever** | See §3 |
| Wire/rig removal where camera or subject **moves** | **Go** | Motion reveals the hidden area; reconstruction is correct |
| Wire/rig removal on a **static** occlusion | No-go | Nothing ever reveals what's behind; it invents plausible mush |
| Reflection **inside** a generated object (chrome, mirror) | **No-go** | Hallucinates a stock reflection, not your plate |
| Refraction through a generated object (glass, water, crystal) | **No-go** | Renders opaque; you never see through it |
| Reflection **onto** an already-reflective surface in the plate | **Go** | Works, including onto generated wet ground |
| Contact deformation for **ordinary** interactions | **Go** | Fur under a stroking finger compresses correctly |
| Contact deformation for **novel** creature contact | **No-go** | Claws sit on skin like stickers; no dent, no pucker |
| **Impact / force response** — a body reacting to being hit | **Hard no-go** | See §5. Do not spend credits here |
| Shot-to-shot character continuity | **No-go** | Identity drifts every pass |
| Iterative client-notes revision on one element | **No-go** | Every edit re-renders the whole frame |

**The unifying rule:** these models render *what things look like*, never *how things respond*.
Anything appearance-based tends to work. Anything requiring physical consequence or knowledge of
scene geometry tends to fail. Sort your shot list by that axis before you start.

---

## 2. Prompt structure that works

Five blocks, in this order. Omitting the last two is why most output looks generated.

```
1. PLATE LOCK      what must not change
2. SUBJECT         what you're adding or replacing, described concretely
3. INTERACTION     how it touches / affects the real footage
4. LIGHTING        how the light in the scene reaches it, and vice versa
5. FINISHING       lens, DOF, grain, grade — the DI pass
```

### 1. Plate lock

State explicitly what stays. These models drift by default; negative constraints are the main
tool for holding a shot.

> Exact same camera framing, angle and handheld motion as the original footage. Background, floor,
> and room lighting completely unchanged.

### 2. Subject

Concrete and physical. Avoid adjectives that only describe mood.

> A small live mouse resting in the palm — soft grey-brown fur, pink nose, long whiskers, small
> round ears, dark eyes with a catchlight, thin pink tail curling against the fingers for grip.

### 3. Interaction

The part most people skip, and the part that separates a composite from a sticker.

> Where the finger presses down, the fur visibly compresses and flattens under the fingertip,
> springing back as the finger lifts. Its weight presses into and slightly indents the palm.

### 4. Lighting

Both directions: how the plate lights the element, **and** how the element lights the plate.
The second direction is unreliable (§5) but worth asking for.

> Treat the fireball as the only light source: warm flickering glow on the palm and forearm,
> falling off with distance so the background stays dark, with a warm glow bouncing onto the
> nearest floor tiles.

### 5. Finishing

See §3. This is the block that decides whether it reads as film.

---

## 3. The finishing block — the biggest quality lever

A technically correct result (environment replaced, subject correctly relit, reflections right)
can still read instantly as AI art. The reasons are photographic, not generative:

1. Everything in perfect focus — **no depth of field**
2. No lens character — no flare, no chromatic aberration, no halation
3. No grain
4. No atmospheric depth falloff — distant objects as contrasty as near ones
5. Oversaturated uniform grade (the teal-and-magenta generative default)
6. Locked-off camera

Appending an explicit DI request fixes most of it. This block, near-verbatim, took a shot from
"AI art" to "plausible plate" in testing:

```
Cinematic finishing: shallow depth of field with the background soft and out of focus, sharp
focus held only on the subject. Anamorphic lens character with subtle horizontal flare from the
bright sources, gentle chromatic aberration toward the frame edges. Soft halation and bloom
around bright light sources. Fine 35mm film grain. Restrained colour grade with natural material
tones, blue-lifted shadows, warm highlights, not oversaturated. Atmospheric haze softening
contrast with distance. Subtle handheld camera movement. Motion blur consistent with a 24fps
cinema shutter.
```

Keep it in your snippets file and append it to everything. **Your existing knowledge of lenses,
DI and grading is worth more here than any prompt trick** — the tool rewards craft vocabulary.

---

## 4. Negative constraints — use them heavily

Positive-only prompts drift toward whatever is statistically most common in training data. What
that means in practice:

| You ask for | You get by default | Fix |
|---|---|---|
| "Generic humanoid figures" | **Artist's posable mannequins** with ball joints | "Real human faces, hair and skin. Not mannequins, not dolls, no visible joints or seams." |
| "Small figures on a table" | **Glossy action figures** | "Matte weathered materials with scuffs. Not statues, not action figures, no plastic sheen." |
| "Futuristic city" | **Neon cyberpunk cliché** | "No neon, no signage, no modern elements." |
| "Replace the hands with X" | X **added alongside** the still-visible hands | "The hands and forearms are completely absent from the frame — no fingers, knuckles, wrists or forearms visible anywhere." |

Note that last one especially: **"replace" reads as "add."** If something must be gone, say it is
gone, list the parts, and say it twice.

---

## 5. Where to stop spending credits

Two things did not improve across repeated, escalating attempts. Recognise them and route around
them.

### Force response — six failures

The request was one punch landing with a real reaction: head snapping sideways, torso folding,
feet losing grip, body displaced. Beat-by-beat choreography was supplied, plus explicit
instruction on loose trailing limbs and a camera jolt at contact.

Everything *except* the reaction landed — loaded windup, cape momentum, shockwave, speed ramp,
matte materials. The struck body never moved. The final version has a fist buried in a head, a
shockwave bursting outward, and the recipient standing bolt upright, feet planted.

**Route around it:** shoot the reaction as a separate shot. Generate the swing in one clip and the
recoil in another, then cut them together. A real fight scene is 15–25 cuts of 1–2 seconds
anyway; nobody shoots it as one unbroken take. Which leads to §6.

### Reflection and refraction inside generated objects

A chrome sphere returned a hallucinated stock "mirror-ball selfie" instead of the actual room. A
"glass" hand rendered fully opaque. Both require inferring surrounding geometry the model never
built. Do these in comp, not here.

---

## 6. Workflow that actually works

### Build in isolated steps, one variable at a time

Compound requests collapse. Asking in a single prompt for environment replacement + full-body
character generation + face relighting + fight choreography caused the model to **abandon the
source footage entirely** and return a generic two-panel template.

Broken into steps, each inheriting the last, the same shot succeeded:

```
1. remove hands → plain figures        (isolate the removal)
2. plain figures → distinct designs    (isolate character design)
3. add choreography                    (isolate action)
4. replace environment                 (isolate the world)
5. add cinematic finishing             (isolate the DI pass)
```

Feed each output back as the ingredient for the next step. It inherits the solved layers, so all
its effort goes to the one new thing.

### Budget for identity drift

Every pass re-renders everything. In testing, a character drifted from sleek superhero → medieval
knight → bare-faced → new emblem → new trim across four corrective passes, each fix breaking
something adjacent. Expect this. **Lock in look before action**, since re-rolling for a costume
note will also re-roll your choreography.

### Generate short, cut long

Coherent motion holds for roughly 2–3 seconds. A five-beat fight in a 10-second clip gets mushed
into soup with dead space in the middle. One beat in 4 seconds is clean.

If your tool locks output duration to ingredient length, trim the ingredient locally first:

```bash
ffmpeg -ss 4 -i source.mp4 -t 4 -c:v libx264 -crf 16 -preset slow -an out_4s.mp4
```

### Inspect frames, never playback

Every significant defect found in testing was invisible at thumbnail size and during normal
playback, and obvious once cropped into the contact point.

```bash
ffmpeg -i output.mp4 -vf "fps=5" f_%02d.png                        # extract
ffmpeg -i f_18.png -vf "crop=350:300:250:550,scale=1050:900" z.png # crop the contact point
```

For a dark plate, brighten **for inspection only** — never pre-brighten the plate you feed in:

```bash
ffmpeg -i plate.mp4 -vf "curves=all='0/0 0.3/0.55 1/1'" f_%02d.png
```

Declare your pass condition *before* you generate. "Does the fur compress under the fingertip,
yes or no" beats "does this look good," which you will always answer yes to at 4 a.m.

---

## 7. Content moderation — plan for it

These tools refuse IP-adjacent output, and the filter is broader than trademarked words.

- **Naming a franchise** blocks immediately.
- **Describing its structure** without naming it also blocks. Two colour-coded energy-blade
  duellists on a walkway over lava was refused with no franchise named — the filter matched the
  pattern.
- **Naming the thing you want removed** can block. A prompt asking to *remove* a lightning-bolt
  emblem was refused, because it described a lightning bolt on a red-suited masked figure.
  Describe the target state instead: *"plain smooth crimson with no marking of any kind."*
- **Drifted footage becomes permanently un-editable.** Once a character drifts IP-adjacent
  (§6, identity drift), the filter reads the *source clip*, and no prompt rewording recovers it.
  In testing, four escalating attempts to remove a spark effect were refused, down to a nine-word
  prompt with no describable IP content at all.

**Practical defences:**

1. **De-IP early and explicitly.** Put "wholly original design, no emblem, symbol, logo or
   insignia of any kind" in the first prompt, not the fifth.
2. Beware over-correcting. "Plate armour with an open helm" — written to dodge a superhero
   resemblance — turned both characters into medieval knights. Specify what you *do* want.
3. **Keep every intermediate generation.** When a later one becomes un-editable, an earlier
   clean version is your only recovery path.
4. Treat any schedule-critical shot as unsuitable. Refusals are non-appealable and give no
   diagnostic.

---

## 8. Quick reference

**Prompt skeleton**

```
[PLATE LOCK]   Exact same camera framing, angle and motion as the original. <what stays> unchanged.
[SUBJECT]      <concrete physical description — materials, colour, build, texture>
[NEGATIVES]    Not <default it will drift to>. No <artefact you don't want>.
[INTERACTION]  Where <element> touches <plate>, <specific deformation / shadow / occlusion>.
[LIGHTING]     <plate light> falls on <element>; <element light> falls on <plate>.
[FINISHING]    <the DI block from §3>
```

**Before you generate**
- Is this task in the "go" column of §1?
- One variable, or am I compounding?
- Have I written the negatives?
- Have I written the finishing block?
- What is my pass condition, in one sentence?

**After you generate**
- Extract frames. Crop into the contact point. Judge there.
- Compare against the *previous* version too, not just the brief — check what silently drifted.
