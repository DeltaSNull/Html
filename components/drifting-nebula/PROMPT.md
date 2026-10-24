Build a "Drifting Nebula" ambient space component as a single HTML file: 26 soft radial blobs in pink, violet, and blue breathe and drift with additive blending over 70 twinkling stars; the cursor gently pulls the nebula clouds. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- Hardcoded dark purple ground #0a0418 — in both the CSS and the canvas fill; change both to retheme.
- Blob palette (cols array, RGB triplets): pink 255,90,170 / violet 150,90,255 / blue 90,160,255 / peach 255,140,90. Starlight #fff; caption #ffd6f4 at opacity .34.
- A small uppercase caption ("Drifting Nebula", 10px system-ui, letter-spacing .08em, pointer-events:none) sits bottom-left.

THE NEBULA
- Full-viewport canvas (#c). 26 blobs (N=26), each with a random position, radius 0.16–0.42 of the smaller viewport dimension, a palette color, a slow drift (dx/dy up to ±0.0004), a phase, and a breathe speed 0.3–0.9.
- Each blob draws as a radial gradient (color at 0.22 alpha → 0.08 at half radius → transparent) with globalCompositeOperation 'lighter', so overlapping clouds add up to brighter nebula cores.
- Breathing: radius scales by 0.85 + 0.15·sin(t·sp + ph) each frame.
- Blobs bounce off a margin of −0.2 to 1.2 in normalized coordinates.
- 70 stars (radius up to 1.2px) twinkle via alpha 0.4 + 0.5·|sin(t·3 + ph)|.
- Pointer move shifts the whole field: each blob's draw position is pulled by 0.02 × (pointer − 0.5) in each axis.

SCRIPT (one IIFE, no libraries)
- DPR-aware sizing (min(devicePixelRatio, 2)) with ctx.setTransform and a resize listener.
- visibilitychange: stop rAF when hidden, restart when visible.
- Frame time t advances 0.01 per frame; under reduced-motion t freezes, so blobs stop moving.

ACCESSIBILITY AND MOTION
- A matchMedia('(prefers-reduced-motion: reduce)') branch draws exactly one frame and never starts the loop; stars still twinkle in that single frame because t advances once.

WHAT BREAKS
- Changing the ground color in only the CSS or only the canvas fill leaves a flash of the other color every frame — change both.
- Removing 'lighter' blending makes overlapping blobs occlude instead of glowing.
- Raising the blob count or radius too far turns the field into an opaque wash; the low alphas are what keep it nebulous.
