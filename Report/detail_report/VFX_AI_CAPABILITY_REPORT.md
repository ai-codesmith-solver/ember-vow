# Can Generative AI Do VFX Work?

### A structured capability evaluation of Google Flow / Omni Flash on real phone-shot plates

**Date:** 30 July 2026
**Tool under test:** Google Flow, model *Omni Flash*, Ingredients (video-to-video) mode
**Output format:** 720×1280, 24 fps, 4–10 s
**Plates:** 3 handheld phone clips, deliberately low-light and noisy
**Generations analysed:** 20
**Method:** frame-by-frame inspection against pre-declared pass/fail criteria

---

## 1. Executive summary

Omni Flash is genuinely strong at **appearance** and consistently weak at **behaviour**.

It will invent a photoreal creature, relight your subject convincingly, replace an entire
environment, and — most surprisingly — apply a credible digital-intermediate finish
(depth of field, anamorphic flare, halation, film grain, atmospheric falloff) on request. On
look alone, several results here would survive a first-pass review.

It cannot make a struck body absorb a punch. It cannot reflect or refract the room it was
given. It cannot be iterated on in a controlled way, because every edit re-renders the whole
frame. And it can refuse, without recourse, to let you fix your own original footage.

The gap is not resolution or realism. It is that the model renders **what things look like**,
never **how things respond**. Every failure in this report reduces to that one sentence.

For a production pipeline, the three findings that matter are not the visual ones:

- **No physical response** (§4.5) — the model has no concept of force transfer.
- **No layer isolation** (§4.7) — iteration is whack-a-mole; nothing is non-destructive.
- **Arbitrary moderation** (§4.8) — a routine fix was blocked four times on original content.

A studio can work around a soft-looking flame. It cannot work around a tool where fixing the
mask breaks the emblem, and where the fix for the emblem is refused outright.

### The reframe

The question this evaluation set out to answer — *can generative AI do VFX work?* — turns out
to be the wrong question. Testimony from a working compositor at a top-tier Hollywood facility
(§6) reframes it as: **where in the pipeline does this class of tool belong?**

The answer, already being applied at studio level, is the **labour tier** — roto, paint, prep,
cleanup — and not the hero tier. The reason given from inside the industry is precisely the
defect this evaluation found independently from the outside: **generative models are
non-deterministic by construction, and finishing work is deterministic by requirement.** That
is a structural incompatibility, not a quality gap that a better model closes.

Read §4 as a map of which tier each capability falls into, and §6 for how the industry is
already acting on it.

---

## 2. Why these plates

All three source clips were shot on a phone, handheld, in poor light, with visible noise and
motion blur.

| Plate | Contents | Duration |
|---|---|---|
| A | Open palm reaching into a dim hallway; marble tile floor; blown-out doorway behind | 19.8 s |
| B | Left hand cradling an RGB gaming mouse; right index finger pressing it | 7.2 s |
| C | Two hands "finger-walking" as sparring figures on a glossy black tabletop; face above | 10.5 s |

This was deliberate. A clean, well-lit, tripod-mounted plate would have flattered the tool and
told us nothing. Plate A in particular is a hostile case: the subject is underexposed while the
background is clipped to white, which forces any honest evaluation of relighting.

Plate C additionally provides a **real glossy reflective surface** already in shot — a free,
unfakeable test of whether generated elements are correctly reflected.

---

## 3. Method

For each test:

1. Declare, in advance, the single named VFX discipline being tested and the pass condition.
2. Feed the plate as an Ingredient with a text prompt.
3. Extract frames: `ffmpeg -i <output> -vf "fps=N" f_%02d.png`
4. Read the frames — **zoomed into the contact or interaction point**, cropped where needed.
5. Record pass / partial / fail against the pre-declared condition.

Point 4 is the whole method. Every significant failure in this report is invisible at
thumbnail size and at normal playback speed, and becomes obvious the moment you crop into the
point where two things touch.

---

## 4. Findings

### 4.1 Additive effects — **PASS**

Inserting new elements into a plate is the tool's core strength, and it is genuinely good.

- **Scarlet macaw on an open palm.** Talons gripping the fingers, correct feather colouring, tail hanging past the wrist. Identity held rock-solid across the full clip with zero morphing.
- **Baby dragon.** Scale texture, wing membrane, proportions all consistent. Tight per-foot contact shadows, noticeably better than a generic blob shadow.
- **Live mouse replacing a computer mouse.** Fur, whiskers, ear translucency, tail curling around the fingers. One of the strongest results in the session.
- **Plasma sphere, ember fracture, expanding shockwave rings.** All convincing.
- **Full environment replacement.** A rooftop city and an ancient temple courtyard, both coherent and stable.

