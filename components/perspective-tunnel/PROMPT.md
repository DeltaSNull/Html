Build a "Perspective Tunnel" CSS-3D component as a single HTML file: 18 neon rings recede through a CSS perspective tunnel, cycling rainbow hues and rotating as they rush toward the viewer; pointer X steers the spin rate. Theme-aware (dark default, light via ?theme=light), no canvas, one script.

PALETTE AND TOKENS
- :root tokens: --c09-bg:#05060f (dark navy ground), --c09-ring:90% 62% (S%
  L% pair of the JS hue cycle), --c09-glow:rgba(106,208,255,.45) (ring glow),
  --c09-fade:.15 (farthest-ring opacity), --c09-cap:#6ad0ff / --c09-cap-o:.34
  (caption). The :root[data-theme="light"] block re-points every token
  (--c09-bg:#f2f4fb, --c09-ring:72% 34%, --c09-glow:rgba(30,60,120,.16),
  --c09-fade:.34, --c09-cap:#14203a, --c09-cap-o:.5).
- Stage: #ce9-stage fixed inset:0, flex-centred, perspective:420px;
  #ce9-tunnel 62vmin square, transform-style:preserve-3d. Rings: 2px solid
  border, border-radius:10px, box-shadow 0 0 12px var(--c09-glow).
- Caption: 11px system-ui, uppercase, letter-spacing .08em, fixed bottom-left,
  opacity .34.

THE TUNNEL
- N=18 rings, SPAN=1800 depth units; ring i starts at z = -(i/N)*SPAN.
- Loop: roll += spin (0.25 base); each ring advances z += 4/frame while demo
  runs, wraps (z -= SPAN) past z=200; depth = (z+SPAN)/SPAN (0 far … ~1 near);
  hue = (z*0.12 + roll*2) % 360; opacity = fade + depth*(1-fade); transform =
  translateZ(z) rotateZ(roll + z*0.05) scale(0.2 + depth*0.9).
- Pointer: pointermove sets spin = 0.25 + (clientX/innerWidth - 0.5)*1.6, so
  the left half slows or reverses the spin and the right half speeds it.

SCRIPT (one IIFE, no libraries)
- Theme hook in <head> reads ?theme=light|dark before first paint; a
  forever:theme postMessage listener re-themes live; syncTheme() re-reads
  --c09-ring / --c09-fade from computed style and repaints each ring's last
  hue so a switch shows even when the loop is paused.
- Demo-v1 preview lifecycle: demoEnabled from window.__foreverPreview; a
  forever:interactive event, pointerdown, or keydown triggers takeover() —
  demo off, interaction for 1100ms. pointermove extends the interaction window.
- visibilitychange stop/start, and a prefers-reduced-motion check: with
  reduced motion loop() runs once to place the rings, then stops.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce) is handled in JS, not CSS: the
  tunnel freezes on its first frame — there is no animated fallback.
- The animation is pure decoration; the caption is plain text.

WHAT BREAKS
- N and SPAN are tightly coupled — changing the ring count without adjusting
  SPAN shifts ring spacing.
- The hue formula uses o.z*0.12; the 0.12 factor and roll*2 together set the
  rainbow spread.
- Removing preserve-3d or perspective collapses the tunnel to flat
  overlapping squares.
- With reduced motion there is no animated fallback — the tunnel freezes on
  its first frame.
- Background is dark navy by default; ?theme=light switches the token block.
