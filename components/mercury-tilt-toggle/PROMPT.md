Build a "Mercury Tilt Toggle" component as a single HTML file: a glass ampoule tilt switch where a mercury bead rolls under simulated gravity, squashes on impact, and bridges the contact pins — flipping the readout from OPEN to BRIDGED. Zero dependencies, one rAF physics loop, system mono font.

PALETTE AND TOKENS
- --mtt-* tokens on :root: --mtt-bg:#0A0A0A, --mtt-surface:#161616, --mtt-text:#EDEDEA / --mtt-muted:#8A8A85 / --mtt-faint:#5A5A56, --mtt-border (14% white), --mtt-accent:#C7F000 lime / --mtt-line / --mtt-soft, --mtt-mono (ui-monospace stack).
- Light re-theme in :root[data-theme="light"]; ?theme=light|dark before first paint, plus forever:theme postMessage at runtime.

THE SWITCH
- .mtt-root centres the .mtt-amp ampoule (18.5em × 5.4em pill, tilted rotate(-10deg) at rest) holding the .mtt-hg mercury bead and a pair of contact .mtt-pins on the right; below, .mtt-info shows "CIRCUIT // [OPEN|BRIDGED]".
- Click / Space / Enter flips the ampoule to rotate(10deg); ArrowRight/ArrowLeft force the tilt direction.
- Physics: gravity .68 per frame (dt-corrected), .92 damping, bounce .32 at each end of the 152-unit reference track; the bead squashes on impact (scale X up to 1.28 while |velocity| is high); when pos passes MAX-14 the pins light lime and the readout bridges.
- When the loop settles, rAF stops — nothing spins when idle.

SCRIPT (one IIFE, no libraries)
- Physics runs in a 152-unit reference track; k maps it onto the rendered track via ResizeObserver on the ampoule, so resizing stays proportional.
- Attract mode tips the ampoule every 2.8 s until the first deliberate input (pointerdown/keydown/forever:interactive stops it permanently).
- visibilitychange cancels the rAF loop and demo timer, and restarts them on return.

ACCESSIBILITY AND MOTION
- The ampoule is role="switch" with aria-checked, focus-visible lime outline, keyboard control.
- @media (prefers-reduced-motion:reduce): the bead teleports to the target end with no squash; the ampoule rotation transition is removed; no attract mode.

WHAT BREAKS
- Hardcoding pixel distances instead of the 152-unit track breaks the bead's end-stops on resize.
- Removing the settled check keeps rAF spinning forever (battery drain).
- State is local to each mounted instance.
