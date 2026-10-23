Build a "Hyperspace Warp" ambient space component as a single HTML file: 110 star streaks radiate outward from a glowing center in a hyperspace tunnel; pressing and holding accelerates warp speed 2.6×. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- Hardcoded dark navy ground #02030a (CSS); trail fill rgba(2,3,10,0.34).
- Star palette: #9fdcff (25%), #ffd1f0, #ffffff; center glow rgba(180,210,255).
- A small uppercase caption bottom-left, opacity .32, pointer-events none.

THE WARP
- Full-viewport canvas, focusable (role="button", aria-label "Hold to accelerate warp travel"). 110 stars, each with angle, depth z, speed, color.
- Each frame: trail fill; stars advance z += sp * speed; streak drawn from z to z + sp*speed*12, line width and alpha grow with z; stars past z=1 respawn at center.
- Press and hold (pointer, or Space/Enter while focused): target speed 2.6; release: target 0; speed eases toward target (×0.06). Center glow radial gradient radius 40.

SCRIPT (one IIFE, no libraries)
- Demo lifecycle (demo-v1): demoEnabled from window.__foreverPreview; attract work stops on 'forever:interactive', pointerdown, click, non-Tab keydown (capture).
- DPR-aware sizing with a resize listener; visibilitychange stop/start.
- prefers-reduced-motion: one frozen frame — z never increments.

WHAT BREAKS
- The trail fill alpha is the motion-blur: raising it toward 1 erases the streaks, lowering it smears the whole field.
- Removing the demo lifecycle stop makes the attract loop fight the user's hold-to-accelerate input.
- Background lives in CSS (#02030a) only — the canvas never paints an opaque ground, so a CSS change alone rethemes it, but a mismatched trail fill color will ghost.
