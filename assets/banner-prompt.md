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
