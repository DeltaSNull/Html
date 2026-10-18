Build a "Spiral Galaxy Rotate" ambient space component as a single HTML file: 115 stars forming a two-armed spiral galaxy rotating around a glowing violet core on near-black. Pointer move briefly boosts the spin. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- Hardcoded dark ground #06040f — in both the CSS and the canvas fill; change both to retheme.
- Star palette by radius: inner #fff3d0, mid #D0C3DF, outer #ff9ad0 (30%) / #D8C197; violet core glow (radial gradient rgba(255,240,210,.9) → transparent).
- A small uppercase caption bottom-left, opacity .34, pointer-events none.

THE GALAXY
- Full-viewport canvas #c. 115 stars: arm = i % 2, radius = pow(random, .6) * 190, base angle = arm * PI + r * .022, jitter ±.25, size .6–2.4.
- Each frame: trail fill rgba(6,4,15,.3); translate to center, squash y by .62, rotate by t; draw the core glow (55*scale radial gradient); each star at angle base + jit + t * .15 * (60/max(r,20)).
- Pointermove sets spin=2.4, decaying toward 1 (spin += (1-spin)*.04); t += .0055 * spin.

SCRIPT (one IIFE, no libraries)
- DPR-aware sizing (min(devicePixelRatio, 2)) with a resize listener.
- visibilitychange: stop rAF when hidden, restart when visible.
- prefers-reduced-motion: render one static frame, never start the loop.

WHAT BREAKS
- Forgetting the canvas fill when retheming leaves a #06040f trail smear behind the stars.
- Removing the y-squash (.62) makes the galaxy circular instead of tilted-elliptical.
- The reduced-motion path must still paint one frame or the canvas stays blank.
