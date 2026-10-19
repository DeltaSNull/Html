Build a "Viscosity Swirl" particle component as a single HTML file: a circular "espresso cup" on a deep green board, filled with 65 golden crema particles swirling around the center like thick liquid — move the pointer over it to agitate and stir them. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- :root: --pa-board-deep:#27412F (deep green board), --pa-blush:#E8B4A8 (text/border tint), --pa-butter:#E9C96A (caption, particle gold).
- Particle colors: gold #E9C96A and amber #C78C46 at 0.55 alpha, drawn over a dark rgba(28,18,12,0.2) fillRect each frame for motion trails.
- Fonts: Caprasimo (serif caption) + Work Sans (Google Fonts).

THE CUP
- .pa06-cup-wrap: a 360px circle (max-width 360px, height 360px), background #18281E, 8px blush-tinted border, inset + drop shadows, overflow hidden, crosshair cursor, role="region" aria-label="Espresso Crema Viscosity".
- Canvas fills the cup at 360×360. A Caprasimo caption ("stir the espresso") sits bottom-center, pointer-events none; a small fixed "Viscosity Swirl" caption bottom-left.
- Under 300px containers, a container query shrinks the cup to min(86vmin,260px) with a 5px border.
- Body centers the cup with flexbox; overflow hidden so nothing scrolls.

SCRIPT (one IIFE, no libraries)
- 65 particles spawn at random angle/radius with radius 6–14. Each frame they rotate ~0.012 rad around the cup center; pointer proximity (<80px) pushes particles away with falloff.
- Demo lifecycle: demoEnabled from window.__foreverPreview; attract animation stops on the window 'forever:interactive' event and on pointerdown/click/keydown (capture).
- Frame scheduler: tracks registered drivers and repaint requests; pauses when hidden (visibilitychange), resumes on input; prefers-reduced-motion freezes the field (animations off, one static draw).

ACCESSIBILITY AND MOTION
- role="region" with aria-label; captions are aria-hidden.
- @media (prefers-reduced-motion:reduce): all animations/transitions killed.

WHAT BREAKS
- Removing the overflow:hidden on the cup lets particles and shadows spill.
- Breaking the pointermove/push math kills the signature "stir the espresso" feel.
- The demo lifecycle must set takenOver before stopping demoEnabled, or attract mode keeps repainting forever.
