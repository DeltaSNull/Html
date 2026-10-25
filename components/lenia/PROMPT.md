Build a "Lenia" smooth-life automaton component as a single HTML file: glowing teal-to-cyan blobs drift and pulse on a dark field via a typed-array convolution sim, rendered at 96x96 and scaled up with CSS. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- :root tokens: --fr04-bg:#06070b (ground), --fr04-bg1:#0c0e18 / --fr04-bg2:#06070b (radial ground gradient), --fr04-blend:screen, --fr04-g1/g2 (static glow, teal at 16%/6%), --fr04-cap:#fff / --fr04-cap-o:.42 (caption), --fr04-r0..r4 (density ramp: #06070b → #0f3c46 → #1dacc9 → #8ce0d4 → #e8f5f0).
- A :root[data-theme="light"] block holds the inverted light palette (ground #eef5f6, blend multiply, ramp #f1f2f6 → #2c5847, caption #123338); ?theme=light in the URL or a {type:'forever:theme'} postMessage switches at runtime, and a theme hook script in <head> stamps data-theme before first paint so there is no dark flash.
- A small uppercase caption ("Lenia", 10px monospace, letter-spacing .08em, opacity .42, user-select:none) sits bottom-left.

THE STAGE
- A fixed, full-viewport stage (#fr04-stage) with a radial ground gradient holds a static glow div (#fr04-glow, 78% circle, never repaints) and the canvas (#fr04-c, 100% x 100%, mix-blend-mode from --fr04-blend).
- The canvas backing store is exactly the 96x96 grid; CSS scales it to the viewport and imageSmoothingEnabled softens the upscale.

SCRIPT (one IIFE, no libraries)
- Sim grid: N=96, Float32Array state A and next B. An annular kernel (R=12) is precomputed once: smooth bump shell peaking at r~0.5, weights normalized, split into koffX/koffY offset arrays.
- Growth: bell(x) = 2·exp(−((x−MU)/SIG)²/2) − 1 with MU=0.15, SIG=0.03, DT=0.1; step() convolves toroidally (wrap on both axes) and clamps to [0,1], allocation-free with a buffer swap.
- Seeding: three gaussian orbium-ish patches (relative centers 0.30/0.42, 0.66/0.36, 0.50/0.68; radii 9, 11, 10) with amplitude 0.85 and noise.
- Render: a single ImageData blit at grid resolution; the 256-entry density ramp is rebuilt from the --fr04-r* tokens on every theme switch, so dark runs brighter-denser and light runs deeper-denser.
- Loop: ~22 Hz sim (STEP=1000/22) with an accumulator; frames are drawn only when a step happened. visibilitychange pauses and resumes the loop; resize re-runs size() debounced 150ms.

ACCESSIBILITY AND MOTION
- With prefers-reduced-motion, the sim runs 40 steps to a settled frame, draws once, and never starts the loop.
- The stage is role="img" with an aria-label describing the automaton; the caption is decorative.

WHAT BREAKS
- Removing toroidal wrapping makes blobs die at the grid edges instead of gliding across them.
- Raising N much past 96 makes the O(N² × KLEN) convolution heavy on low-end CPUs; the fixed 96x96 grid is the performance contract.
- Changing the ramp tokens without rebuilding the ramp (buildRamp on theme switch) leaves stale colors.
- The light theme needs --fr04-blend flipped to multiply; keeping 'screen' on a light ground washes the blobs out.
- Blobs cannot be steered or seeded at runtime; state is local to each mount.
