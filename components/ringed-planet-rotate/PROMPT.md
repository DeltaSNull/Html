Build a "Ringed Planet Rotate" ambient component as a single HTML file: an amber gas giant with tilted Saturn-like rings slowly rotates over a twinkling starfield; moving the pointer adjusts the ring tilt and the light angle. One canvas, one script, zero dependencies.

PALETTE AND TOKENS
- No :root tokens — colours are hardcoded: ground #0a0710 (dark
  purple-black, set both in CSS and in the frame() fillStyle), band palette
  #e8a85c / #d98f43 / #f0c074 / #c87c3a / #ecb568, radial-gradient stops
  #ffd9a0 → #d98f43 → #5e3416, ring segments rgba(240,210,160,.55) /
  rgba(200,150,90,.45) / rgba(230,190,140,.3), caption #ffe1b0 at opacity .34.
- Caption: 10px system-ui, uppercase, letter-spacing .08em, fixed
  bottom-left, opacity .34.

THE PLANET
- A full-viewport canvas (#c, position:fixed inset:0, tagged
  data-section="stage"); html,body overflow:hidden.
- size(): DPR capped at 2, canvas sized to the viewport, centre (cx, cy) and
  planet radius R = min(W,H)*0.2. 55 stars are seeded with random x/y (0–1)
  and radius 0–1.1px.
- pointermove sets tilt = 0.22 + (pointerY / H) * 0.5 (default 0.4). There is
  no mouse-leave reset — the tilt stays where the pointer left it until
  reload.
- Each frame: t++, rot += 0.006 (unless reduced motion). The starfield is
  redrawn with twinkle alpha 0.4 + 0.4*|sin(t*0.02 + s.x*7)|. The planet is
  drawn rotated -0.32 rad: back half of the ring, the radial-gradient disc,
  six clipped band stripes (alpha .28, offset by (rot*8)%1 so they drift),
  a linear-gradient shadow terminator (transparent → rgba(0,0,0,.55)), then
  the front half of the ring.
- drawRing(back): scales y by tilt and strokes three ring segments as arcs —
  back half from π to 2π, front half from 0 to π — with radii R*1.35–1.5,
  R*1.55–1.78, R*1.85–2.0 and the opacities above.

SCRIPT (one IIFE, no libraries)
- DPR-aware resize (capped at 2), visibilitychange stop/start, and a
  prefers-reduced-motion guard: the reduce flag stops the rot increment, so
  the planet freezes — but the rAF loop keeps drawing the same static frame
  (the component's documented limitation). With reduced motion frame() is
  called once at load; otherwise start() kicks off the loop.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce) is handled in JS, not CSS: rotation
  stops, but the loop still runs — it just redraws an identical frame.
- The animation is pure decoration; the caption is plain text, and the
  canvas carries no keyboard interaction.

WHAT BREAKS
- Changing R without touching both the band loop (which spans -R to +R) and
  the ring radii (R*1.35–2.0) breaks the planet/ring proportions.
- Removing the visibilitychange guard leaves the loop running when the tab
  is hidden.
- The background is hardcoded in two places — the CSS background and the
  frame() fillStyle — re-theme both together; there is no light variant.
- Ring tilt is pointer-driven only; without pointer movement it stays at the
  0.4 default, and it never resets on mouse-leave.
