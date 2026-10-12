Build a "Boids Flocking" ambient background component as a single HTML file: up to 60 lime-green triangular boids flocking with alignment, cohesion, and separation on a dark canvas, fleeing the cursor, with motion-trail fade. Zero dependencies, canvas 2D, one inline script.

TOKENS AND THEME
- :root tokens (dark defaults; a [data-theme="light"] block re-themes): --c08-bg:#0a0e0a, --c08-cap:#a3e635 / --c08-cap-o:.5 (caption), --c08-fade:rgba(10,14,10,0.28) (the per-frame trail wash — it MUST match the background or trails smear the wrong color), --c08-s:80 / --c08-l:62 (boid hsl saturation/lightness, read by JS via syncTheme()).
- A theme hook runs before first paint: ?theme=light|dark in the query string (or data-theme on <html>) avoids a dark flash; a {type:'forever:theme'} postMessage switches at runtime.

THE SIMULATION
- Full-viewport canvas #c08-canvas. DPR-aware sizing (DPR capped at 2, setTransform), re-init on resize.
- Boid count: Math.min(60, Math.round(W*H/4200)). Each boid: random position, velocity ±1, hue 90+Math.random()*60 (lime-green range).
- Per frame: paint the fade wash over the whole canvas (this is the motion trail — no clearRect). For each boid, O(n²) neighbor loop with radius R=34: alignment (steer toward average velocity ×0.04), cohesion (steer toward centroid ×0.0009), separation (push apart within Rs=18 ×0.05).
- Cursor flee: pointer within pd<90 gets pushed away at 0.5 strength. Track pointermove/pointerleave on the canvas.
- Speed clamp maxS=2.6. Toroidal wrap-around boundaries (exit right, enter left).
- Draw: save/translate/rotate to velocity angle, fill triangle (5,0)→(-3,3)→(-3,-3) in hsl(h, BS%, BL%), restore.

LIFECYCLE AND MOTION
- visibilitychange pauses/starts the rAF loop. prefers-reduced-motion renders a single static frame.
- demo-v1 preview lifecycle: demoEnabled reads window.__foreverPreview; any pointerdown/click/keydown (except Tab) or a forever:interactive event stops the attract loop and hands control to the session.
- A small uppercase caption ("Boids Flocking", 10px ui-monospace) sits bottom-right, pointer-events none via user-select:none.

WHAT BREAKS
- Raising the boid cap far above 60–80 tanks low-end mobile (O(n²) neighbor search).
- Changing --c08-bg without matching --c08-fade leaves colored ghost trails.
- Removing the DPR setTransform makes boids blurry on retina; removing the resize re-init freezes the flock at the old size.
