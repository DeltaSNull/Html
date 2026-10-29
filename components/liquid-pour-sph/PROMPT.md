Build a "Liquid Pour SPH" ambient fluid component as a single HTML file: 200 SPH fluid particles pour into a basin, slosh, and drain on a slow cycle with an additive-blend cyan glow; pointer move tilts the fluid sideways. Zero dependencies, canvas 2D, one inline script. Light-theme ready.

PALETTE AND TOKENS
- :root theme tokens: --lps-bg #071a22, fade rgba(7,26,34,0.30), accent #34d6ff, wall/stream tints, core rgb 210,248,255, and glow stops --lps-g0..g2 (cyan, baked into a sprite). Light variant in [data-theme="light"] (paper #eef5f8 ground, additive blend swapped for source-over).
- A small uppercase caption bottom-left, opacity .34, pointer-events none.

THE FLUID
- Full-viewport canvas. Basin spans W×0.30 to W×0.70, floor at H×0.82, pour origin above at H×0.40.
- SPH: smoothing radius S×0.052, rest density 2.6, stiffness 0.55, near-stiffness 1.6, gravity 0.30, viscosity 0.14. Double-density relaxation (Clavet et al.) over a spatial hash, run as 2 substeps of dt×0.5 per frame.
- Ambient cycle: phase advances slowly; the pour runs while sin(phase) > −0.2, the basin drains while it drops below −0.55 (walls narrow), and an empty basin hurries to refill.
- Rendering: a precomputed radial glow sprite drawn per particle (brightness by speed), plus bright cores; basin walls and a pour-stream hint drawn as strokes.

SCRIPT (one IIFE, no libraries)
- Theme hook: ?theme=light|dark bootstrap plus a forever:theme message listener that re-reads tokens and re-bakes the glow sprite.
- Demo lifecycle (demo-v1): demoEnabled from window.__foreverPreview; attract work stops on 'forever:interactive', pointerdown, click, non-Tab keydown (capture).
- DPR-aware sizing with a resize listener; visibilitychange pause/resume.
- prefers-reduced-motion: settle a resting pool (120 particles, 40 steps) and draw one static frame.

WHAT BREAKS
- The glow sprite is baked once per resize/theme change — editing a glow token without re-running buildGlow keeps the old palette.
- Raising N past ~300 strains the string-keyed spatial hash and the indexOf+splice recycling; switch to integer keys first.
- The double-substep loop is what keeps the fluid stable; collapsing it to one full step makes the pour jitter and explode.
