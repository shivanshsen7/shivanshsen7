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

## divider.gif

An animated version of `divider.png`, used in place of the static image in the
README. Model: Gemini Omni Flash (`gemini-omni-flash-preview`), called
directly via the Gemini API (image-to-video), anchored on the actual
`divider.png` file as the input image rather than a blind text prompt.

A first attempt used a fresh text-only prompt describing the same weaving
motif — it ignored the "flat, no gradients, no shading" instructions and
came back as a 3D-shaded interlocking-ring "atom" shape, closer to a
generic AI-video default than the site's actual flat-vector mark. Feeding
the real `divider.png` as the source image for image-to-video generation
instead kept the output anchored to the true flat style and exact brand
palette.

The model composited the source's transparent background as solid black for
the first frame and a light gray (`srgb(224,224,224)`, coincidentally the
same tone used to key `divider.png`'s own checkerboard artifact — see below)
for the rest of the clip, rather than any real alpha channel — video output
has no alpha channel to preserve in the first place. Recovered via
`ffmpeg colorkey` on that gray, after trimming off the one black lead-in
frame. The raw generation was also not a seamless loop (first and last
frame don't match), so it's ping-ponged (forward + reverse, concatenated)
rather than regenerated — a plain edit, not a second API call — which
guarantees a seamless loop from a single one-directional generation.
Trimmed to ~4.5s of source before ping-ponging and downscaled for file size
(the full untrimmed ping-pong version was 14MB; the shipped version is ~3MB).

```
In a single unbroken scene, locked-off static camera, no camera movement, no
zoom, no pan, no cuts. Animate exactly this reference image: keep the
identical flat, solid-opaque-color, poster-print vector style with hard
crisp edges — no 3D depth, no gradients, no lighting, no shadows, no
shading, no glossy or metallic look, stay perfectly flat like the source.
The five ribbon strands (rose-burgundy, violet-purple, deep teal, warm gold,
slate blue) continue their same gentle weaving and braiding motion, flowing
smoothly over and under each other exactly as they already do in the image,
converging into the same tight braided knot on the right side which keeps
subtly twisting in place. Background stays plain and unchanged. No text, no
logos, no watermark, no dialogue, no audio, no music, completely silent.
Loop-friendly: the last frame should closely match the first frame.
```

`divider.png` is kept in the repo as the static source/fallback asset, not
removed.

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
