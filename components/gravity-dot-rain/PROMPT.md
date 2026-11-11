Build a "Gravity Dot Rain" ambient interactive component as a single HTML file: multicoloured dots fall under gravity with motion trails, bounce off the floor, and scatter away from the cursor on a dark navy canvas. Theme-aware (dark default, light via ?theme=light), one canvas, one script.

PALETTE AND TOKENS
- :root tokens: --c01-bg:#0b0f1c (ground), --c01-cap:#6ee7ff / --c01-cap-o:.45
  (caption), --c01-trail:.25 (trail alpha), --c01-pal:#6ee7ff,#a78bfa,#f472b6,
  #34d399,#fbbf24 (comma list feeding the JS palette). The
  :root[data-theme="light"] block re-points every token (--c01-bg:#eef1f8,
  --c01-cap:#00778f, --c01-cap-o:.6, --c01-trail:.32, --c01-pal the darker
  light-variant palette).
- Caption: 10px ui-monospace, uppercase, letter-spacing .08em, fixed
  bottom-left, opacity .45.

THE RAIN
- A full-viewport canvas (#c01-canvas, position:fixed inset:0, tagged
  data-section="stage"); html,body overflow:hidden.
- N = clamp(round(W*H/5200), 24, 90) drops. Each drop: random x, y above the
  viewport, vy 0, radius 1.6–4.2px, palette index, and a b/by pair kept for
  compatibility. Drops spawn across the full height at load (reduced motion
  seeds them in the top 15–80% so the field looks populated).
- Each frame: vy += g (0.16), y += vy. If a drop comes within dd<5000 of the
  pointer it is pushed away: f = 40/max(8, sqrt(dd)), px += dx*f*0.02.
  Hitting the floor (y > H - r) bounces with damping -0.62; once |vy| < 1.2
  the drop recycles to a fresh spawn while the demo is running, or settles
  (vy = 0) after takeover.
- Each drop draws a short trail (line from (px,py) to (px, py - min(18,
  vy*3)), width r*0.9, alpha trailA) then a filled circle of its palette
  colour (arc radius r, 0 to 7 radians).

SCRIPT (one IIFE, no libraries)
- Theme hook in <head> reads ?theme=light|dark before first paint; a
  forever:theme postMessage listener re-themes live; syncTheme() re-reads
  --c01-pal / --c01-trail from computed style (per-drop colour is a palette
  index, so a theme switch recolours live).
- Demo-v1 preview lifecycle: demoEnabled from window.__foreverPreview; a
  forever:interactive event, pointerdown, click, or non-Tab keydown triggers
  stopDemo() — demo off, takeover on. The loop keeps running while the demo
  is on or any drop is still falling or moving.
- DPR-aware resize (capped at 2), visibilitychange pause/play, and a
  prefers-reduced-motion guard: with reduced motion the loop draws one frame
  then stops, and the drops are seeded as visible static grains (cursor
  scatter still redraws on pointermove).

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce) is handled in JS, not CSS: the loop
  runs once and a static grain field is drawn instead of animating.
- The animation is pure decoration; the caption is plain text, and the
  canvas carries no keyboard interaction beyond the takeover keys.

WHAT BREAKS
- Changing the scatter radius without touching both the dd<5000 check and
  the 40/max(8, sqrt(dd)) falloff breaks the cursor feel.
- Removing the visibilitychange guard or the demo takeover leaves the
  animation running when the tab is hidden or after interaction ends.
- The light re-theme needs the --c01-pal list re-pointed, not just --c01-bg;
  the canvas colours are read from computed style in syncTheme().
- After takeover, settled drops are not recycled — the floor fills up and
  stays still until reload; that is the component's documented behaviour.
- Background is dark navy by default; ?theme=light switches the token block.