**Observation:** static or resting subjects succeeded every time. The one additive failure was
the pink snake, whose head repeatedly vanished and reappeared elsewhere and whose wrist coil
read as a rigid plastic bangle with no taper. The difference is that a snake must *continuously
deform* around a limb, whereas a bird or mouse simply rests. Subjects requiring sustained
non-rigid deformation are unreliable; subjects that hold a pose are not.

---

### 4.2 Reflection and refraction — **FAIL**

Two independent tests, both failing the same way.

**Chrome orb.** The prompt asked for a mirror-finish sphere hovering above the palm, accurately
reflecting the surrounding hallway. The model produced a convincing chrome ball whose
reflection showed **a different person holding up a phone — a generic "mirror-ball selfie."**
It did not attempt to reflect the actual room. It pattern-matched "shiny sphere" to the most
common such image in its training data and substituted that.

**Crystal hand.** The prompt asked for the hand to become transparent crystal with the marble
floor visible and refracted through it. The model produced a beautiful cracked-glass *surface* —
specular highlights, internal fractures — that was **entirely opaque.** At no point is anything
visible through it.

Both are the same failure. Reflection requires knowing what is *around* the object; refraction
requires knowing what is *behind* it. The model has neither, because it never built a spatial
representation of the plate — it is compositing on a 2D canvas.

**Important qualifier (§4.6):** reflections onto a surface *already visibly reflective in the
plate* worked correctly every time, including onto a fully synthetic wet-concrete rooftop. The
capability is real. What fails is synthesising a reflection **inside** an object where the
model must infer the surrounding geometry.

---

### 4.3 Occlusion reconstruction — **CONDITIONAL PASS**

This finding is the most practically useful in the report, because it identifies the exact
condition under which removal work succeeds.

**Fail case — invisible arm.** Plate A, arm asked to turn invisible with the hidden floor
revealed. The transition itself was sophisticated (translucent ghost-outline of the fingers,
fingertips returning semi-transparent before solidifying). But the reconstructed floor is
**smooth marble with no tile grout lines at all.** The real floor has a continuous grid of
joints running through that region. It painted "more marble," not the actual tiles.

**Pass case — removed hands.** Plate C, both hands and forearms removed so figures could stand
in their place. The hands are **completely gone** — no fingers, knuckles or wrists anywhere —
and the background behind them is **correctly reconstructed**: the hanging beaded strings run
continuous and unbroken, the water bottle and crate fully present.

**The difference is motion.** In Plate C the hands moved across frame, so other frames in the
clip genuinely revealed that background — the model had real temporal information to borrow.
In Plate A the arm was comparatively static, so the hidden floor was never revealed anywhere
and had to be invented.

**Practical implication for wire/rig removal:** this tool is viable where the camera or subject
moves enough to expose the occluded region at some point in the shot, and unreliable where it
does not. That is a testable, shot-by-shot criterion an artist can apply before committing.

---

### 4.4 Contact deformation — **INCONSISTENT, and the reason is instructive**

Contact deformation — skin visibly compressing where something presses into it — is treated as
a distinct hard problem in the literature, with dedicated physics models built specifically for
it (e.g. *PhysHand*, arXiv:2409.05143). It was tested twice with opposite results.

**Fail — dragon claws.** Zoomed into the contact points, all four claw tips sit on the palm
**like stickers.** No dimpling, no compression, no pucker; the natural skin lines continue
straight through the contact points as though nothing were pressing on them. No finger sag
under the creature's weight.

**Pass — petting a mouse.** Zoomed into the fingertip, the **fur genuinely parts, flattens and
compresses** in a radiating pattern under the stroking finger.

The likely explanation is training-data frequency, not physics understanding. "Human finger
stroking a small furry animal" is one of the most-filmed interactions in existence. "Reptilian
claw denting a human palm" is vanishingly rare. The model reproduces the deformation it has
seen a million times and cannot generalise to the one it has not.

**This distinction matters commercially.** It means contact realism is not a capability you can
assume — it is a lookup, and its reliability depends entirely on how ordinary your interaction
is. Novel creature contact, which is most of creature work, falls on the wrong side.

---

### 4.5 Physical force response — **HARD FAIL**

Tested six times across two environments with escalating specificity.

The request: one punch lands, and the struck fighter reacts — head snapping sideways, torso
folding away, feet losing grip and sliding back, body displaced. Beat-by-beat choreography was
supplied. Explicit emphasis was placed on loose, trailing limbs (a genuine tell — real impact
briefly slackens limbs) and on a camera jolt at contact.

