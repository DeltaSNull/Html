Build a "Truchet Tiles" generative-geometry component as a single HTML file: an 8x8 grid of rainbow quarter-arc Truchet tiles that flip orientation in slow sine waves, weaving shifting interlocked curves across a dark violet background. Zero dependencies, one canvas, one tiny script.

PALETTE AND TOKENS
- :root tokens: --gg03-bg:#1a1224 (dark violet ground), --gg03-cap:#ffb3e6 / --gg03-cap-o:.45 (caption), --gg03-arc-s:80 / --gg03-arc-l:65 (arc saturation/lightness fed into the JS hsl()). A [data-theme="light"] block repoints them (--gg03-bg:#f6f1f7, --gg03-cap:#d1008c, --gg03-arc-s:62, --gg03-arc-l:42) and the JS picks LIGHT {s:62,l:42} vs DARK {s:80,l:65} from data-theme.
- A theme-hook script in <head> reads ?theme=light|dark from the URL and stamps data-theme on <html> before first paint, so there is no dark flash. Standalone default stays dark.

THE GRID
- A full-viewport canvas #c (display:block, width/height 100%) holds the whole component; a small monospace caption ("truchet tiles", 11px, letter-spacing .5px) sits fixed bottom-left.
- Grid is N=8; tile = min(W,H)/N, so the grid is always square and centred in the smaller viewport dimension.
- Each tile is drawn by drawArc(x, y, rot): two quarter-circle arcs of radius tile/2 (one from (0,0), one from (tile,tile)), stroke width tile*0.09, in hsl() where hue = ((x+y)*0.7 + sin(t*0.001 + d*3)*40) % 360 — d is the distance from the tile to the pointer, so the hue warms near the cursor. Reduced motion freezes the sine term at 0.
- Each frame, a tile's flip is sin(t*0.7 + r*0.7 + c*0.5) > 0 ? 0 : PI/2 — a slow sine wave sweeping across rows and columns, so tiles flip in travelling waves and the arcs interlock into shifting curves.
- t advances by 0.00012*16.666 per frame.

SCRIPT (one IIFE, no libraries)
- DPR-aware sizing: size() caps DPR at 2, sets canvas width/height, and recomputes tile; a resize listener re-runs it.
- pointermove/pointerleave track the pointer in normalised coordinates (mx, my, -1 when absent) for the hue warmth.
- start/stop via requestAnimationFrame; visibilitychange pauses when the tab is hidden. With prefers-reduced-motion, a single static frame is drawn (t=0, no sine hue) — note the rAF loop still schedules one more frame, so the guard freezes the pattern rather than stopping the loop.

ACCESSIBILITY AND MOTION
- The canvas is pure decoration; the caption is pointer-events:none. Reduced motion collapses the animation to a static pattern.

WHAT BREAKS
- Changing the arc radius or the two arc centres breaks the interlocking curves — the arcs only meet when both are tile/2 from opposite corners.
- Changing N without keeping the grid square (tile = min(W,H)/N) distorts the tiles.
- The background is hardcoded dark violet in CSS; repoint --gg03-bg (and the light block) for other themes.
- Trust the code, not the page's older guidance: the live file uses canvas #c, drawArc, N=8 and tokens --gg03-arc-s / --gg03-arc-l — not #truchet, drawTile, --gg03-arc-1/--gg03-arc-2 or rows/cols. There is no click handler; pointermove only warms the hue.
