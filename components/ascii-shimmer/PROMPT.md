Build an "ASCII Shimmer" retro-glitch component as a single HTML file: a grid of ASCII ramp characters shimmering through interfering radial and plasma waves in green-teal hues; character density and the wave centre follow the pointer. Zero dependencies, one canvas, one scoped script block.

PALETTE AND TOKENS
- :root tokens: --fc-bg:#01120b (dark green-black ground), --fc-text:#eafff3, --fc-accent:#46f49e (green-teal accent), --fc-cap:#fff / --fc-cap-o:.42 (caption).
- The canvas background is a vertical gradient from #01120b to #00150c, drawn each frame; the wrapper #fc-wrap is fixed inset:0 with overflow:hidden.

THE FIELD
- A full-viewport canvas #fc-canvas (position:absolute, inset:0, width/height 100%) holds the whole component; a small uppercase monospace caption ("ASCII shimmer", 10px, letter-spacing .08em) sits fixed bottom-left.
- resize() sizes the canvas to its client size and sets cell = max(8, min(30, min(w/46, h/30))) — the cell cap keeps characters legible and bounds the grid; cols = ceil(w/cell), rows = ceil(h/cell).
- draw() fills the gradient, then for each cell computes v = sin(dist*18 - t*0.6)*0.5 + sin((x/cols)*9 + t*0.35)*0.3 + sin((y/rows)*11 - t*0.4)*0.2, where dist is the distance from the cell centre to the pointer (px, py, normalised, default 0.5/0.4). a = (v+1)/2 picks the ramp character chars[idx] from " .:-=+*#%@" and the colour hsl(140 + v*60, 70%, 30 + a*45%).
- Characters are drawn centred in each cell with a ui-monospace font at cell px.

SCRIPT (one scoped const/let block, no IIFE, no libraries)
- t advances by 0.035 per frame in loop(); pointermove updates px/py from clientX/innerWidth and clientY/innerHeight.
- start/stop via requestAnimationFrame; visibilitychange pauses (cancelAnimationFrame) when the tab is hidden and restarts on return. With prefers-reduced-motion, a single static frame is drawn (t frozen at 0).

ACCESSIBILITY AND MOTION
- The field is role="img" with an aria-label describing the shimmer; the caption is pointer-events:none. Reduced motion freezes the wave (t never advances) and draws one static frame.

WHAT BREAKS
- Changing the cell cap divisors (46 / 30) without the max(8, ...) floor makes characters unreadably small or the grid too coarse.
- Removing the pointermove handler leaves the wave centre stuck at the default 0.5/0.4 — the field still shimmers, but never responds.
- The background is hardcoded dark green-black; there is no light theme, and the shimmer competes with legible foreground text.
- No DPR scaling — the canvas renders at CSS pixel resolution, so it looks soft on high-DPI displays.