Progress was made on everything *except* the reaction:

| Requested | Result |
|---|---|
| Rear-foot plant and hip rotation into the strike | **Landed** — a real loaded windup |
| Expanding impact shockwave | **Landed** — large, sustained, cinematic |
| Cape snapping forward with momentum | **Landed** |
| Matte weathered armour, no plastic sheen | **Landed** |
| Impact moment slowed for weight | **Landed** |
| **Struck body absorbs and transfers the force** | **Never landed, six attempts** |

In the final version the fist is buried in the opponent's head, a massive shockwave is bursting
outward — and he is standing **bolt upright with both feet planted, entirely undisplaced.**

This is the same failure as the dragon claws at a different scale. Force goes in; nothing comes
out. The model can render the *iconography* of a powerful hit — windup, flare, shockwave, speed
ramp — while having no model of what a hit *does*.

For fight and stunt work this is disqualifying on its own. The reaction sells the hit; the punch
never does.

---

### 4.6 Cinematography and post-production — **STRONG PASS**

The single largest quality lever found in the entire session, and the most commercially
interesting positive result.

A mid-session rooftop version was technically correct — environment replaced, subjects genuinely
relit cold, reflections correct in synthetic wet concrete — and still read unmistakably as AI
art rather than a film plate. Diagnosis:

1. Everything in perfect focus, no depth of field
2. No lens character — no flare, no chromatic aberration, no halation, no grain
3. No atmospheric depth falloff; distant buildings as contrasty as near ones
4. Uniform saturated teal-and-magenta grade (the overused generative cyberpunk palette)
5. Locked-off camera

A prompt was then written that explicitly requested a **digital-intermediate finishing pass**:
shallow depth of field with the background soft, anamorphic lens character with horizontal
flare, chromatic aberration at frame edges, halation and bloom around light sources, 35 mm film
grain, restrained grade with blue-lifted shadows, atmospheric haze softening contrast with
distance, subtle handheld movement, 24 fps shutter motion blur.

**Nearly all of it landed.** Background pillars fall genuinely soft while the fighters hold
focus. A textbook horizontal anamorphic flare slashes across the impact frame. Grain, haze,
depth falloff and a restrained natural grade are all present. The result stops looking generated.

**This is the key positive finding.** The difference between "AI slop" and "plausible film plate"
was not model capability, subject matter or resolution. It was **photographic and finishing
vocabulary in the prompt.** An artist who knows how to ask for lens and DI behaviour gets
dramatically better output from the identical model than one who does not.

---

### 4.7 Iteration and layer isolation — **FAIL**

Follow-up editing does not perform a local edit. It re-renders the frame.

**Early evidence.** A follow-up asking only for skin dimpling under dragon claws returned
slightly more surface creasing — and a **globally shifted skin tone and exposure across the
entire clip**, in regions the prompt never mentioned.

**Full evidence.** Six consecutive corrective passes on the fight shot:

| Pass | Requested | Broke |
|---|---|---|
| 1 | Replace hands with figures | Hands not removed; figures *added* alongside them |
| 2 | Real humans, not mannequins | — (clean pass) |
| 3 | Distinct superhero designs | Read as glossy action figures |
| 4 | De-IP the armour | Both characters became **medieval knights** |
| 5 | Restore modern superheroes | Lost the mask; gained an IP-adjacent chest emblem |
| 6 | Restore mask, remove emblem | Picked up new gold trim, drifted toward another franchise |

Every pass fixed its target and broke something adjacent. There is no mechanism to say "change
this element and hold everything else." A compositor fixes a mask on its own layer and nothing
else in the frame moves a pixel; here, everything moves.

**Consequence:** the tool cannot be driven to a specification. It can be driven *near* one, and
each additional correction has a real chance of undoing prior work. That is incompatible with
notes-driven client revision, which is most of the job.

---

### 4.8 Content moderation — **BLOCKING FAIL**

Six generation attempts were refused with:

> *"I can't generate the video you requested right now due to interests of third-party content
> providers."*

**Sequence 1 — franchise reference.** A prompt naming a well-known space-opera franchise was
blocked. A rewrite removing every trademarked term but keeping the scene structure (two
colour-coded energy-blade duellists on a walkway over lava) was **also blocked** — the filter
matched the *pattern*, not the words. A wholly original replacement concept (elemental beings on
a floating stone ring) passed.

