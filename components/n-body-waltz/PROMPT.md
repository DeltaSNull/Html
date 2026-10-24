Build an "N-Body Waltz" ambient space component as a single HTML file: five glowing gravitational bodies orbit and sling each other in deep space, trailing colored comet tails; the pointer perturbs the dance and a click reseeds it. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- :root tokens: --nbw-bg:#211A17 (ground), --nbw-fade:rgba(33,26,23,0.30) (trail fade), --nbw-accent:#D345A4 (caption), --nbw-p1..p5 (core,glow palette pairs: pink/magenta, peach/orange, mint/teal, cream/gold, rose/red), --nbw-blend:lighter, --nbw-ta:0.5 (trail alpha), --nbw-ga:0.85 (glow alpha).
- A :root[data-theme="light"] block holds the light palette; ?theme=light in the URL or a {type:'forever:theme'} postMessage switches at runtime, and a theme hook script in <head> stamps data-theme before first paint so there is no dark flash.
- A small uppercase caption ("N-Body Waltz", 10px monospace, letter-spacing .16em, opacity .34, pointer-events:none) sits bottom-left.

THE BODIES
- Full-viewport canvas (#nbw-c). Five bodies (N=5) seeded on a ring (radius 70–110, mass 10–32, radius from mass^0.42), with net momentum and center of mass zeroed.
- Physics: O(N²) gravity with G=1400 and softening SOFT=900, 3 substeps of dt=0.55 per frame. Trails hold TLEN=46 positions per body, drawn as fading segments (alpha t² × --nbw-ta, line width 0.4 → 2.6).
- Soft recentering pulls the center of mass back 4% per frame; a gentle leash reins in any body slung past 45% of the smaller viewport dimension — this is what keeps the dance on-frame.
- Bodies render as a radial glow (core color → glow color → transparent halo, radius × 5.5) over a solid core dot.
- Pointer move repels bodies (f=2600/d2, softened by +400); click reseeds the whole system.

SCRIPT (one IIFE, no libraries)
- DPR-aware sizing (min(devicePixelRatio, 2)) with ctx.setTransform and a resize listener; canvas scale S = min(W,H)/520.
- visibilitychange: stop rAF when hidden, restart when visible.
- Theme sync re-reads the --nbw-* tokens on every theme flip; bodies keep their PAL references.

ACCESSIBILITY AND MOTION
- @media is not used; instead a matchMedia('(prefers-reduced-motion: reduce)') branch draws one static frame after 140 steps and never starts the loop.

WHAT BREAKS
- Raising N much past 5 makes the O(N²) loop with 3 substeps heavy on low-end CPUs.
- Removing the soft recentering or the leash lets bodies drift off-frame and never return.
- The light theme needs the --nbw-blend token flipped to source-over; keeping 'lighter' on a light ground washes the bodies out.
- State is local to each mounted instance; a theme/format remount preserves takeover, not component values.
