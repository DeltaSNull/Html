Build a "Charged Plasma Lattice" ambient physics component as a single HTML file: gold and teal charged particles attract and repel via Coulomb forces, and teal bonds crystallize between opposite charges as a temperature pulse swings the field between order and chaos. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- :root tokens: --cpl-bg:#17181A (ground), --cpl-fade:rgba(23,24,26,0.30) (trail fade), --cpl-pos:#C0B459 (gold positive charge), --cpl-neg:#1DACC9 (teal negative charge), --cpl-bond:29,172,201 with --cpl-bond-a:0.55 (bond color/alpha), --cpl-core:#ffffff with --cpl-core-a:.9 (particle core), --cpl-blend:lighter.
- Charge colors stay 6-digit hex because the script appends "55" for the halo stop.
- A :root[data-theme="light"] block holds the light palette; ?theme=light in the URL or a {type:'forever:theme'} postMessage switches at runtime, and a theme hook script in <head> stamps data-theme before first paint so there is no dark flash.
- A small uppercase caption ("Charged Plasma Lattice", 9–11px monospace, letter-spacing .14em, opacity .36, pointer-events:none) sits bottom-left.

THE LATTICE
- Full-viewport canvas (#cpl-c). Particle count scales with area: round(min(64, max(34, W*H/5200))), alternating charge, radius 2.4–4.0 × scale (scale = min(W,H)/420).
- Forces: Coulomb f = (q1·q2)·1400/d² (like repels, opposite attracts, softened by +90), a soft pull toward center (×0.0009), thermal jitter scaled by temperature, velocity damped ×0.86 per step. Walls bounce at ×-0.6.
- Bonds: any two opposite charges closer than 66 × scale are joined by a teal line whose alpha fades with distance and with temperature (×(1 − temp×0.6)) — bonds appear as the field cools and melt as it heats.
- Temperature pulses as temp = 0.5 − 0.5·cos(t·0.55), advancing t by 0.012 per frame.
- Pointer move charges the cursor (strength 9000); click flips the pointer charge sign.
- Particles render as a radial glow (charge color → charge color at 55 alpha → transparent halo, radius × 4) over a small solid core.

SCRIPT (one IIFE, no libraries)
- DPR-aware sizing (min(devicePixelRatio, 2)) with ctx.setTransform and a resize listener; particle count is recomputed on resize.
- visibilitychange: stop rAF when hidden, restart when visible.
- Theme sync re-reads the --cpl-* tokens on every theme flip.

ACCESSIBILITY AND MOTION
- A matchMedia('(prefers-reduced-motion: reduce)') branch runs 260 steps at temp 0.04 and draws one static snapshot, never starting the loop.

WHAT BREAKS
- The O(N²) force loop is CPU-bound on low-end devices at N=64; capping the count is the first fix.
- Charge colors must stay 6-digit hex — a 3-digit hex or rgb() value breaks the halo stop concatenation.
- Removing the temperature pulse freezes the lattice in one state; the crystallize/melt rhythm is the whole effect.
- Reduced-motion shows only a static snapshot, no fallback animation.
