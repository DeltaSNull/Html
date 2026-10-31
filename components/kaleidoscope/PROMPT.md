Build a "Kaleidoscope" interactive generative-art component as a single HTML file: a 10-fold mirrored kaleidoscope of rotating polygon blobs in pink and magenta on a deep plum field. Tap/click spins the pattern and regenerates the blobs. Canvas 2D, zero dependencies.

PALETTE AND TOKENS
- :root tokens: --gg10-bg:#0c0612 (deep plum ground), --gg10-cap:#ff9ad1 (caption),
  --gg10-fi-s:85 / --gg10-fi-l:62 / --gg10-fi-a:0.85 (blob fill saturation, lightness, alpha),
  --gg10-st-s:90 / --gg10-st-l:75 / --gg10-st-a:0.9 (blob stroke). Every canvas color is read
  from these tokens once at start and once per theme switch.
- A :root[data-theme="light"] block carries the light re-theme; ?theme=light or a
  {type:'forever:theme'} postMessage switches at runtime.

THE KALEIDOSCOPE
- A full-viewport .stage (overflow:hidden) holds the #c canvas and a small monospace caption.
- Each frame: clear to the bg token, then draw the pattern: for SEG=10 segments, rotate by i*ang
  and flip every other segment with scale(1,-1), clipping each segment to its wedge
  (arc of ±ang/2, radius 1.5R).
- M=7 blobs live inside one wedge (radius 0.1–0.9R, angle within ±0.45·ang, bouncing at ±0.55·ang —
  a blob outside the wedge is clipped away in every segment, so keep the bounds).
- Blobs are polygons (3–6 sides) drawn with fill hsla(hue, fiS, fiL, fiA) and a 1.2px stroke of the
  hue shifted +40. The whole wheel rotates at t*0.0002 plus a spin impulse that decays (*=0.95).
- pointerdown: spin+=0.4 and regenerate the blobs.

SCRIPT (one IIFE, no libraries)
- DPR-aware sizing (capped at 2); on resize re-init blobs.
- visibilitychange pauses the rAF loop when the tab is hidden.
- Theme hook: ?theme= bootstrap runs before first paint (no dark flash); the message listener
  re-syncs tokens on forever:theme messages.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce): draw one static frame instead of animating.

WHAT BREAKS
- Blobs drawn outside the ±0.55·ang wedge are clipped away in every segment — near-blank output.
- Background color is hardcoded in both CSS and the fillRect — change both to retheme.
- The reduced-motion path still runs the rAF loop (blobs update silently) while drawing only once.
