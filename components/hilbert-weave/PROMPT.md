Build a "Hilbert Weave" ambient background component as a single HTML file: a Hilbert space-filling curve draws itself as a glowing teal-to-violet ribbon with a leading spark, cycling through orders 2–5 and looping continuously. Canvas 2D, zero dependencies, one inline script.

PALETTE AND TOKENS
- :root tokens: --hw-bg:#021016 (dark navy ground), --hw-cap:#23c2c9 (caption), --hw-cap-o:.34, --hw-ghost:rgba(35,194,201,0.07) (faint full-curve ghost stroke), --hw-glow-s:85 / --hw-glow-l:60 / --hw-glow-a:0.22 (glow underlay hsl), --hw-core-s:70 / --hw-core-l:58 / --hw-core-a:0.95 (crisp core ribbon hsl), --hw-spark0/1/2 (head-spark radial-gradient stops).
- A [data-theme="light"] block repoints the tokens to a pale aqua ground (#eef6f7) with a dark ribbon and soft coloured glow shadow; ?theme=light or a {type:'forever:theme'} postMessage switches at runtime.
- A small uppercase caption ("Hilbert Weave", 10px monospace, letter-spacing .16em, opacity .34) sits bottom-left.

THE CANVAS
- A full-viewport canvas #hw-c (display:block, width/height 100%) is the whole page; it carries data-section="stage".
- size() sets DPR-aware canvas dimensions (DPR = min(2, devicePixelRatio)) and re-runs on resize; the drawing square S = min(W,H)·0.86 is centred with offsets ox, oy.
- d2xy(n, d) maps distance d along the Hilbert curve of order n to grid coords (bitwise rotation/reflection loop); buildPath(order) builds a Float32Array polyline of cell-centre points for n = 2^order cells per side.
- render(prog): fills the background, strokes the faint full-curve ghost (lineWidth cell·0.10) so the completed lattice reads as woven cloth, then draws the active ribbon in two passes — a wide translucent glow underlay (lineWidth lw·2.4, globalAlpha --hw-glow-a) and a crisp core ribbon (lineWidth lw = max(1.4, cell·0.30)) — both with hue = (168 + f·150) % 360 flowing along arc length (teal → violet → warm), plus a radial-gradient head spark at the leading point.
- frame(): a draw/hold phase machine — draw advances by SPEED=0.018·(0.5 + 0.5·(1−draw)) per frame, then holds 90 frames, then advances order (2→3→4→5→2) and rebuilds the path. hueShift drifts 0.0009 per frame.
- Pointer move sets nudge=1, click sets nudge=1.6 (decaying ·0.92 per frame) to speed the draw and hue drift.
- requestAnimationFrame loop with visibilitychange pause/resume; theme tokens are re-read from CSS on a theme flip.

ACCESSIBILITY AND MOTION
- prefers-reduced-motion: renders the complete order-4 curve once (render(1)) and never starts the loop.
- The canvas is pure decoration; the caption is a plain div.

WHAT BREAKS
- Background color is hardcoded in CSS (#021016) — edit both CSS and ctx.fillStyle to change it.
- The per-segment draw loop scales as O(n²) cells — order 5 draws 1023 segments twice per frame (glow + core), may drop frames on slow hardware; don't raise the order range without a performance budget.
- Replacing the single wide glow pass with per-segment shadowBlur repaints the whole element every frame and costs cycles.
