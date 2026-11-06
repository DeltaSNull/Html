Build a "Comet Streak Loop" ambient background component as a single HTML file: cyan and violet comets with glowing heads and gradient tails streak diagonally across a twinkling dark starfield in a slow loop. Canvas 2D, zero dependencies, one inline script.

PALETTE AND TOKENS
- No :root tokens — background hardcoded dark navy (#030a14) in CSS and in the frame fill. Comet hue palette lives in the script: '#7fe8ff' (cyan) and '#c9b6ff' (violet).
- A small uppercase caption ("Comet Streak Loop", 10px system sans, #bff0ff, opacity .34) sits bottom-left.

THE CANVAS
- A full-viewport canvas #c (display:block, width/height 100%) is the whole page; it carries data-section="stage".
- size() sets DPR-aware canvas dimensions (DPR = min(2, devicePixelRatio)) and re-runs on resize.
- 60 background stars with random positions and radii (up to 1.2px) twinkle via alpha = 0.3 + 0.5·|sin(t·0.02 + x·9)|.
- spawn(p0): pushes a comet with speed sp = 0.0045 + random·0.004, lane = 0.1 + random·0.8, tail length len = 0.20 + random·0.13, hue picked at random from the two palette colours. Two comets spawn at start (p = 0.1, 0.55).
- frame(): fills the navy background, draws the twinkling stars, then for each comet computes its head position interpolating from off-screen top-right to off-screen bottom-left (direction dx=-0.78, dy=0.62), draws a gradient tail (hue → transparent) with lineWidth 2.4 and round caps, and stamps a pre-rendered radial glow sprite at the head. Comets past p > 1.3 are removed and a new one spawns at p = 0.
- Pointer: pointerdown spawns an extra comet at p = 0 (cap 5 comets). Comets auto-respawn on exit anyway, so the click is a bonus, not the engine.
- requestAnimationFrame loop with visibilitychange pause/resume; reduced-motion renders a single static frame.

ACCESSIBILITY AND MOTION
- prefers-reduced-motion: frame() runs once and stops — comets are frozen in place, not removed.
- The canvas is pure decoration; the caption is a plain div.

WHAT BREAKS
- Removing the pre-rendered glow sprites (glowSprite) and using shadowBlur per frame instead re-blurs every comet head every frame and costs cycles.
- Light themes need the hardcoded #030a14 background and frame fill repointed; the dark navy is baked in.
- Raising the comet cap above 5 without lowering tail length or star count adds draw calls per frame.
- Reduced-motion freezes the comets mid-flight — a static frame, not an empty sky.
