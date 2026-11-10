Build a "Smoke Vortex Street" ambient interactive component as a single HTML file: 190 smoke particles curl through an alternating vortex street, glowing as you stir them with your pointer; click for a burst. Theme-aware (dark default, light via ?theme=light), one canvas, one script.

PALETTE AND TOKENS
- :root tokens: --c02-bg:#07080d (ground), --c02-smoke:88% 58% (S% L% pair of
  the JS hue cycle), --c02-edge:#9d9aa0 (particle edge strokes), --c02-cap:#9d9aa0
  / --c02-cap-o:.36 (caption), --c02-line:rgba(150,150,158,.06). The
  :root[data-theme="light"] block re-points every token (--c02-bg:#f4f6fa,
  --c02-smoke:80% 30%, --c02-edge:#3a3f4a, --c02-cap:#3a3f4a, --c02-cap-o:.5).
- Caption: 11px system-ui, uppercase, letter-spacing .08em, fixed bottom-left,
  opacity .36. Hint ("move to stir · click for a burst"): 10px, letter-spacing
  .1em, fixed bottom-right.

THE VORTEX FIELD
- A full-viewport canvas (#c02-c, position:fixed inset:0, tagged
  data-section="stage"); html,body overflow:hidden.
- field(x,y,t): u = sin(y*0.006 + t*0.7)*22 + cos((x+y)*0.002 - t*0.3)*10;
  v = sin(x*0.006 - t*0.7)*22 + sin((x-y)*0.002 + t*0.3)*10 — alternating
  street vortices plus gentle drift.
- N=190 particles, each with random x/y, angle a, age, life 6–12s, size factor
  s 0.3–1.0. Particles are pulled by the field (velocity damped 0.92, field
  weighted 0.04) and wrap at the edges (±20px); when a particle outlives its
  life it respawns at a random spot.
- Pointer stir: within 160px of the pointer, particles are pushed away and
  dragged by pointer velocity (×0.12). While the pointer is down, particles
  within 110px are pushed harder (×0.8). Click bursts 26 new particles (life
  1–2.5s, size 0.6–1.5); the array is capped at 520.
- Each particle fades in and out over its life (sine curve), radius
  2 + s*14*fade, hue drifting 0.25/frame from 140, drawn as an hsl(h, smokeSL)
  circle with a faint edge stroke (0.6px, alpha 0.25*fade) trailing behind it.

SCRIPT (one IIFE, no libraries)
- Theme hook in <head> reads ?theme=light|dark before first paint; a
  forever:theme postMessage listener re-themes live; syncTheme() re-reads
  --c02-smoke / --c02-edge from computed style.
- Demo-v1 preview lifecycle: demoEnabled from window.__foreverPreview; a
  forever:interactive event, pointerdown, or keydown triggers takeover() —
  demo off, interaction for 1100ms. pointermove extends the interaction window.
- DPR-aware resize (capped at 2), visibilitychange stop/start, and a
  prefers-reduced-motion check: with reduced motion the loop draws one frame
  then stops, plus a static scatter of 60 particles.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce) is handled in JS, not CSS: the loop
  runs once and a static scatter is drawn instead of animating.
- The animation is pure decoration; the captions are plain text, and the
  canvas carries no keyboard interaction beyond the takeover keys.

WHAT BREAKS
- Changing the 160px stir radius without touching both the distance check
  (d2 < 160*160) and the falloff (1 - d/160) breaks the stir feel.
- Removing the visibilitychange guard or the demo takeover leaves the
  animation running when the tab is hidden or after interaction ends.
- The light re-theme needs the --c02-smoke S% L% pair re-pointed, not just
  --c02-bg; the canvas colour is read from computed style in syncTheme().
- Background is dark by default; ?theme=light switches the token block.
