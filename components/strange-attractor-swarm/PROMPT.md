Build a "Strange Attractor Swarm" ambient generative component as a single HTML file: 560 hot-pink particles trace the slowly rotating Aizawa strange attractor in 3D, glowing where fast. Zero dependencies, canvas 2D, one inline script. Light-theme ready.

PALETTE AND TOKENS
- :root theme tokens: --sas-bg #120010 plus a speed ramp (--sas-c0 base rgb + --sas-cd delta); light variant in [data-theme="light"] (ramp inverted, paper #f8eef4 ground, additive blend dropped).
- Accent #ff5fa0; a small uppercase caption bottom-left, opacity .36, pointer-events none.

THE SWARM
- Full-viewport canvas. Aizawa attractor params A=0.95, B=0.7, C=0.6, D=3.5, E=0.25, F=0.1, time step DT=0.0085.
- Particles scatter near the basin, integrate the velocity field each frame, respawn when life exceeds 520 or they leave bounds. Projected with rotation about the vertical axis, slight tilt, and perspective (1.7/(1.7+tz+1.3)).
- Color: speed maps along the token ramp from deep magenta dust to hot pink to near-white crest; alpha and radius grow with depth.
- Pointer move spins the view (pull) and boosts simulation speed (2 extra integration steps while boosted); click boosts harder.

SCRIPT (one IIFE, no libraries)
- Theme hook: ?theme=light|dark bootstrap plus a forever:theme message listener that re-reads tokens (and repaints the static frame when reduced-motion).
- Demo lifecycle (demo-v1): demoEnabled from window.__foreverPreview; attract work stops on 'forever:interactive', pointerdown, click, non-Tab keydown (capture).
- DPR-aware sizing with a resize listener; visibilitychange stop/start.
- prefers-reduced-motion: settle the swarm onto the attractor (700 warm-up steps), draw one static frame — with 900 particles, deliberately denser than the animated 560.

WHAT BREAKS
- The color ramp reads tokens once per theme flip, never per frame — editing a token value without re-running syncTheme leaves the old palette.
- Lowering N helps low-end devices, but the reduced-motion path's 900 is intentional; "fixing" it to match 560 thins the static frame.
- The rotation expression rot+=(demoEnabled?.0016:0) is a ternary, not optional chaining — don't "fix" it into demoEnabled?.0016.
