# Banner generation

`banner.png` (used in the README) is `banner-raw.jpg` with real typography
composited on top via ImageMagick — DM Serif Display for the name, DM Sans
for the title/tagline, matching shivanshsen.com's actual type tokens. The
mark itself was left untouched from generation; no AI-rendered text.

## Model

Gemini 3 Pro Image ("Nano Banana Pro"), `gemini-3-pro-image-preview`, called
directly against the Google AI Studio API at 2K resolution.

## Prompt

```
An ultra-wide letterbox banner, aspect ratio approximately 3.5:1 — much wider
than tall, like a bookmark or a ruler, absolutely NOT square, NOT 16:9, NOT a
tall or medium frame. Strict two-panel diptych composition split at 58% of
the width with a hard invisible boundary. LEFT PANEL (0% to 58% of width):
pure flat solid off-white #FAFAF8, completely empty — no shapes, no texture,
nothing. RIGHT PANEL (58% to 100% of width, same #FAFAF8 background): a
single bold flat-vector emblem — three or four ribbon-like bands in solid
opaque rose-burgundy (#A82E4B) and solid opaque muted violet-purple
(#6B5B95), weaving over and under each other into a compact woven knot/seal
shape. The emblem must be SMALL RELATIVE TO THE PANEL and fully contained
with generous empty margin on every side — at least 15% of the frame height
as clear background space above, below, and to the right of the emblem.
Nothing may touch or cross any edge of the frame. Flat poster-print style,
solid opaque colors only, no gradients, no drop shadows, crisp hard edges.
No text, no logos, no watermark.
```

The woven-ribbon motif is a deliberate visualization of the "workforce
orchestration" idea already on the LinkedIn banner and site copy — human and
AI threads woven into one form — not generic circuit/robot AI iconography.

## favicon.svg (astro-site)

The site's `favicon.svg`/`favicon.ico`/`apple-touch-icon.png` are a vector
trace of this exact knot mark (not a new generation) — `potrace`, one binary
mask per flat color (fuzzy color-match + morphological open/close to remove
JPEG-noise speckles), traced separately, recombined with the exact brand hex
values as fills rather than the JPEG-drifted sampled colors. Two earlier
directions were tried and rejected for the favicon/logo use case specifically
(not the banner, which was already approved): a terracotta/indigo "earth"
palette recolor, and a simplified 2-crossing "pretzel knot" — both dropped in
favor of tracing the original 4-crossing knot as-is, at the user's request.

## divider.png

A second, separate abstract graphic used as a section divider in the
profile README, generated to represent deploying AI agents across multiple
platforms (Salesforce, Anthropic, Gemini, AWS, and others) — deliberately
*not* a literal collage of those companies' actual logos (a real trademark
gray area), staying in the same abstract woven-ribbon language as the header
mark instead, just with more strands.

Same model (Gemini 3 Pro Image, 2K). Requested background was fully
transparent; the model instead rendered a visible checkerboard pattern baked
into JPEG pixels (no real alpha channel came back) — recovered by color-keying
the two checker tones out via ImageMagick, then cropping the tail (where the
requested fade-to-transparent partially failed to key cleanly) and replacing
it with a proper programmatic alpha gradient.

```
A very wide, short horizontal divider graphic, aspect ratio approximately
6:1, on a completely transparent background — no background color, no canvas
fill, fully transparent PNG. A flat-vector abstract emblem: five thin
ribbon-like strands of different solid opaque colors — rose-burgundy
(#A82E4B), muted violet-purple (#6B5B95), deep teal (#2A6F6F), warm gold
(#C9922E), and slate blue (#3B5A78) — running horizontally from the left
edge, gradually weaving over and under each other, converging and braiding
tightly together in the center of the frame into a single dense compact
braided knot, then the strands continue on and fade out (taper to nothing,
becoming fully transparent) before reaching the right edge. Symbolizes many
separate systems converging into one unified practice. Flat poster-print
style, solid opaque colors only, no gradients except the fade-to-transparent
taper at the ends, no drop shadows, crisp hard edges, thin strand width
relative to the frame height. No text, no logos, no watermark.
```

## divider.gif (tried, reverted)

An animated version of `divider.png` was tried and shipped briefly (Gemini
Omni Flash, image-to-video, anchored on `divider.png` as the source image).
It read fine on inspection but looked worse than the static original once
actually placed on the profile — reverted back to `assets/divider.png`, and
the gif file removed from the repo. Kept as a note here so the attempt
isn't repeated blind: a text-only prompt rewrite of the same motif (no
source image) failed worse first, coming back as a 3D-shaded
interlocking-ring "atom" shape instead of the flat vector look — anchoring
on the real source image fixed *that* specific failure, but the animated
divider still wasn't an improvement over the plain static image for this
particular spot on the page.

