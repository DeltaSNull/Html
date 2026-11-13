Build a "Game of Life" fractals-automata component as a single HTML file: Conway's Game of Life with glowing magenta cells aging through three colour tiers on a dark navy field; the field reseeds automatically when the population collapses. Zero dependencies, one canvas, one IIFE.

PALETTE AND TOKENS
- :root tokens: --gol-bg:#0a0714 / --gol-bg2:#120a20 (dark navy ground, radial gradient), --gol-accent:#ff6ee7 (magenta), --gol-t0:#ff6ee7 / --gol-t1:#c44dff / --gol-t2:#6a8cff (cell age tiers: pink, purple, blue), --gol-dead:#0a0714, --gol-sep:rgba(0,0,0,0.55), --gol-glow:rgba(255,110,231,0.18), --gol-cap:#fff / --gol-cap-o:.42 (caption).
- A [data-theme="light"] block repoints them (--gol-bg:#f4f0fa, --gol-accent:#a21caf, tiers #c026d3 / #7c3aed / #2563eb). A theme-hook script in <head> reads ?theme=light|dark from the URL and stamps data-theme on <html> before first paint, so there is no dark flash. Standalone default stays dark. A {type:'forever:theme'} postMessage switches theme at runtime.

THE SIMULATION
- A full-viewport canvas #gol-c (position:absolute, inset:0, width/height 100%) inside #gol-stage (fixed inset:0, overflow:hidden, radial-gradient ground); a small uppercase monospace caption ("Game of Life", 10px, letter-spacing .08em) sits fixed bottom-left.
- Grid is COLS=180 x ROWS=180, stored in Uint8Array cur/nxt/age buffers. seed() fills cur at 0.30 density with age 0. step() applies 8-neighbour rules with toroidal wrap: a live cell survives on 2–3 neighbours, a dead cell is born on exactly 3; age increments while a cell stays alive and resets on death.
- STEP_MS=110 throttles generations in frame(); render() writes each cell into a low-res 180x180 ImageData (age >= 24 → tier 2, >= 8 → tier 1, else tier 0; dead cells get BG), then stretches it to the full canvas with imageSmoothingEnabled=false for crisp pixels. If the live count drops below 0.018 * grid size, reseed() injects new cells at 0.16 probability per cell.
- syncTheme() reads the --gol-* tokens from computed style into the BG / TIERS / GLOW arrays, so the canvas follows the CSS theme.

SCRIPT (one IIFE, no libraries)
- size() sizes the canvas to the stage; a debounced (120ms) resize listener re-runs it and re-renders under reduced motion.
- start/stop via requestAnimationFrame; visibilitychange pauses when the tab is hidden (skipped under reduced motion). With prefers-reduced-motion, init() steps once, renders once, and never starts the loop.

ACCESSIBILITY AND MOTION
- The field is role="img" with an aria-label describing the evolving cells; the caption is pointer-events:none. Reduced motion shows one static generation.

WHAT BREAKS
- The low-res ImageData stretch is the performance trick — rendering at full resolution instead would be far slower; keep imageSmoothingEnabled=false or the pixels blur.
- The background colour lives in both CSS (--gol-bg) and the BG array (via syncTheme) — update the token, not the array, or the two drift apart.
- Age resets on cell death, so settled patterns rarely reach tier-2 colour; that is by design, not a bug.
- There is no pointer interactivity; the field is fully passive.
