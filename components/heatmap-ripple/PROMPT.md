Build a "Heatmap Ripple" micro-dataviz component as a single HTML file: a 12x6 grid of rounded cells coloured via a magma ramp, with a sine-wave ripple emanating from a point that orbits and follows the cursor. Zero dependencies, one canvas, one IIFE.

PALETTE AND TOKENS
- :root tokens: --c07-bg:#120a18 (dark plum ground), --c07-cap:#ff7ac6 / --c07-cap-o:.45 (caption), --c07-ramp (5 magma stops as "r g b" triples, cold -> hot: 20 12 40, 120 30 110, 230 60 110, 255 150 80, 255 230 150).
- A [data-theme="light"] block repoints them (--c07-bg:#f6f1f7, --c07-cap:#d6007a, ramp repointed to pale pinks). A theme-hook script in <head> reads ?theme=light|dark from the URL and stamps data-theme on <html> before first paint, so there is no dark flash. Standalone default stays dark. A {type:'forever:theme'} postMessage switches theme at runtime.

THE GRID
- A full-viewport canvas #c07-canvas (display:block, width/height 100%). A small uppercase monospace caption ("heatmap", 10px, letter-spacing .08em) sits fixed bottom-right.
- COLS=12, ROWS=6. Each frame, the ripple source sits at (cx, cy) plus an orbit offset (0.32*cos(phase*0.7), 0.3*sin(phase*0.9)); phase advances 0.05 per frame. For each cell, v = 0.5 + 0.5*sin(phase*1.6 - d*9), attenuated by max(0, 1 - d*0.9), where d is the distance from the cell centre to the source.
- ramp(v) interpolates the --c07-ramp stops (read from computed style by syncTheme(), so both themes drive the same code). Cells are drawn as rounded rects (rr helper, radius min(4, cell/4)) with a 7% side pad and a gap of 12% of the smaller cell dimension.
- pointermove on the canvas moves the ripple source (cx, cy) to the cursor; under reduced motion (or after takeover) it repaints once instead of animating.

SCRIPT (one IIFE, no libraries)
- resize() sizes the canvas with DPR capped at 2 and repaints when ready. draw(t) clears, advances phase, and paints all 72 cells each frame.
- start/stop via requestAnimationFrame; visibilitychange cancels the frame and re-starts (or paints once) when the tab returns. With prefers-reduced-motion, init() draws one static frame and never starts the loop (phase frozen, orbit zeroed).
- Demo lifecycle: window.__foreverPreview gates the loop (demoEnabled), and a forever:interactive event, pointerdown, click, or keydown takes over (stops the attract loop, paints the current frame). The first deliberate input freezes the procedural drift after its input paint; hover alone keeps the idle demonstration running.

ACCESSIBILITY AND MOTION
- The canvas has no text alternative; it is pure decoration. Reduced motion shows one static ripple frame with no orbit or phase advance.

WHAT BREAKS
- The ripple is purely decorative — values are procedural, not driven by real data; do not present it as a real heatmap or bind real datasets to it without replacing the value function.
- Changing COLS/ROWS without the pad/gap math leaves cells stretched or overlapping; the grid is sized from W and H each frame.
- The ramp stops live in --c07-ramp as space-separated triples; syncTheme() parses them, so edit the token, not just the stops array defaults.
- The background colour lives in --c07-bg; update the token (and the light block) to retheme.
