Build a "Sandfall Hourglass" cellular-automaton sand simulation as a single HTML file: amber sand grains fall through an hourglass-shaped cavity, pile with an angle of repose at the base, and auto-flip when the falling bulb drains; clicking reverses gravity. Zero dependencies, one small script, canvas 2D only, full light-theme re-theme support.

PALETTE AND TOKENS
- :root tokens (dark defaults): --sfh-bg:#1a120b (near-black amber ground), --sfh-accent:#e8a44c (caption/lines), --sfh-cap-o:.34, --sfh-wash (glass cavity wash rgba), --sfh-line (glass outline rgba), --sfh-s0..--sfh-s5 (six-stop sand shade ramp, dark #3a2310 to light #f6c878).
- A :root[data-theme="light"] block re-points every token (warm paper ground #f6f2ea, brown-ink sand, darker amber accent, caption opacity .62) — this is the shipped light theme; preserve it.
- syncTheme() reads the tokens back from getComputedStyle into the JS palette object C (bg, wash, line) and the PAL array, then re-bakes the glass layer — a theme flip re-colors everything with no hardcoded colors left in JS.

THE HOURGLASS
- A full-viewport #sfh-c canvas (display:block, width/height 100%, tagged data-section="stage"); html,body overflow hidden. A small uppercase caption ("Sandfall Hourglass", 10px, letter-spacing .09em) sits bottom-left.
- A head IIFE reads ?theme=light|dark before first paint and stamps data-theme (no dark flash). A forever:theme postMessage listener re-themes live (setAttribute data-theme + syncTheme()).
- The grid is a cellular automaton: Int8Array cells (0 empty, 1..6 sand shade), Uint8Array mask carved by the hourglass equation halfW = neck + (bulge-neck)*|2v-1|^1.35, neck=COLS*0.05, bulge=COLS*0.40. Grain size cell = max(2, round(min(W,H)/110)) so it scales with the frame. The top bulb starts filled with pseudo-random shades.
- Static layers (cavity wash + outline) are pre-rendered once per resize into an offscreen canvas by bakeBg() — never per frame.

SCRIPT (one IIFE, no libraries)
- step(): scans the grid in gravity direction (alternating scan bias via flip&1 for symmetry); grains fall straight into empty mask cells, otherwise topple diagonally into empty neighbours for the angle-of-repose pile. Every 4th flip, if the falling bulb has drained (<2 grains) and the other has settled (>4), gravity auto-reverses — but only while demoEnabled.
- frame(): fixed-timestep loop (acc>=33, max 3 steps, +6 extra steps on pointermove kick), then draw(): bg fill, pre-baked glass drawImage, grain fillRects from PAL.
- Interactions: pointerdown/click/keydown(Tab)/forever:interactive stops the attract demo; canvas click flips dir (gravity reverses, grains re-settle); pointermove adds sim steps.
- visibilitychange pauses/resumes the rAF loop; prefers-reduced-motion runs 900 steps once and draws a single static frame.

ACCESSIBILITY AND MOTION
- No interactive controls beyond click-to-reverse-gravity; caption is pointer-events none.
- @media (prefers-reduced-motion:reduce): the simulation fast-forwards 900 steps to a static settled frame; no loop starts.

WHAT BREAKS
- Removing bakeBg() (or re-baking it every frame) tanks per-frame perf — the full grid fillRect scan is the whole perf story.
- Breaking syncTheme()'s re-bake leaves the old palette's glass layer on screen after a theme flip; note syncTheme() references draw() which must be defined (function hoisting covers it).
- The demo-v1 lifecycle marker (meta name="forever-preview-lifecycle") and the demoEnabled stop-on-input wiring control preview embedding — dropping them breaks the preview contract.
- auto-flip is physics-driven, not clock-accurate — do not promise precise timing. Per-frame full grid scan is O(COLS*ROWS): heavy on large viewports with small cell sizes.
