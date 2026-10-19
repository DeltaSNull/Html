Build a "Reaction Diffusion" generative component as a single HTML file: a Gray-Scott reaction-diffusion simulation morphing between coral, spots, and maze patterns on a 128×128 grid, rendered in a teal-to-bone color ramp on near-black. Move the pointer to seed fresh chemical. Zero dependencies, canvas 2D, one inline script. Light-theme ready.

PALETTE AND TOKENS
- :root dark defaults: --rd-bg:#08120f, --rd-s0..--rd-s4 as r,g,b triplets (the five density-ramp stops: deep bg → teal → accent #39d98a → bone), --rd-cap for the caption.
- :root[data-theme="light"]: inverted ramp — dark teal on pale mint #eef7f2. The five stops are read live from the CSS tokens; a theme flip rebuilds the cached ramp immediately.
- Theme hook: ?theme=light|dark in a pre-paint script (no flash) plus a forever:theme postMessage listener.

THE FIELD
- #rd-c: full-viewport canvas, image-rendering:pixelated, drawing at sim resolution (128×128) while CSS upscales — sharp but blocky is the look. A small uppercase caption sits bottom-left.
- Body overflow hidden; nothing scrolls.

SCRIPT (one IIFE, no libraries)
- Gray-Scott solver on fixed 128×128 Float32Arrays with buffer-swap (a/a2, b/b2), 9-point Laplacian, diffusion DA=0.18 / DB=0.09.
- params(): feed/kill drift through pattern territory (coral ↔ spots ↔ maze) on slow sines of t.
- step() runs 5 iterations per frame (8 while nudging); draw() maps (a−b) through a precomputed 256-entry ramp (Uint32) into putImageData.
- Pointer seeds a fresh splash of chemical B in a ±2 cell radius around the cursor.
- visibilitychange pauses/resumes the rAF loop; prefers-reduced-motion runs 3200 pre-steps and draws one static frame.

ACCESSIBILITY AND MOTION
- The canvas is a decorative simulation field; caption is informational.
- @media (prefers-reduced-motion:reduce): 3200 pre-steps then one static draw — no loop.

WHAT BREAKS
- Breaking the buffer-swap pattern (reading from a2/b2 mid-step) produces garbage patterns or runaway feedback.
- Raising GW/GH makes the per-pixel loop heavy fast — the CPU cost is O(grid² × steps).
- Forgetting to sync the theme (read CSS tokens, rebuild ramp, repaint) leaves the canvas drawing dark colors on the light theme.
- The pixelated upscale is intentional; smoothing it kills the aesthetic.
