Build a "Rotozoom Texture" demoscene background component as a single HTML file: a chunky-pixel violet-and-cyan checker texture rotates and breathes in a continuous rotozoom; the cursor drifts the plane and a click boosts the zoom. Zero dependencies, one canvas, one small script.

PALETTE AND TOKENS
- No :root tokens — background hardcoded deep violet (#1b0033) in CSS. Palette
  lives in four byte arrays in the script: A [0x1b,0x00,0x33] (deep violet),
  B [0x30,0x00,0x66] (violet), C [0x00,0xf0,0xff] (cyan), D [0x8b,0x00,0xff]
  (bright violet). A 2x2 checker table ch=[[0,2],[2,0],[1,3],[3,1]] picks
  which palette entry each checker cell uses.
- Caption: 10px uppercase monospace, letter-spacing .14em, #f7e8ff at opacity
  .34, fixed bottom-left, pointer-events:none.

THE TEXTURE
- A full-viewport canvas (#rz-c) with image-rendering:pixelated is the whole
  page; html,body overflow:hidden.
- size() reads the canvas bounding rect, then sets the chunk size P =
  Math.max(3, Math.round(Math.min(W,H)/80)) and the backing canvas to
  ceil(W/P) x ceil(H/P) — a low-res buffer upscaled by the browser, which is
  what makes the pixels chunky.
- frame(): t advances 0.016 per frame (starting at t=120, so the reduced-motion
  static frame is mid-animation); boost decays *0.94. Scale s =
  1.05 + 0.45*sin(t*0.013) + boost*0.6 (the breathing zoom); rotation angle a =
  t*0.011. For every pixel, ch2(x,y,1) gives the base checker index, the pixel
  is rotated by (ca, sa) and divided by s, offset by pointer drift
  ((px-0.5)*40, (py-0.5)*40), then quantised to 16px cells (tx, ty) to pick
  between palette pairs (C/A or D/B). Pixels are written with putImageData.
- Pointer move updates px/py (normalised cursor position); click sets boost=1.

SCRIPT (one IIFE, no libraries)
- start()/stop() wrap requestAnimationFrame; visibilitychange pauses when the
  tab is hidden and restarts when shown.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce) is read once into `reduce`: a single
  static frame at t=120 is drawn and no animation loop runs.
- The canvas is pure decoration with no ARIA role; the constant motion is
  intense — do not use it behind legible text.

WHAT BREAKS
- Removing image-rendering:pixelated (or raising the backing resolution)
  destroys the chunky-pixel look.
- The per-pixel loop is CPU-bound: on low-end or battery-constrained devices
  it is heavy, and shrinking P makes it heavier.
- Changing the background requires editing the CSS (#1b0033) — the canvas
  covers the whole viewport, so the CSS colour only shows at the edges.
- The checker quantisation (/16 cells) and the ch2() bit tricks are coupled:
  changing one without the other breaks the pattern.
