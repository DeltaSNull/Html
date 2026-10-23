Build a "Gravitational Lens Ring" ambient space component as a single HTML file: background starlight bends tangentially around a dark foreground mass, and as alignment drifts the lensed galaxy arcs converge into a glowing magenta Einstein ring. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- Hardcoded dark purple ground #0d0814 — in both the CSS and the canvas fill; change both to retheme.
- Magenta ring glow rgba(255,158,210); galaxy starlight tints (magenta → warm → violet by hue parameter); bent starfield #cfe0ff with #aebbe6 streaks.
- A small uppercase caption bottom-left, opacity .34, pointer-events none.

THE LENS
- Full-viewport canvas. Einstein radius R = min(W,H) * 0.28.
- Bent starfield: 150 stars; each star's light is deflected by (R²/dist) * 0.6 and smeared tangentially into a short arc, twinkling over time.
- Source galaxy: 70 points. Each is imaged twice by point-lens math — theta = (beta ± sqrt(beta²+4))/2 — drawn as tangentially stretched arcs whose spread grows as the source nears the optical axis.
- Alignment beta oscillates (0.62 + 0.6 * cos(t * 0.011)); near beta ≈ 0 a radial-gradient Einstein ring glow fades in. A dark radial mass occludes the center, with a faint lensing-galaxy core.
- Pointer move nudges the alignment offset (×1.6), decaying ×0.95 per frame.

SCRIPT (one IIFE, no libraries)
- DPR-aware sizing (min(devicePixelRatio, 2)) with a resize listener.
- visibilitychange: stop rAF when hidden, restart when visible.
- prefers-reduced-motion: draw one static frame (fixed drift 0.18), never start the loop.

WHAT BREAKS
- Changing only the CSS background leaves the canvas fill #0d0814 — the field still paints dark purple.
- Dropping the tangential smear turns the bent starfield into ordinary dots and the effect reads as noise.
- The pointer nudge decay is frame-rate dependent; converting to dt-based decay changes the feel.
