Build a "Lissajous Scope" oscilloscope component as a single HTML file: a cyan Lissajous figure drifting and rotating on a dark scope grid with phosphor trail decay and a glowing beam head; a Live button upgrades it to real microphone input. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- Background is hardcoded dark navy #07080d in BOTH the CSS and the canvas fill — update both to re-theme.
- Beam: cyan rgba(34,211,238) at two passes (wide dim halo + bright core) plus a glowing beam head; grid lines at 7%/4% opacity.
- Fonts: ui-monospace / Menlo, uppercase 10px letter-spaced labels.

THE SCOPE
- .aav05-stage: fixed full-viewport region holding the canvas (#aav05-c, 100%×100%) and the Live button (top-right, cyan outline, hover/opacity states, .is-on inverted when live).
- Small fixed caption bottom-left. Canvas resizes with DPR capped at 2; a faint scope grid (crosshair + two concentric circles) is drawn each frame.
- Body is display:grid place-items:center with overflow hidden; no horizontal scroll.

SCRIPT (one IIFE, no libraries)
- synthXY(p, time): X and Y as sums of sines with slow-LFO frequency drift and phase precession (time*0.5) so the figure rotates; N=110 points per frame.
- Phosphor trail: an offscreen canvas holds the persistent buffer — each frame decays it with an rgba(7,8,13,0.13) fillRect, then draws the fresh beam additively (globalCompositeOperation 'lighter'); composited to screen with the grid on top.
- Live path: clicking Live creates an AudioContext INSIDE the click handler only (never before), wires getUserMedia through an AnalyserNode (fftSize 2048) — never connects to ctx.destination; teardown stops tracks and closes the context on pagehide.
- Demo lifecycle: demoEnabled from window.__foreverPreview; first deliberate pointerdown/click/keydown fires stopDemo on the 'forever:interactive' event (live mic stream stays up until teardown).
- visibilitychange pauses (and suspends the live AudioContext) and resumes; prefers-reduced-motion renders one static frame only.

ACCESSIBILITY AND MOTION
- Live button has an aria-label and a :focus-visible outline; the scope field is decorative.
- @media (prefers-reduced-motion:reduce): one static frame, no loop.

WHAT BREAKS
- Breaking the offscreen trail canvas pattern destroys the phosphor effect (the signature look).
- Creating the AudioContext outside the click handler breaks autoplay policy and mic permission flow.
- The background lives in two places (CSS + canvas fill); changing one leaves a visible seam.
- Reducing N too far makes the figure jagged; the two-pass halo pass needs the wide line width for bloom without a blur filter.
