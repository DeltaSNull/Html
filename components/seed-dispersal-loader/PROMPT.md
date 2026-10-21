Build a "Seed Dispersal Loader" component as a single HTML file: a round tooth-textured plate on pressed-fibre ground where up to twenty-eight seeds lift off a small seed head and drift out of frame on three-hair pappus tails, fading as they reach the rim and respawning at the centre. Canvas-drawn, zero dependencies, one script.

PALETTE AND TOKENS
- :root --sd21-* tokens (dark defaults): --sd21-ground:#152019 (pressed-fibre ground), --sd21-fibre:#202d23, --sd21-ink:#edf0e5, --sd21-lichen:#adb8aa, --sd21-butter:#ddbd61 (seed tone), --sd21-moss:#82a17d, --sd21-lift:#3a4a3d, hairline/rule alphas, --sd21-body:"Noto Sans",ui-sans-serif stack, --sd21-mono.
- :root[data-theme="light"] re-theme: pale recycled fibre (--sd21-ground:#e7e1d3 etc.). ?theme=light|dark query param or a {type:'forever:theme'} postMessage switches at runtime before first paint / live.
- Ground texture: two crossed repeating-linear-gradient hairline layers at 2–3% ink — one paint layer. Plate: 62% fibre mixed into ground with a radial dot tooth texture (5px grid).

THE STAGE
- A fixed full-viewport .sd21-stage (role=status, aria-live=polite) centers a round plate (min(58vmin,240px), 1px hairline border, border-radius:50%) whose canvas fills it; a specimen-tag label below reads "dispersing · N seeds" with a tabular mono count; a small uppercase caption ("Seed Dispersal Loader") sits bottom-left, pointer-events:none.
- 58vmin sizing and overflow:hidden on html/body are what prevent horizontal scroll — keep them.

SCRIPT (one IIFE, no libraries)
- Seeded LCG RNG (seed=11) so the initial scatter is deterministic; spawn(i,seeded) places seeds around the head with angle/radius, velocity v, spin, rotation, drift.
- drawSeed: three pappus hairs (hair lines at ±3.4k) + butter-toned ellipse body; fade alpha out toward the rim (fade by radius).
- frame: dt-clamped loop — head redrawn each frame, seeds drift outward (r += v), angular drift, respawn at rim (r > c-4) with a fresh spawn; one draw per frame, DPR clamped to 2, N capped at MAX=28.
- syncTheme() reads --sd21-ink/--sd21-butter/--sd21-rule from computed CSS so canvas colours follow tokens, re-read on forever:theme. Label count = min(MAX,N).
- visibilitychange pause/stop; resize re-fits and reseeds the scatter.

ACCESSIBILITY AND MOTION
- prefers-reduced-motion: one painted static scatter around the head, no rAF loop.
- The stage is role=status + aria-live polite; the canvas is aria-hidden; the count label is the live text.

WHAT BREAKS
- Removing the syncTheme() token reads (hardcoding colours in the script) breaks the light theme.
- Raising MAX above 28 or dropping the one-draw-per-frame discipline hurts perf on mobile.
- Changing plate size without min(58vmin,240px) risks overflow at 390px width.
- The seeded RNG makes the first frame identical every load — switching to Math.random() loses that determinism.
