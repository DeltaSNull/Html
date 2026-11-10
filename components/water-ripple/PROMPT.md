Build a "Water Ripple" calm interactive component as a single HTML file: concentric ripple rings expand and fade across a deep teal pond; moving the pointer stirs ripples, clicking drops a stone. Theme-aware (dark default, light via ?theme=light), one canvas, one script.

PALETTE AND TOKENS
- :root tokens: --c08-bg:#07272b (deep teal ground), --c08-ring:90% 62% (S%
  L% pair of the JS hue cycle), --c08-pond:#0e4d57, --c08-cap:#9fd8de /
  --c08-cap-o:.4 (caption), --c08-line:rgba(150,220,230,.05). The
  :root[data-theme="light"] block re-points every token (--c08-bg:#eef6f7,
  --c08-ring:80% 30%, --c08-pond:#bfe0e4, --c08-cap:#14505a, --c08-cap-o:.5).
- Caption: 11px system-ui, uppercase, letter-spacing .08em, fixed bottom-left,
  opacity .4. Hint ("move to stir · click to drop a stone"): 10px,
  letter-spacing .1em, fixed bottom-right.

THE RIPPLES
- A full-viewport canvas (#c08-c, position:fixed inset:0, tagged
  data-section="stage"); html,body overflow:hidden.
- spawnDrop(x, y, strength): pushes a ripple {x, y, r:2, a:0.6*strength,
  hue:195+random*20} unless 16 are already pending; 12 random drops seed the
  pond on load.
- Auto-drops: every 900ms (dropInterval) a random drop spawns while fewer
  than 24 ripples are alive.
- Pointer: moves faster than 12px within 28ms spawn a ripple (strength up to
  1.4); pointerdown drops a stone (strength 2).
- Loop: each ripple grows 2.4px/frame (dt-scaled) and decays a *= 0.96^dt;
  removed when a < 0.02 or r > 75% of the larger viewport dimension. Drawn as
  two concentric strokes — 1.5px at full alpha, 3px at 0.35 alpha with the
  inner ring at 0.85r — hue ~195–215.

SCRIPT (one IIFE, no libraries)
- Theme hook in <head> reads ?theme=light|dark before first paint; a
  forever:theme postMessage listener re-themes live; syncTheme() re-reads
  --c08-ring / --c08-pond from computed style.
- Demo-v1 preview lifecycle: demoEnabled from window.__foreverPreview; a
  forever:interactive event, pointerdown, or keydown triggers takeover() —
  demo off, interaction for 1100ms. pointermove extends the interaction window.
- DPR-aware resize (capped at 2), visibilitychange stop/start, and a
  prefers-reduced-motion check: with reduced motion the loop runs a single
  frame and stops.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce) is handled in JS, not CSS: the loop
  runs once and stops, leaving a static frame.
- The animation is pure decoration; the captions are plain text, and the
  canvas carries no keyboard interaction beyond the takeover keys.

WHAT BREAKS
- The spawn cap (16 pending drops) and the loop cap (24 live ripples) are
  different numbers — raising one without the other changes click
  responsiveness.
- Ripple removal uses max(W,H)*0.75; shrinking that cap clips big-screen
  ripples early.
- --c08-ring is an S% L% pair read by JS; putting a full colour there breaks
  the hsl() template.
- Background is deep teal by default; ?theme=light switches the token block.
