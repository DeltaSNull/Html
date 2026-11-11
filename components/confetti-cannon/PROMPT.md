Build a "Confetti Cannon" celebration component as a single HTML file: broad confetti rectangles in an ochre festival-paper palette fire from the bottom corners, tumble and flip as they fall; a click fires a burst at the cursor. Theme-aware (dark default, light via ?theme=light), one canvas, one script.

PALETTE AND TOKENS
- :root tokens: --c03-bg:#241D14 (ground), --c03-cap:#B29234 / --c03-cap-o:.5
  (caption), --c03-pal:#B29234,#ffd166,#06d6a0,#A19A87,#C08459,#ff9f1c (comma
  list feeding the JS palette). The :root[data-theme="light"] block re-points
  every token (--c03-bg:#f6f1e8, --c03-cap:#836b26, --c03-cap-o:.6, --c03-pal
  the darker light-variant palette).
- Caption: 10px ui-monospace, uppercase, letter-spacing .08em, fixed
  bottom-left, opacity .5.

THE CANNON
- A full-viewport canvas (#c03-canvas, position:fixed inset:0, tagged
  data-section="stage"); html,body overflow:hidden.
- fire(ox, oy, dir): launches 18 + random*10 bits from a point, each with an
  angle around straight up (±0.5 rad plus the direction offset), speed 5–10,
  width 6–10px, height 4–7px, random rotation and spin velocity (±0.2),
  palette index, life 1, and a flip phase sw 0–6. The array is capped at 110
  bits.
- Auto-fire: while the demo is running and not reduced motion, every
  55 + random*30 ticks a volley fires from both bottom corners (12, H-12
  angled 0.7 and W-12, H-12 angled -0.7), stopping once 100 bits are present.
  First input stops automatic launches; a manual burst keeps its finite
  cleanup.
- Each frame: vy += 0.13 (gravity), vx *= 0.99, position and rotation advance,
  sw += 0.12, life -= 0.006. Bits below the floor (y > H+12) or out of life
  are removed. Each bit is drawn as a filled rectangle scaled by cos(sw) on
  the x-axis (the flip), alpha clamped to life*1.4.

SCRIPT (one IIFE, no libraries)
- Theme hook in <head> reads ?theme=light|dark before first paint; a
  forever:theme postMessage listener re-themes live; syncTheme() re-reads
  --c03-pal from computed style (each bit stores a palette index, so a theme
  switch recolours live).
- Demo-v1 preview lifecycle: demoEnabled from window.__foreverPreview; a
  forever:interactive event, pointerdown, click, or non-Tab keydown triggers
  stopDemo() — demo off, takeover on. pointerdown on the canvas fires a
  burst at the cursor.
- DPR-aware resize (capped at 2), visibilitychange stop/play, and a
  prefers-reduced-motion branch: with reduced motion a single static burst is
  fired from the bottom centre and pre-rolled 20 steps, then the loop draws
  one frame and stops.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce) is handled in JS, not CSS: the loop
  runs once and a populated paper field is drawn instead of auto-firing.
- The canvas has no aria role, so the click interaction is invisible to
  assistive tech — the component's documented limitation.

WHAT BREAKS
- Changing the auto-fire interval without touching both the t>next check and
  the next = t + 55 + random*30 reset breaks the volley rhythm.
- Removing the visibilitychange guard or the demo takeover leaves the
  cannon auto-firing when the tab is hidden or after interaction ends.
- The 110-particle cap can suppress bursts under heavy click-spam; raising
  it without touching the bits.length<100 auto-fire gate changes the balance.
- The light re-theme needs the --c03-pal list re-pointed, not just --c03-bg;
  the canvas colours are read from computed style in syncTheme().
- Background is dark brown by default; ?theme=light switches the token
  block. For click-only production use, remove the t>next auto-fire block.