## avatar-sandbend.gif / avatar-sandbend.mp4

A short looping animation of the GitHub avatar (an illustrated character
portrait, turban and robe, already depicted mid-sandbending with a swirl of
sand in his raised palm), used in the README's right column. `.mp4` (with
audio) is kept in the repo for potential reuse elsewhere later; `.gif`
(silent, no audio track — GIF can't carry one) is what's actually embedded,
since GitHub strips `<video>` tags from README markdown entirely, so a real
video element can't render there regardless of format.

Two prior attempts before this one:
1. A fresh text prompt (no reference image, no established character) —
   came back as a generic 3D-shaded interlocking-ring shape, unrelated to
   the actual brand mark.
2. Gemini Omni Flash, image-to-video anchored on the real avatar image —
   the sand motion and art-style fidelity were good, but the framing
   cropped into the turban and clipped the sand at the frame edge no matter
   how the source was padded/cropped in post; the model's own generated
   composition didn't leave enough margin to fully contain in a square crop.

Switched to Veo 3.1 Fast (`veo-3.1-fast-generate-preview`) for the shipped
version, using its `VideoGenerationReferenceImage` / `reference_type="asset"`
feature — built specifically to preserve a subject's appearance across
generated motion, rather than a general image-to-video "animate this frame"
task. The prompt explicitly named Avatar: The Last Airbender's sandbending
visual language (circular martial-arts-style bending gestures, sand arcing
in ribbons/rings, glinting grains at the trailing edge) and demanded
generous margin ("at least 25% clear background margin... nothing may touch
or cross any edge of the frame, even at the widest point of the sand's
arc") — the character came back fully contained with real margin on the
first generation, no post-hoc padding needed this time. Reference images
force an 8-second duration on Veo 3.1 (not adjustable). Model also generated
real ambient audio (not near-silent, unlike the Omni Flash attempts) — audible
in the `.mp4`, dropped in the `.gif`.

Cropped to a square via `ffmpeg crop` (visually verified across multiple
sampled frames for edge containment, not just the first frame) rather than
ping-ponged — the first and last frames of the 8s clip already closely
matched, so a straight forward loop was clean without needing a
reverse-concat.

```
A chest-up portrait shot of the exact character shown in the reference
image: a bearded man with glasses, a red-and-gold turban, and an ornate
red-and-gold robe, illustrated in the same flat 2D animated art style as
the reference, against the same warm blue-to-orange desert sunset gradient
sky. Locked-off static camera, no zoom, no pan, no camera movement, no
cuts. The character himself, his face, turban, and robe stay completely
still and unchanged throughout, exactly matching the reference image's
design and colors. He is Avatar-The-Last-Airbender-style sandbending: with
a slow, deliberate, martial-arts-like circular gesture of his raised hand,
he draws a stream of golden desert sand up from below and bends it through
the air in smooth sweeping arcs and ribbons around his hand and shoulder,
the way sandbenders and earthbenders control sand and earth in Avatar: The
Last Airbender — controlled, flowing, rhythmic, with the sand catching the
warm sunset light and glinting as fine individual grains at the trailing
edges of each arc. The character is framed small and centered with
generous empty space on every side — at least 25% clear background margin
above his turban, and to the left and right of him and the sand — so that
nothing, neither the character nor the sand, ever touches or crosses any
edge of the frame, even at the widest point of the sand's arc. Flat
illustrated animation style throughout, no photorealism, no 3D rendering.
No text, no logos, no watermark, no dialogue, no audio, no music,
completely silent. The motion should be smooth and cyclical enough to loop.
```

## Stack badges (assets/badge-*.svg)

`salesforce`, `amazonaws`/`aws`, and `openai` were all removed from the
[simple-icons](https://github.com/simple-icons/simple-icons) dataset shields.io
draws from — confirmed by checking simple-icons' own data file directly, not
assumed. `badge-salesforce.svg` is hand-built: the cloud mark traced (potrace)
from a reference image the user supplied, with the "salesforce" wordmark
inside it removed via a flood-fill (not blurred/closed — the text sat as a
fully enclosed island inside the cloud silhouette, so flood-filling it
preserved the true outer cloud contour exactly, where morphological closing
at any radius that fully sealed the text also visibly deformed the cloud's
outer shape). `badge-aws.svg` and `badge-codex.svg` use icon marks from
[lobehub/lobe-icons](https://github.com/lobehub/lobe-icons) (MIT-licensed),
which still carries these. All three badges hand-replicate shields.io's own
flat-square SVG structure and icon-offset math (verified empirically by
diffing a real shields.io badge with vs. without a logo) rather than
guessing dimensions. Codex's background was changed from plain black to
OpenAI's actual brand teal (#10A37F).
