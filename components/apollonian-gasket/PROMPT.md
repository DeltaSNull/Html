Build an "Apollonian Gasket" ambient background component as a single HTML file: recursive Apollonian circle packing with infinite slow zoom; circles shift from gold to blue by depth, with vignette and pointer/click nudge. Canvas 2D, zero dependencies, one inline script.

PALETTE AND TOKENS
- :root tokens: --apg-bg:#0d1b2a (dark navy ground), --apg-cap:#e0a458 (caption), --apg-cap-o:.34, --apg-c0:224,164,88 / --apg-c1:114,124,208 (ends of the depth ramp the ring strokes interpolate along), --apg-fill-a:0.06 (shallow-fill alpha), --apg-vig:13,27,42 / --apg-vig-a:0.9 (vignette wash).
- A [data-theme="light"] block repoints the tokens to a pale blue-grey ground (#eef4f9) with brown-to-navy hues; ?theme=light or a {type:'forever:theme'} postMessage switches at runtime.
- A small uppercase caption ("Apollonian Gasket", 10px system sans, opacity .34) sits bottom-left.

THE CANVAS
- A full-viewport canvas #apg-c (display:block, width/height 100%) is the whole page; it carries data-section="stage".
- size() sets DPR-aware canvas dimensions (DPR = min(2, devicePixelRatio)) and re-runs on resize; centre (cx, cy) and radius R = min(W,H)·0.46 are derived from it.
- The gasket is built once with Descartes' theorem: circles are {b: curvature, x, y: curvature-weighted centre, d: depth}. An outer bounding circle (b=-1) and two inner kissing circles (b=2) seed the packing; descartes() finds each 4th tangent circle, build() recurses to MAX=7, stopping when curvature exceeds 900 (c4.b>900).
- draw(): phase = (t·0.0009) % 1; zoom = 1 + phase^1.3·2.6 + nudge·0.25 (a slow breathing zoom, 1.0 → ~2.4 then loop); visD = 1 + phase·maxDepth·1.15 makes deeper circles fade in as the zoom advances. Each circle is stroked with a colour interpolated by depth (heat = min(1, d/6)) from --apg-c0 (gold) to --apg-c1 (blue), lineWidth max(0.4, 0.9 − d·0.08)/zoom, faint fill on shallow circles (d<3). A radial-gradient vignette sinks the edges.
- Pointer move sets nudge=1, click sets nudge=1.6 (decaying ·0.93 per frame) to nudge the zoom.
- requestAnimationFrame loop (t += 16 per frame) with visibilitychange pause/resume; theme tokens are re-read from CSS on a theme flip.

ACCESSIBILITY AND MOTION
- prefers-reduced-motion: draws a single static frame (t=300) and never starts the loop.
- The canvas is pure decoration; the caption is a plain div.

WHAT BREAKS
- Background color is hardcoded in both CSS and the canvas fillStyle — change both to retheme.
- Depth-fade bands shift if MAX recursion depth is changed without recalibrating visD.
- Hundreds of arc calls per frame — heavier on low-end devices at high DPR; raising MAX or the curvature cutoff adds circles and draw calls.
- The constant zoom and vignette compete with legible overlaid text — use as a backdrop, not behind copy.
