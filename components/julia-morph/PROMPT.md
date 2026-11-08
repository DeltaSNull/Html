Build a "Julia Morph" ambient background component as a single HTML file: a Julia set fractal whose complex constant traces a slow Lissajous loop, morphing green filaments between connected and dendritic forms over ~70 seconds. Canvas 2D, zero dependencies, one inline script.

PALETTE AND TOKENS
- :root tokens: --fr02-bg:#06070b (near-black ground), --fr02-accent:#378651 (green), --fr02-glow:rgba(55,134,81,.14) / --fr02-vig:rgba(6,7,11,.82) (radial glow and vignette), --fr02-blend:screen, --fr02-frame:rgba(55,134,81,.10) (hairline border), --fr02-cap:#fff / --fr02-cap-o:.42 (caption), --fr02-idim:.5 / --fr02-imix:#000000 (bounded-set interior dim and mix color), --fr02-s0..s6 (escape-time palette ramp: deep green → accent green → sage → gold rim).
- A [data-theme="light"] block repoints the tokens to a paper-green ground (#eef3ef) with the ramp inverted (gold rim becomes the deepest ink) and multiply blending; ?theme=light or a {type:'forever:theme'} postMessage switches at runtime.
- A small uppercase caption ("Julia Morph", 10px monospace, opacity .42) sits bottom-left.

THE CANVAS
- A full-viewport .fr02-stage div (position:fixed, inset:0, overflow:hidden) holds the canvas #fr02-c, a .fr02-glow overlay (two radial gradients, screen blend), a .fr02-frame hairline border, and the caption; the stage carries data-section="stage" and role="img" with an aria-label describing the fractal.
- resize(): DPR-aware sizing (DPR = min(2, devicePixelRatio)), then caps the internal render resolution at 420px on the long edge (cap=420) so per-frame pixel work stays bounded; CSS upscales the canvas.
- buildLUT(): reads --fr02-s0..s6 from CSS and interpolates them into 1024-entry R/G/B lookup tables; also reads --fr02-idim and --fr02-imix for the bounded-set interior mix. Rebuilt on every theme switch.
- render(ccx, ccy): evaluates z <- z^2 + c per pixel (VIEW=3.05 complex height, MAXIT=150, escape radius squared 64) into a Uint32Array pixel buffer — allocation-free in steady state. Escaped pixels get smooth (log-log) coloring from the LUT; bounded pixels are shaded by orbit-trap distance and dimmed toward the interior mix color.
- constantAt(t): a gentle two-frequency Lissajous loop centered near the cardioid cusp (-0.512 + 0.36·cos(a) + 0.045·cos(1.5a), 0.560·sin(a) + 0.045·sin(1.5a)) — the set stays connected but near the boundary, the dendritic regime.
- frame(ts): t = (ts - t0) * 0.00009, so one full morph loop takes ~70 seconds; requestAnimationFrame loop with visibilitychange pause/resume; resize re-renders the current frame.

ACCESSIBILITY AND MOTION
- prefers-reduced-motion: renders a single static frame at constantAt(2.35) and never starts the loop; a @media rule also collapses any CSS animation durations.
- The stage has role="img" and an aria-label; the caption is a plain div.

WHAT BREAKS
- Removing the 420px internal canvas cap makes the per-frame Julia evaluation run at full screen resolution — expensive on slow CPUs (each frame iterates up to MAXIT=150 per pixel).
- Background and glow colors are hardcoded — retheme both the CSS vars and the LUT stops, or the palette and ground disagree.
- The complex-constant path is fixed; there is no interactivity — the user cannot steer or zoom.
