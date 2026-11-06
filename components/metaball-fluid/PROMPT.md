Build a "Metaball Fluid" ambient background component as a single HTML file: 7 hot-pink gooey metaball blobs drift and merge fluidly via a high-contrast canvas threshold, with the cursor adding an extra attracting blob. Canvas 2D, zero dependencies, one inline script.

PALETTE AND TOKENS
- :root tokens: --c07-bg:#0e060c (near-black ground), --c07-fx:contrast(3) blur(0.4px) (threshold filter on the canvas), --c07-rgb0:255 40 120 (blob colour base), --c07-rgbk:0 30 20 (field-strength slope), --c07-scap:1e9 (field cap), --c07-cap:#ffb3d9 / --c07-cap-o:.72 (caption).
- Light variant in :root[data-theme="light"]: --c07-bg:#f7eaf2, --c07-rgb0:242 36 134, --c07-rgbk:-2.6 -3.8 -3.3, --c07-scap:5, caption #8f0a50. Theme hook: ?theme=light|dark query param plus a forever:theme message listener that re-syncs colours from the CSS custom properties.
- A small uppercase caption ("Metaball Fluid", 10px system sans, opacity .72) sits bottom-left.

THE CANVAS
- A full-viewport canvas #c07-canvas inside .c07-wrap (fixed inset 0, flex centred) is the whole page; the canvas carries data-section="stage".
- resize() sets DPR-aware canvas dimensions (DPR = min(2, devicePixelRatio)) and an offscreen canvas at 1/4 resolution — the field is computed on the small buffer for performance, then drawn upscaled with imageSmoothingEnabled=false.
- init() creates 7 blobs (radius 18–48px, random position, slow Lissajous drift speed 0.001–0.004).
- step(): for every pixel of the offscreen buffer, sums each blob's field (r²/(dx²+dy²+1), plus a 120px cursor blob when the pointer is active); where the sum exceeds 1, writes a pink pixel (hue shifts with field strength). Blobs then advance along their Lissajous paths (x = 0.5 + 0.4·cos(p), y = 0.5 + 0.4·sin(p·1.3)).
- Pointer: pointermove/pointerdown update the cursor blob position and activate it; pointerleave deactivates it.
- requestAnimationFrame loop with a document.hidden guard; visibilitychange pauses/resumes; demo-v1 preview lifecycle (demoEnabled from window.__foreverPreview, stopDemo on forever:interactive / pointerdown / click / Tab keydown).

ACCESSIBILITY AND MOTION
- prefers-reduced-motion: a single static frame renders; blobs do not animate.
- The canvas is pure decoration; the caption is a plain div.

WHAT BREAKS
- Raising the offscreen resolution (currently W/4) makes the per-pixel loop much slower — it is the main performance lever.
- Removing the contrast/blur filter (--c07-fx) breaks the gooey merge; blobs render as hard circles.
- The light theme only works through the theme hook; the dark ground is the default.
- Reduced-motion renders one static frame — no animation at all.
