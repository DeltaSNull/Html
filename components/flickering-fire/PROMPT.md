Build a "Flickering Fire" ambient background component as a single HTML file: additive-blended ember particles rise from a glowing orange base, flickering like a real flame on near-black; moving the cursor left/right fans the fire sideways. Canvas 2D, zero dependencies, one inline script.

PALETTE AND TOKENS
- :root tokens: --ff-bg:#160a06 (near-black ground), --ff-cap:#ffc78a / --ff-cap-o:.34 (caption), --ff-c0:255,250,210 / --ff-c1:255,160,40 / --ff-c2:200,40,10 (ember gradient stops as "r,g,b"), --ff-a:0.5 (ember alpha), --ff-glow:255,120,30 / --ff-glow-a:0.25 (base glow), --ff-blend:lighter (additive), --ff-fade:rgba(22,10,6,0.28) (per-frame trail wash).
- A [data-theme="light"] block repoints the tokens to a cream ground (#f9f0e6) with saturated flame embers and source-over blending; ?theme=light or a {type:'forever:theme'} postMessage switches at runtime.
- A small uppercase caption ("Flickering Fire", 10px system sans, opacity .34) sits bottom-left.

THE CANVAS
- A full-viewport canvas #c (display:block, width/height 100%) is the whole page; it carries data-section="stage".
- size() sets DPR-aware canvas dimensions (DPR = min(2, devicePixelRatio)) and re-runs on resize; reset() puts the flame base at (W/2, H*0.82).
- spawn() pushes a particle at the base with random horizontal spread (W*0.07), upward velocity -(1–2.4), size 8–22 and a random seed; up to 5 spawn per frame (3 on narrow screens).
- frame(): washes the canvas with the --ff-fade trail color, then draws each particle as a radial gradient (hot core --ff-c0 → mid --ff-c1 → transparent --ff-c2) with globalCompositeOperation from --ff-blend ("lighter" on dark). Particles drift with sin(t*2+seed)*0.5, fan sideways by fanX*1.2, decelerate (vy*=0.99) and lose life (0.016/frame, faster above 45% height). Cap is 120 particles.
- A radial glow at the base (radius W*0.25, --ff-glow at --ff-glow-a) is drawn every frame.
- pointermove on the canvas sets fanX from -1 to 1 across the width; pointerleave resets it to 0.
- requestAnimationFrame loop with visibilitychange pause/resume; theme tokens are re-read from CSS via syncTheme() on every theme flip.

ACCESSIBILITY AND MOTION
- prefers-reduced-motion: spawns only up to 30 particles and slows decay (life -= 0.02) — the flame still animates, so it is not a fully static fallback.
- The canvas has no aria label (a known limitation); the caption is a plain div.

WHAT BREAKS
- Removing globalCompositeOperation "lighter" kills the additive glow — embers paint as flat orange blobs instead of light.
- Background color is hardcoded in the CSS tokens (#160a06); repoint --ff-bg (and --ff-fade) for light themes.
- The globals (cv, x, P, t, raf) are not wrapped in a module — they can clash if this file is merged into a multi-component page.
