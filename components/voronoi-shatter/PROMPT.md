Build a "Voronoi Shatter" generative mosaic component as a single HTML file: 14 drifting sites partition the canvas into cool-blue Voronoi cells; moving the pointer nudges the nearest site toward the cursor. Zero dependencies, canvas 2D, one inline script. Light-theme ready.

PALETTE AND TOKENS
- :root theme tokens (dark defaults): --gg01-bg #0d1b2a (ground), --gg01-cap #7fd1ff (caption), --gg01-cell-s 55 / --gg01-cell-l 30 (cell saturation/lightness), --gg01-dot rgba(220,245,255,.8) (site dots). Light variant in :root[data-theme="light"]: paper #eef4f9 ground, #0072b2 caption, cell-l 65.
- Caption: ui-monospace 11px, letter-spacing .5px, opacity via --gg01-cap-o.

THE MOSAIC
- A full-viewport canvas #c (tagged data-section="stage"), display:block, width/height 100%.
- Theme hook in <head> before first paint: read ?theme=light|dark from the URL and stamp data-theme on <html>; listen for {type:'forever:theme'} postMessage to switch at runtime. The canvas reads all its colors from the CSS tokens via getComputedStyle in syncTheme() (called at start and on every switch).
- resize(): DPR = min(devicePixelRatio, 2); W/H from clientWidth/clientHeight; ctx.setTransform(DPR,0,0,DPR,0,0). Re-run on resize.
- N=14 sites, each {x,y in 0..1, vx/vy rnd(-1,1)*.04, hue rnd(180,210)}. Per frame each site moves x += vx*0.004 (same for y), bouncing off the unit square.
- Rendering: coarse pixel-grid Voronoi — cell = max(5, floor(min(W,H)/64)); for each grid cell find the nearest site (O(N*cells)); fill the cell with hsl(hue, cellS + bi*1.5%, li%) where li = cellL + sin(t*0.001+bi)*8 + (1-min(1,bd*6))*20, so cells brighten near their site. Draw site dots (radius 1.6) on top.
- pointermove over the canvas: record the pointer in unit space and nudge the nearest site 20% of the way toward it; pointerleave clears it.
- A small lowercase caption ("voronoi shatter", 11px mono, pointer-events:none) sits bottom-left.

SCRIPT (one IIFE, no libraries)
- rAF loop; visibilitychange cancels the frame when hidden and restarts it when visible.
- prefers-reduced-motion: call step(0) once for a static mosaic and never start the loop.

ACCESSIBILITY AND MOTION
- Reduced motion renders one static frame — the mosaic appears as a still geometric texture.
- The canvas is pure decoration; the caption is plain text.

WHAT BREAKS
- Reading theme colors once and never calling syncTheme() again desyncs the canvas from a runtime theme switch.
- Shrinking the cell divisor (bigger cells) or raising N much above 14 blows the O(N*cells) budget — large screens stutter on low-end devices.
- Background must be changed in both the CSS tokens and the canvas syncTheme defaults, or the first paint flashes the wrong ground.
