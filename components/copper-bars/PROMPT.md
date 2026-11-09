Build a "Copper Bars" retro/demoscene background component as a single HTML file: six glossy multicolor raster bars bob sinusoidally over a twinkling starfield, with a specular sheen and a scanline overlay. Zero dependencies, one canvas, one small script.

PALETTE AND TOKENS
- No :root tokens — background hardcoded dark navy (#000016) in CSS and in the
  frame fill. Bar hues live in the BARS array in the script, one entry per bar:
  hue (0–360), sat (%), off (phase offset), amp (travel amplitude, 0.20–0.34),
  spd (animation speed, 0.42–0.63). The six bars: amber 28, cyan 200, magenta
  300, green 120, gold 48, red-pink 340.
- Caption: 10px uppercase monospace, letter-spacing .14em, #ff7b00 at opacity
  .34, fixed bottom-left, pointer-events:none.

THE BARS
- A full-viewport canvas (#cb-c) is the whole page; html,body overflow:hidden.
- size() is DPR-aware (DPR capped at 2): sets canvas width/height to W*DPR /
  H*DPR and ctx.setTransform(DPR,0,0,DPR,0,0). Re-run on resize. Stars are
  regenerated on resize: count Math.round(W*H/2600), each with x/y/z/tw.
- band(cx, cy, bh, b) draws one bar: a vertical linear gradient from dark edges
  (4% lightness) through a saturated mid (40%) to a bright specular core (60%
  at the centre) and back down, then two white highlight lines near the top
  (0.85 alpha, then 0.30 alpha) for the hardware-raster sheen.
- frame(): t advances 0.016 per frame; nudge decays *0.93. Starfield first:
  fill #000016, each star drawn as a 0–1.5px square with alpha
  (0.25+z*0.55)*twinkle, orange (#ff7b00) for the brightest stars, pale blue
  (#cfe2ff) otherwise. Then the bars: height bh = Math.min(H*0.16, 76),
  travel span = H*0.30; each bar's centre cy = H*0.5 + sin(t*spd*(1+nudge*0.6)
  + off) * span * amp, stacked with (i - 2.5) * bh * 0.62 offsets. Then the
  scanline overlay: globalAlpha 0.06, 1px #000016 lines every 3px.
- Pointer move sets nudge=1, click sets nudge=1.6 — the bars surge briefly.

SCRIPT (one IIFE, no libraries)
- start()/stop() wrap requestAnimationFrame; visibilitychange pauses when the
  tab is hidden and restarts when shown.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce) is read once into `reduce`: a static
  single frame is drawn (twinkle fixed at 0.6) and no animation loop runs.
- The canvas is pure decoration; the caption is aria-hidden by being
  pointer-events:none and non-interactive.

WHAT BREAKS
- Removing the DPR setTransform makes the bars blurry on retina screens.
- Changing the background requires editing BOTH the CSS (#000016) and the
  frame fill — otherwise the canvas paints over the CSS colour every frame.
- Bars and scanlines obscure overlaid text — never put legible copy on top.
- The scanline loop redraws every frame; on very tall viewports it costs
  extra paint time.
