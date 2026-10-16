Build a "Phantom Polytope" ambient canvas component as a single HTML file: a glowing sage-green 4D tesseract (hypercube) wireframe rotating through itself via dual perspective projection, edges glowing hotter by 4D depth. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- :root tokens (dark defaults): --pp-bg:#1B1E1A (ground, hardcoded), --pp-fade:rgba(27,30,26,.30) (trail fade), --pp-blend:lighter (additive stacking), --pp-edge-a:140,210,110 / --pp-edge-b:230,210,170 (edge gradient RGB), --pp-node:224,235,219 (vertices), --pp-ea:1 (edge alpha), --cf12-cap:#B1CCA4 / --cf12-cap-o:.36 (caption).
- Light variant in :root[data-theme="light"] — sage-on-paper palette, --pp-blend:source-over.
- Theme hook: ?theme=light|dark query param or a {type:'forever:theme'} postMessage switches at runtime with no dark flash.

THE SCENE
- A full-viewport canvas#cf12-c (data-section="stage") with a small uppercase caption ("Phantom Polytope", 10px monospace, letter-spacing .14em, opacity .36, pointer-events:none) bottom-left.
- 16 tesseract vertices: all (±1)^4. Edges connect pairs differing in exactly one coordinate (32 edges).
- Per frame: rotate in the XW/YW/ZW planes (speeds 0.31 / 0.23 / 0.17), 4D→3D perspective (wd=2.4), a 3D Y-rotation for readable depth (0.27 speed), then 3D→2D perspective (dd=3.2), scaled to fit (S*0.62).
- Edges are stroked with an RGB ramp from --pp-edge-a to --pp-edge-b by depth; the 4D "innerness" (w4) pushes the hue hotter. Line width scales with depth. Vertices are drawn as glowing nodes.
- Trail effect: each frame paints --pp-fade over the canvas instead of clearing (additive blend on dark).
- Pointer move / click on the canvas boosts rotation speed (cap 3, click sets 4), decaying at 0.95 per frame.

SCRIPT (one IIFE, no libraries)
- DPR-aware canvas sizing (capped at 2) with a resize handler.
- rAF render loop; visibilitychange pauses/resumes to save cycles.
- prefers-reduced-motion: renders one static frame and stops.
- demo-v1 preview lifecycle: the attract loop runs until the first deliberate input (forever:interactive event, pointerdown, or keydown), then stops and never resumes; manual pointer boost keeps working.

ACCESSIBILITY AND MOTION
- The canvas carries no aria semantics — it is pure decoration; add a role/label if it must be announced.
- Reduced motion shows a single static frame.

WHAT BREAKS
- Painting a solid background each frame instead of the fade fill kills the motion trails.
- Changing --pp-blend away from lighter on dark muddies the additive glow.
- The background is hardcoded (#1B1E1A) — repoint --pp-bg AND ctx.fillStyle to retheme.
