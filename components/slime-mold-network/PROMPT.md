Build a "Slime Mold Network" Physarum simulation component as a single HTML file: up to 6000 agents self-organize into glowing purple radial veins on a dark canvas, with trails attracted toward the cursor. Canvas 2D, zero dependencies, one inline script, dark by default with a light re-theme.

TOKENS AND THEME
- :root tokens (dark defaults; a [data-theme="light"] block re-themes): --smn-bg:#0c0a14 (ground), --smn-cap:#d9b3ff / --smn-cap-o:.34 (caption), --smn-base:12,10,20 / --smn-span:205,120,255 / --smn-hot:40,90 (trail color ramp as "r,g,b" triplets).
- --smn-base MUST equal the page background — it is the color an empty trail cell paints. The light variant runs negative spans (pale agar ground, dark violet veins) under the same rule.
- The trail ramp is the ONLY color the canvas draws with: colour = base + pow(t,.7)*span, plus a hot boost in the top decile (t>0.75 and t>0.8). All channels are read from the tokens by syncTheme() — never hardcode colors in JS.
- A theme hook runs before first paint: ?theme=light|dark in the query string (or data-theme on <html>) avoids a dark flash; a {type:'forever:theme'} postMessage switches at runtime, re-syncs tokens, and repaints the field immediately.

THE SIMULATION
- Full-viewport canvas #smn-c (display:block, width/height 100%) inside an explicit 100%x100% [data-section="stage"] wrapper (overflow:hidden). DPR-aware sizing (DPR capped at 2, setTransform); resize re-inits everything (trails are lost — accept it or debounce).
- The sim runs on a low-res grid for speed: SCALE=2 (grid = canvas/SCALE), capped at GMAX=240 cells per side with aspect preserved (minimum 80). This cap is perf-critical — the diffuse pass scales with cell count.
- N=min(6000, GW*GH*0.18) agents, seeded in a disc of radius min(GW,GH)*0.34 with outward headings (→ radial veins). Per agent per frame: sense trail at center/left/right (sensor offset SO=8, sensor angle SA=0.55), steer toward the strongest by rotation angle RA=0.42 (randomized ±50%), move SS=1.1 px, wrap edges toroidally, deposit DEP=22 (clamped at 255).
- Pointer attraction: while the pointer is over the canvas (mPow=1, decaying ×0.95/frame), agents within d<90 of the cursor nudge their heading toward it by 0.08*mPow.
- Diffuse + decay: cheap 4-neighbour blur (DIFF=0.16) then multiply by DECAY=0.90, in double-buffered Float32Arrays; swap buffers each frame.
- Paint: per-cell ImageData ramp packed via a Uint32Array view ((255<<24)|(b<<16)|(gg<<8)|r), put to a small offscreen canvas, then upscaled to the full backing store with imageSmoothingEnabled=true and imageSmoothingQuality='high'.
- A small uppercase monospace caption ("Slime Mold Network", 10px, letter-spacing .14em, color var(--smn-cap), opacity var(--smn-cap-o), pointer-events:none) sits bottom-left, fixed.

LIFECYCLE AND MOTION
- visibilitychange stops/starts the rAF loop. prefers-reduced-motion: run 240 simulation steps once, paint once, never loop.

WHAT BREAKS
- Raising GMAX or removing the grid cap makes the diffuse pass far more expensive and drops frames on mobile — the low-res grid IS the performance strategy.
- Resize re-seeds agents and zeroes trails: all veins vanish and regrow. If that is unacceptable, snapshot and restore the trail buffer across resizes.
- Changing --smn-bg without --smn-base (or vice versa) leaves a visible seam between the CSS ground and the painted cells — keep them equal, in CSS and in the light block.
