---
name: cinematic-video-finishing
description: Turn raw/AI-generated video clips into footage that reads as a real film, using ffmpeg — cinematic color grade, cinemascope letterbox, film grain, vignette, caption/title cards, dip-to-black transitions, and open/close fades. Use when a video "looks like stitched clips" and needs a professional finishing pass, or when asked to add captions, a title card, transitions, grade, grain, or letterbox to a video. Includes fallbacks for old/stripped ffmpeg builds that lack drawtext and xfade.
---

# Cinematic finishing with ffmpeg

The single biggest lever for making footage read as *film* is not the shots — it's the finishing
pass. Grade + letterbox + grain + vignette does more than any transition. Proven on the *Ember Vow*
short (assembled AI clips → looked like a film after one pass).

**Order of impact** (do the top ones first; each is a real step up):
1. **Cinemascope letterbox** — the strongest single "this is cinema" signal.
2. **Color grade** — unifies separately-generated shots into one graded look.
3. **Film grain + vignette** — texture and focus; kills the flat "digital AI" surface.
4. **Title card + sparse captions** — narrative spine + film identity (esp. for silent pieces).
5. **Transitions + fades** — editorial rhythm.

---

## 0. First, probe your ffmpeg — builds vary wildly

Old/stripped builds are common and **may lack `drawtext` and `xfade`**. Check before designing:

```bash
ffmpeg -filters | grep -iE "drawtext|xfade|curves|noise|vignette|crop|pad|overlay|fade"
```

- No `drawtext` → render captions as PNGs (§4) and `overlay` them.
- No `xfade` → use dip-to-black transitions via segment concat (§5), not crossfades.

---

## 1. The finishing filter chain (grade + letterbox + grain + vignette)

Applied to the whole assembled video, in one pass. Tested values (restrained, not oversaturated):

```
curves=r='0/0.02 0.25/0.21 0.5/0.5 0.75/0.79 1/0.99':b='0/0.07 0.5/0.49 1/0.93',
hue=s=1.08,
vignette,
noise=alls=3:allf=t,
crop=1920:930:0:75,pad=1920:1080:0:75
```

- **curves** — lifts blue in shadows (`0/0.07`) and cuts it in highlights (`1/0.93`) → teal shadows,
  warm highlights; slight S on red for filmic contrast. Keep it subtle; the generative default is
  *over*-graded, so restraint reads more filmic.
