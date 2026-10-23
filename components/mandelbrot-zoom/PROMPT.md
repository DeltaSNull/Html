Build a "Mandelbrot Zoom" ambient fractal component as a single HTML file: an escape-time Mandelbrot set slowly zooms into a filigree boundary seam, in smooth red-to-gold bands on near-black. Zero dependencies, canvas 2D, one inline script. Light-theme ready.

PALETTE AND TOKENS
- :root theme tokens: --fr01-bg #06070b plus a 7-stop palette ramp --fr01-s0..s6 (near-black → deep red → accent #C43131 → tan → gold).
- Light theme (:root[data-theme="light"]): paper #f5f1ed ground, the same hues with the ramp inverted (paper → deep ochre ink).
- Static glow + vignette layers painted once over the canvas; a small uppercase caption bottom-left.

THE FRACTAL
- Zoom target: a filigree seam at TX=-0.74364388703, TY=0.13182590421. View height span starts at 3.0, decays ×0.9885 per step, snaps back at 0.0009.
- Rendering: fixed low-res offscreen buffer under a hard pixel budget (MAXPIX=96000 — W×H ≤ 96000 at the stage's aspect), iteration cap MAXIT=220, escape radius 256 for smooth (continuous) iteration counts, colored through a 2048-entry LUT interpolated from the CSS token stops. The browser scales the buffer up via CSS.
- Recompute throttled to ~30fps; the rAF loop writes into a cached Uint32 view of the ImageData — no per-frame allocation.

SCRIPT (one IIFE, no libraries)
- Theme hook: ?theme=light|dark bootstrap plus a forever:theme message listener that rebuilds the LUT from the new tokens and repaints.
- visibilitychange: pause when hidden, resume when visible.
- prefers-reduced-motion: render one static, deep frame (span = BASE × 0.13) and stop.

WHAT BREAKS
- Raising MAXPIX or MAXIT blows the CPU budget — the pixel loop stutters on low-end devices despite throttling.
- The interior color is derived from --fr01-bg; hardcoding it desyncs the set interior from the page ground on theme switch.
- Forgetting to rebuild the LUT on theme change leaves dark colors on the light theme.
