Build a "Perlin Flow-Field" generative canvas component as a single HTML file: 100 peach-gold particles stream along a value-noise flow field on dark charcoal, with motion-trail fade; the cursor deflects nearby particles within a 120px radius. Zero dependencies, canvas 2D, one inline script, dark by default with a light re-theme.

TOKENS AND THEME
- :root tokens (dark defaults; a [data-theme="light"] block re-themes): --gg09-bg:#1D1917 (ground), --gg09-fade:rgba(29,25,23,0.06) (the per-frame trail wash — it MUST match the background), --gg09-cap:#EEBAA0 / --gg09-cap-o:.45 (caption), --gg09-p-s:80 / --gg09-p-l:65 / --gg09-p-a:0.5 (particle streak hsla, read by JS via syncTheme()).
- A theme hook runs before first paint: ?theme=light|dark in the query string (or data-theme on <html>) avoids a dark flash; a {type:'forever:theme'} postMessage switches at runtime and repaints the trail buffer in the new palette (trails can't crossfade).
- Every canvas color is read from the CSS tokens — never hardcode colors in JS.

THE SIMULATION
- Full-viewport canvas #c09-canvas (display:block, width/height 100%). DPR-aware sizing (DPR capped at 2, setTransform), full repaint + re-init on resize.
- N=100 particles, each: random position, hue 350+Math.random()*60 (peach-gold range), life Math.random()*200.
- Cheap value-noise: three summed sine waves scaled by position and time. Each frame paints the fade wash over the whole canvas (this is the motion trail — no clearRect), then for each particle: sample noise at (x*sc, y*sc, t) where sc=4/min(W,H), map to an angle, advance 1.6px along it (flow speed multiplier).
- Cursor deflection: pointermove/pointerleave on the canvas track the mouse; particles within 120px get their angle nudged away from the cursor by atan2 scaled (1-d/120)*1.5.
- Draw: 1.1px strokes from (x,y) to (nx,ny) in hsla(h, pS%, pL%, pA). life decrements; dead or out-of-bounds particles respawn randomly (Object.assign(p, mk())).
- A small lowercase caption ("perlin flow-field", 11px ui-monospace, opacity .45, pointer-events:none, user-select:none) sits bottom-left.

LIFECYCLE AND MOTION
- visibilitychange pauses/resumes the rAF loop. prefers-reduced-motion renders a static pre-render: 60 frames drawn once at 16ms steps, no loop.
- Under reduce, the theme-switch message handler re-draws the 60-frame pre-render after repainting.

WHAT BREAKS
- Changing --gg09-bg without matching --gg09-fade leaves colored ghost trails; the JS canvas fill uses the same tokens, so keep them in sync there too.
- Removing the DPR setTransform makes streaks blurry on retina; removing the resize re-init freezes particles at the old size.
- The noise is a sine approximation — raising the spatial scale (sc) makes the flow visibly repetitive rather than truly Perlin-coherent.
