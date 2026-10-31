Build a "VHS Tracking Distortion" retro-glitch background component as a single HTML file: VHS test-pattern color bars with 46-slice horizontal jitter, random tear events, a rolling tracking bar, and chroma-split REC text on vermillion-tinted black. Canvas 2D, zero dependencies, system monospace fonts.

PALETTE AND TOKENS
- Hardcoded: #1A1614 ground (vermillion-tinted black), bands palette #ff3fb0 / #F4A78F /
  #ffe23f / #7dff7d / #A18B87, chroma-split text #ff2f7f / #C08459 / #fff, caption
  #E04729 at opacity .4, "PLAY SP" white at .55.
- Font: bold ui-monospace/Menlo system stack for the REC text; ui-monospace for captions.

THE DISTORTION
- A full-viewport .stage holds #wrap (fixed, centers the canvas), a fixed .noise overlay
  (repeating-radial-gradient, opacity .07, screen blend), "▶ PLAY  SP" top-left and
  "VHS TRACKING" bottom-right.
- The canvas is sized to 55% of the viewport (pixelated look is intentional).
- Each frame: fill the ground, draw 5 soft color bars (alpha .42, middle half of height).
- Scanline shifting: loop 46 horizontal slices and copy each slice onto the canvas offset by
  jitter (Math.sin(s*0.6+t*4)*2 plus occasional random ±20) and tear (a 3-slice band at
  60% height offset by sin(t*5)*W*0.4). Canvas-to-self drawImage keeps this on the GPU with
  no CPU pixel readback.
- Chroma ghost text: draw "REC ●" three times at cx±3 with pink/tan/white, wobbling with sin(t*3)*4.
- Rolling tracking bar: a white 12%-alpha band at (t*0.13 % 1)*H with a dark edge below.
- t advances by 0.03 per frame (0 when prefers-reduced-motion is set, freezing the frame).

SCRIPT (one IIFE, no libraries)
- visibilitychange pauses and resumes the loop; resize re-sizes the canvas.

ACCESSIBILITY AND MOTION
- prefers-reduced-motion freezes the animation (t stops advancing) — the frame still paints once.

WHAT BREAKS
- The self-drawImage compositing accumulates noise per frame — the per-tick fillRect only
  partially resets it, so jitter bleeds and edges can artifact on some GPU/browser combos.
- Changing the 55% resolution ratio alters the VHS aesthetic.
- Constant jitter and tear make text overlays hard to read — keep foreground copy off this.
- Purely ambient: no click or hover interactivity.
