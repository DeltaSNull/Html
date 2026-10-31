Build a "Digital Rain" Matrix-style background component as a single HTML file: cyan katakana and hex glyphs fall in columns down a dark canvas, leaving fading trails, with occasional bright white leading characters. Canvas 2D, zero dependencies, Google Fonts for the caption.

PALETTE AND TOKENS
- :root tokens: --bg-900:#08090d (near-black ground), --text-2:#8a91a8 (caption),
  --rain-trail:rgba(8,9,13,0.18) (per-frame wash that makes the fading trails),
  --rain-glyph:#22d3ee (column color), --rain-lead:#aef6ff (bright lead chars),
  --rain-still-bg:#0d0f16 / --rain-still-ink:rgba(34,211,238,0.55) (reduced-motion static frame).
- Every color the canvas paints is read from the CSS tokens once at start and once per theme
  switch — never per frame. A :root[data-theme="light"] block carries the light re-theme.
- --font-display:"Space Grotesk",system-ui,sans-serif for the glyphs and caption.

THE RAIN
- A full-viewport .stage (overflow:hidden) holds .c06-rain (aria-hidden="true") with a canvas
  and a small uppercase caption bottom-left.
- setupCanvas: DPR-aware sizing (capped at 2) via ResizeObserver; on resize rebuild the column list.
- Columns = ceil(width / fontSize), fontSize = max(12, round(width/26)); each column starts at a
  random drop height.
- render runs every other frame (frame%2): wash the canvas with the trail color at low alpha,
  then for each column draw one random glyph from the GLYPHS array (katakana + hex + symbols),
  bright lead char when Math.random()>0.92, reset the column to 0 when past the bottom with
  probability 0.975.

SCRIPT (one IIFE, no libraries)
- runLoop: requestAnimationFrame loop with a visibilitychange pause when the tab is hidden.
- Theme hook: ?theme= bootstrap before first paint; forever:theme message listener re-syncs tokens.

ACCESSIBILITY AND MOTION
- prefers-reduced-motion: render a single static frame (two columns of dim glyphs) instead.

WHAT BREAKS
- Background color is hardcoded in CSS and the canvas fill — change both to leave near-black.
- Renders every other frame: can feel slightly choppy at low refresh rates.
- Columns stall near the bottom before resetting (the 0.975 reset probability is intentional).