**Sequence 2 — unfixable own footage.** The final shot's sole flaw was gold spark particles at
the moment of impact — physically wrong, since a fist striking a fabric suit produces no sparks,
and reading as a video-game hit effect rather than film. Four escalating attempts to correct it:

| Attempt | Prompt | Result |
|---|---|---|
| 1 | Full corrective prompt with water-spray replacement | Blocked |
| 2 | Same, all franchise-adjacent phrasing removed | Blocked |
| 3 | Different, IP-clean source clip | Blocked |
| 4 | **Nine words**, no characters, no action, no costumes described | Blocked |

The final attempt was *"Remove the orange sparks. Add water spray instead."* — no describable
IP content of any kind. The filter is keying on the source footage, and there is no prompt-side
remedy.

Note the causal chain: the character drifted toward existing IP **because of §4.7** (uncontrolled
re-rendering across passes), and once drifted, §4.8 made the footage permanently un-editable.
The two failures compound.

**This is the most serious production finding in the report.** The blocked fix is roughly thirty
minutes of ordinary compositing work — pull a matte on the particle layer, delete it, comp in a
water element. Here it was impossible, on wholly original content the user had generated
themselves, with no appeal and no diagnostic explaining what triggered it. A tool that can
arbitrarily refuse to let you finish your own shot cannot be scheduled around.

---

## 5. What we produced anyway

A 4-second finished shot: two original masked superheroes mid-fight in an ancient temple
courtyard, driving rain, lightning and practical brazier firelight, real depth of field,
anamorphic flare, film grain, restrained cinematic grade, correct reflections in wet flagstone.
Built by chaining generations so each pass inherited the previously solved layers.

Honest appraisal: on look, it is close to broadcast-plausible. On behaviour — the punch that
lands without consequence — it is not. And it contains one uncorrectable defect.

---

## 6. Industry corroboration — what a working studio compositor says

> *Source: a video call with a senior VFX compositor at DNEG, one of the handful of top-tier
> Hollywood VFX facilities, with feature credits including large-scale effects work on
> Avatar: The Way of Water and The Odyssey. Paraphrased with permission of the person who
> conducted the interview.*
>
> *[Confirm the studio spelling and the exact credit list before circulating — see note at end
> of section.]*

The findings in §4 were produced from the outside, by black-box testing. They were then put to
someone who does this work inside a major facility. His account both **confirms the central
defect** and **corrects the framing of the question.**

### 6.1 AI is already in production use — for the labour tier

Top-tier studios are actively using AI tooling today. Not experimentally, and not for hero
shots. They use it for what he called the **labour work**: the high-volume, repetitive,
low-creative-judgment tasks that consume enormous artist-hours.

- **Rotoscoping** — frame-by-frame matte extraction
- **Paint and prep** — cleanup, rig and wire removal, plate restoration
- Similar high-volume preparatory work

These are the tasks that historically absorb the largest share of junior and mid-level
artist-hours on a show, and they are the ones being handed to machine assistance first. Multiple
tools are in use across facilities for exactly this band of work.

### 6.2 It is not used for final creative work, and he does not expect it to be

His reasoning is the important part, and it is not a claim about current model quality:

> VFX finishing is a **creative** discipline whose output must also be **precise and
> deterministic**. Every frame must match. Every shot must match every adjacent shot.
> A generative model regenerates on every invocation — so a detail that was correct in one
> frame or one version silently changes in the next.

That is an exact, independent description of **§4.7 (no layer isolation)**. This evaluation
found it by making six corrective passes and watching each one fix its target while breaking
something adjacent — de-IP the armour and the characters became medieval knights; restore the
superhero and the mask disappeared and an IP-adjacent emblem arrived. A compositor states the
same thing as a first principle: *you cannot build deterministic work on a non-deterministic
substrate.*

This matters because it distinguishes two very different kinds of limitation:

| Kind | Example from §4 | Does a better model fix it? |
|---|---|---|
| Quality gap | Flat-looking flame; no floor bounce light | **Yes**, plausibly |
| Structural incompatibility | Every edit re-rolls the frame (§4.7) | **No** — it is what "generative" means |

Findings §4.5 (no force response) and §4.7 (no layer isolation) sit in the second row. They are
not roadmap items. §4.8 (moderation) is a policy choice, but has the same practical effect on a
schedule.

### 6.3 The operating model: AI as an intern, not an artist

His framing was that these tools function as **a capable intern or assistant** — something that
absorbs the grinding work so the artist's own time goes to the judgment-heavy, high-level
creative decisions that actually determine whether a shot works.

