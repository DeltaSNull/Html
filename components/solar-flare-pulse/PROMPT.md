Build a "Solar Flare Pulse" ambient space component as a single HTML file: a fiery orange sun with a pulsing radial corona, 60 radiating flare spikes, and 5 arcing prominences; clicking triggers a brighter solar burst. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- Hardcoded dark ground #1a0703 — in both the CSS and the canvas fill; change both to retheme.
- Corona rgba(255,160,40)/rgba(255,90,20); sun gradient #fff2c0 → #EEBAA0 → #e8521a; flare gradient rgba(255,210,120) → transparent; prominence strokes rgba(255,140,50,0.5).
- A small uppercase caption bottom-left, opacity .36, pointer-events none.

THE SUN
- Full-viewport canvas. Sun radius R = min(W,H) × 0.21, centered.
- Corona: radial gradient out to R×(2.4 + pulse×0.5), where pulse = 0.5 + 0.5×sin(t×1.3) + kick.
- 60 flare spikes: each with random angle, base length, phase, speed; length grows with its sine and with kick×0.6, drawn as gradient strokes from the sun's edge outward.
- 5 prominences: quadratic arcs from the sun's edge, height modulated by their own sine.
- Sun body: offset radial gradient plus 10 mottling blobs clipped to the disc.
- Click (pointerdown) sets kick=1; it decays ×0.93 per frame and fattens the corona, spikes, and sun body.

SCRIPT (one IIFE, no libraries)
- DPR-aware sizing with a resize listener; visibilitychange stop/start.
- prefers-reduced-motion: time frozen (t never advances), one static frame drawn.

WHAT BREAKS
- Changing only the CSS background leaves the canvas fill #1a0703 — the field still paints dark.
- The frame loop always requests the next frame, even reduced-motion — the freeze works because t stops advancing, not because the loop stops; don't "optimize" it into a single draw without keeping the resize behavior.
- Kick decay is per-frame (×0.93), so burst duration varies with frame rate.
