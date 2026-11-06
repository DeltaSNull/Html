Build a "Firework Burst" ambient background component as a single HTML file: colorful glowing sparks burst radially, fall under gravity, and fade against a near-black sky. Canvas 2D, zero dependencies, one inline script.

PALETTE AND TOKENS
- No :root tokens — background hardcoded near-black (#06060f) in CSS and in the trail fill (rgba(6,6,15,0.22)). Hue palette lives in the script: hues = [10, 45, 200, 280, 330, 140].
- A small uppercase caption ("Firework Burst", 10px monospace, #ffd76b, opacity .5) sits bottom-right.

THE CANVAS
- A full-viewport canvas #c02-canvas (display:block, width/height 100%) is the whole page; it carries data-section="stage".
- size() sets DPR-aware canvas dimensions (DPR = min(2, devicePixelRatio)) and re-runs on resize.
- burst(cx, cy): picks a hue at random, spawns n = 28 + (Math.random()*16|0) sparks (capped at 110 live particles), each with a radial angle, speed 1–3.6, radius 1–2.6, life 1.
- step(): paints the trail fill (rgba(6,6,15,0.22)) over the frame, auto-launches a burst every ~30–70 frames while demoEnabled (random position in the middle band), then updates each particle: vy += 0.035 (gravity), vx/vy *= 0.985 (drag), life -= 0.014; dead particles are spliced out. Each spark is drawn as an hsl(hue, 95%, 55–70%) circle with globalAlpha = life.
- Pointer: pointerdown on the canvas spawns a burst at the click point. Initial automatic launches stop after the first input (demo-v1 preview lifecycle: demoEnabled from window.__foreverPreview, stopDemo on forever:interactive / pointerdown / click / Tab keydown).
- requestAnimationFrame loop with a document.hidden guard; visibilitychange pauses/resumes.

ACCESSIBILITY AND MOTION
- prefers-reduced-motion: a single static burst at screen centre, then the loop stops — no ongoing animation.
- The canvas is pure decoration; the caption is a plain div.

WHAT BREAKS
- Removing the trail fill (rgba(6,6,15,0.22)) makes sparks draw as hard dots with no fading trails.
- Raising the particle cap (parts.length < 110) without lowering per-burst counts costs frames on low-end devices.
- Light themes need the hardcoded #06060f background and the trail fill repointed; the dark sky is baked in.
- Reduced-motion fires one static burst then stops — it can read as broken rather than calm.