That is a raising of the artist's role, not a replacement of it. And it aligns precisely with
this evaluation's strongest positive result, **§4.6**: the same model, given photographic and
finishing vocabulary in the prompt — depth of field, anamorphic character, halation, grain,
restrained grade, atmospheric falloff — produced dramatically better output than when given
subject description alone. The tool's ceiling is set by the craft knowledge of the person
driving it.

### 6.4 What this corroboration changes about the report

Three things:

1. **The go/no-go table in §7 is not speculative.** The "go" column maps closely onto what studios have already adopted; the "no-go" column maps onto what they deliberately have not.
2. **§4.3 is the most commercially relevant finding in this document.** It found that occlusion reconstruction succeeds when camera or subject motion exposes the hidden region, and fails on static occlusions. That is *exactly* the paint/prep/cleanup band now being automated — and it gives an artist a concrete, testable, per-shot criterion for whether to send a plate to an AI tool or to a human. That is directly actionable on a real show.
3. **The physics and determinism failures should be read as scope boundaries, not as bugs.** They mark where the labour tier ends and the hero tier begins.

*Verification note: the studio name is recorded here as DNEG based on the spelling supplied
verbally, and the credit list as reported. Both should be confirmed with the source before this
document is circulated externally — a misattributed studio or credit would undercut otherwise
solid findings.*

---

## 7. Conclusions for a VFX pipeline

**Use it for:**
- Concept and pitch visualisation, look development, mood pieces
- Element generation: fire, plasma, embers, debris, shockwaves, atmosphere
- Environment extension and full background replacement
- Creature or object insertion where the subject rests rather than deforms
- Wire/rig removal **when the camera or subject motion exposes the hidden region**
- Anything where the output is the final answer, not an input to further revision

**Do not use it for:**
- Anything requiring physical response — impacts, weight, force transfer, deformation under load
- Reflective or refractive elements that must show the real plate
- Shot-to-shot character continuity across multiple takes
- Client-notes revision cycles, which require touching one element and holding the rest
- Any schedule-critical shot, given non-appealable moderation refusals

**The one thing to take away:** the same model, given photographic and finishing vocabulary in
the prompt, produced dramatically better output than when given only subject description
(§4.6). The skill that raises AI output quality is not prompt trickery — it is the working
knowledge a compositor or DP already has. That is the practical opportunity here: the tool
rewards existing craft rather than replacing it.

### 7.1 Where this lands, in one line

Sorted by pipeline tier, using both the black-box test results (§4) and the studio account (§6):

| Tier | Work | Verdict |
|---|---|---|
| **Labour** | Roto, paint, prep, cleanup, rig/wire removal, plate restoration | **Already in studio use.** Viable now, with the §4.3 motion criterion as the per-shot test |
| **Element** | Fire, plasma, embers, debris, shockwaves, atmosphere, environment extension | **Viable** as generated elements taken into comp |
| **Look-dev** | Concept, pitch visualisation, mood, previs | **Viable and strong** |
| **Hero / final** | Impacts, physical response, reflective and refractive elements, continuity across shots, notes-driven revision | **Not viable**, and structurally rather than temporarily so (§6.2) |

The honest headline is not "AI can't do VFX." It is that **AI has already taken the labour tier
and is structurally blocked from the hero tier** — and the boundary between them is determinism.
Any facility planning around this should be planning for artists supervising automated prep work,
not for automated shot delivery.

---

## 8. Reproducing this

```bash
# extract frames for inspection
ffmpeg -i output.mp4 -vf "fps=5" f_%02d.png

# brighten a dark plate for inspection only (not for generation)
ffmpeg -i plate.mp4 -vf "curves=all='0/0 0.3/0.55 1/1'" f_%02d.png

# crop into a contact point before judging it — this is where failures live
ffmpeg -i f_18.png -vf "crop=350:300:250:550,scale=1050:900" zoom.png

# trim an ingredient to force a shorter generation
# (Flow locks output duration to ingredient length)
ffmpeg -ss 4 -i source.mp4 -t 4 -c:v libx264 -crf 16 -preset slow -an out_4s.mp4
```

---

## References

- PhysHand: A Hand Simulation Model with Physiological Geometry, Physical Deformation, and Accurate Contact Handling — arXiv:2409.05143
- Subsurface scattering explained — Chaos (blog.chaos.com)
- Art of LED wall virtual production: lessons from The Mandalorian — fxguide
- This is the Way: How Innovative Technology Immersed Us in the World of The Mandalorian — StarWars.com
- Channelling the Inner Beast for Creature Effects — VFX Voice