- **hue=s=1.08** — gentle saturation. (`eq` may be absent on old builds; `hue` is widely present.)
- **noise=alls=3:allf=t** — subtle temporal film grain. **See §3 for the file-size trap.**
- **crop=1920:930:0:75, pad=1920:1080:0:75** — cinemascope letterbox (~2.06:1): crop 75px off top and
  bottom, pad the bars back. **75px bars are content-safe** for centered full-body shots (verify feet
  aren't clipped); go gentler (60px) if subjects reach frame edges, harder (100px+) for more drama.

---

## 2. Fades and dip-to-black act breaks

- **Open/close fades:** `fade=t=in:st=0:d=1.2` and `fade=t=out:st=<end-1.2>:d=1.2`.
- **Mid-video dips can't be done with chained `fade`** — a `fade=out` stays black forever after its
  end. To dip-to-black at an internal act break, split into segments, fade the *end* of the outgoing
  segment and the *start* of the incoming one, and concat (§5).

---

## 3. The film-grain file-size trap (important)

Temporal grain (`allf=t`) makes every frame unique, so x264 can't compress it. On one render,
`noise=alls=6` at crf 18 produced a **400 MB** 56s clip. Dropping to `noise=alls=3` at **crf 20,
preset slow** gave **35 MB** with no visible loss. Rules of thumb:

- Keep grain light (`alls=2–4`).
- Use `-crf 20 -preset slow -movflags +faststart`.
- If size is still high, grain is almost always the cause.

---

## 4. Captions & title cards without `drawtext` (PIL → PNG → overlay)

When `drawtext` is missing (or for nicer typography/letter-spacing), render text as a transparent
PNG and overlay it. Generate with Pillow:

```python
from PIL import Image, ImageDraw, ImageFont
W, H = 1920, 1080
img = Image.new("RGBA", (W, H), (0,0,0,0)); d = ImageDraw.Draw(img)
font = ImageFont.truetype(r"C:\Windows\Fonts\georgia.ttf", 60)   # serif reads cinematic
text = "They said it died with him."
bb = d.textbbox((0,0), text, font=font); tw = bb[2]-bb[0]
x = (W-tw)//2 - bb[0]; y = int(H*0.82) - bb[1]                    # lower third
for ox,oy,a in [(3,3,190),(2,2,160),(-1,2,120),(0,0,200)]:        # soft shadow for readability
    d.text((x+ox,y+oy), text, font=font, fill=(0,0,0,a))
d.text((x,y), text, font=font, fill=(245,245,245,255))
img.save("caption.png")
```

For a **title card**, use a bold face at ~100px, centered (`y≈0.46`), with manual letter-tracking
(draw char by char, adding a few px between). Overlay each PNG with a timed alpha fade:

```
-loop 1 -t <dur> -i caption.png
[1:v]format=rgba,fade=t=in:st=8.1:d=0.6:alpha=1,fade=t=out:st=10.6:d=0.6:alpha=1[cap];
[base][cap]overlay=0:0:shortest=1[out]
```

- `fade=...:alpha=1` **is** supported even on old builds (unlike `drawtext`'s animated alpha).
- Multiple non-overlapping captions → chain overlays: `[0][c1]overlay[a];[a][c2]overlay[b];...`.
- A **silent** piece needs a text arc to carry story: setup line → the turn → **title card** at the
  end. The end title is the biggest "this is a film" signal after letterbox.

---

## 5. Transitions without `xfade` (dip-to-black via segments)

No `xfade` → split, fade the seams, concat:

```bash
# split into N equal segments (re-encode for clean cut points)
for i in $(seq 0 $((N-1))); do
  ffmpeg -y -ss $((i*DUR)) -t $DUR -i in.mp4 -c:v libx264 -crf 16 -preset veryfast -pix_fmt yuv420p seg_$i.mp4
done
# dip-to-black at a chosen boundary: fade out end of A, fade in start of B
ffmpeg -y -i seg_A.mp4 -vf "fade=t=out:st=<dur-0.28>:d=0.28" ... seg_Ad.mp4
ffmpeg -y -i seg_B.mp4 -vf "fade=t=in:st=0:d=0.28"          ... seg_Bd.mp4
# concat (copy — no re-encode; all segments must share codec params)
printf "file 'seg_0.mp4'\nfile 'seg_Ad.mp4'\n..." > list.txt
ffmpeg -y -f concat -safe 0 -i list.txt -c copy dipped.mp4
```

Place dips at **story act-breaks** (e.g. the tonal turn, the pre-climax), not on every cut — one or
two is editorial; many looks dated. Then run the §1 finishing pass on `dipped.mp4`.

---

## 6. Editorial judgment (don't over-transition)

- **Hard cuts are what real films use.** The film look comes from grade/letterbox/grain, not from
  dissolving every cut. Reserve transitions for meaningful beats.
- **Keep the watermark decision explicit.** Provider watermarks (e.g. Flow's ✦) can be removed
  in-place with `delogo=x=..:y=..:w=..:h=..` **if** they sit over low-detail areas — but only if the
  user wants it gone; some want it kept.
- **Verify by extracting frames**, not by trusting the render: pull a frame at each caption, each
  dip, and a couple of grade checks before delivering.
- **Deliver silent if you can't source music** — say so plainly rather than faking it; muxing a
  user-provided track is a trivial final step (`-i video -i music -c:v copy -c:a aac -shortest`).

---

## 7. One-block reference (full finish, single pass)

```bash
ffmpeg -y -i dipped.mp4 -loop 1 -t <dur+0.2> -i caption.png -filter_complex "\
[0:v]curves=r='0/0.02 0.25/0.21 0.5/0.5 0.75/0.79 1/0.99':b='0/0.07 0.5/0.49 1/0.93',\
hue=s=1.08,vignette,noise=alls=3:allf=t,crop=1920:930:0:75,pad=1920:1080:0:75,\
fade=t=in:st=0:d=1.2,fade=t=out:st=<end-1.2>:d=1.2[base];\
[1:v]format=rgba,fade=t=in:st=8.1:d=0.6:alpha=1,fade=t=out:st=10.6:d=0.6:alpha=1[cap];\
[base][cap]overlay=0:0:shortest=1[out]" \
-map "[out]" -c:v libx264 -crf 20 -preset slow -pix_fmt yuv420p -movflags +faststart -an out.mp4
```
