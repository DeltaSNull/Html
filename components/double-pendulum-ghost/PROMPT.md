Build a "Double Pendulum Ghost" chaotic physics demo as a single HTML file: a double pendulum swings in emerald on dark green-black, its glowing bob tracing a slowly fading color-shifting ribbon ghost on canvas. Pointer nudges the chaos, click re-kicks it, and stillness triggers an automatic re-kick. Zero dependencies, one small script, canvas 2D only, full light-theme re-theme support.

PALETTE AND TOKENS
- :root tokens (dark defaults): --dpg-bg:#16201A (deep green-black), --dpg-accent:#378651, --dpg-cap-o:.34, --dpg-rod/--dpg-halo/--dpg-bob1/--dpg-bob2/--dpg-core (rod, halo, two bob paints, pivot core), --dpg-blend:lighter (additive ribbon), --dpg-hue:140 (emerald base), --dpg-l0:58/--dpg-lk:1.6 (ribbon lightness + speed ramp), --dpg-ta:0.5 (ribbon alpha).
- A :root[data-theme="light"] block re-points every token (pale green ground #eef4f0, darker greens, ribbon blend source-over, lightness ramp reversed: l0:32, lk:-0.9) — this is the shipped light theme; preserve it.
- syncTheme() reads the tokens into the JS palette object C via getComputedStyle (with a dark fallback map), then clears the offscreen trail canvas so the old palette's ribbon doesn't linger.

THE PENDULUM
- A full-viewport #dpg-c canvas (display:block, width/height 100%, tagged data-section="stage"); html,body overflow hidden. A small ui-monospace caption ("Double Pendulum Ghost") sits bottom-left.
- A head IIFE reads ?theme=light|dark before first paint and stamps data-theme (no dark flash). A forever:theme postMessage listener re-themes live (setAttribute + syncTheme(); in reduced motion it repaints one static frame).
- The physics is an exact double-pendulum Lagrangian integrated with RK4 on the full [a1,a2,v1,v2] state (h=0.04, 8 substeps per frame), with a whisper of damping (0.99992) so it drifts toward rest — then rekicks with random angles when kinetic energy stays below threshold for 120 frames.

SCRIPT (one IIFE, no libraries)
- draw(): run substeps, fade the offscreen trail layer (destination-out, alpha 0.012), paint the new ribbon segment on the trail layer (lineWidth grows with tip speed, hue base 140 ± sin drift, lightness from speed), compose bg + trail + rods + radial-gradient bobs + pivot onto the main canvas.
- Interactions: pointermove nudges angular velocities; click calls rekick(); the demo-v1 lifecycle (demoEnabled from window.__foreverPreview, stopped by first pointerdown/click/keydown(Tab) or forever:interactive) halts attract re-kicks while keeping manual feedback alive.
- visibilitychange pauses/resumes the rAF loop; prefers-reduced-motion paints a single static frame and never starts the loop.

ACCESSIBILITY AND MOTION
- Decorative simulation; no focusable controls; caption is pointer-events none.
- @media (prefers-reduced-motion:reduce): static frame only — one painted frame, no animation, reduced-motion theme flip repaints once.

WHAT BREAKS
- Removing the offscreen trail canvas (tr/tctx) or the destination-out fade pass kills the ghost ribbon — there is no ribbon without the fade layer.
- Changing bg color in CSS without the same change in the JS fill leaves a seam; syncTheme()'s fallback map hardcodes the dark defaults for headless reads.
- Dropping the demo-v1 lifecycle marker (meta name="forever-preview-lifecycle") or the forever:interactive handler breaks preview embedding.
- RK4 with 8 substeps per frame is CPU-intensive on low-end devices; the motion is chaotic and visually intense — not for calm layouts, and the background is hardcoded dark/light per theme, so check both.
