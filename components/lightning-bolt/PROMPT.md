Build a "Lightning Bolt" ambient background component as a single HTML file: jagged lightning bolts with branching forks strike randomly down a dark storm-sky canvas; the screen flashes rust-white on each strike. Tap anywhere to trigger a strike instantly. Canvas 2D, zero dependencies, one inline script.

PALETTE AND TOKENS
- No :root tokens — the sky gradient (#201d1a → #0a0908), bolt colors (rust glow #AE6147, warm white #f5efe6, core #ffffff, branch #efe6da) and flash tint (rgba(196,132,110,...)) are hardcoded in the script; the caption color (#e8ddd0, opacity .34) lives in CSS.
- A small uppercase caption ("Lightning Bolt · tap", 10px system sans, opacity .34) sits bottom-left.

THE CANVAS
- A full-viewport canvas #c (display:block, width/height 100%) is the whole page; it carries data-section="stage".
- size() sets DPR-aware canvas dimensions (DPR = min(2, devicePixelRatio)) and re-runs on resize.
- makeBolt(sx): builds a bolt of 14 segments from the top of the screen to the bottom, each step drifting horizontally by a random (W*0.16) spread and clamped to 10–90% of the width; then adds branches (35% chance per segment from the 3rd to the 3rd-last, each 4 steps long, drifting down-right).
- strike(sx): fires a bolt at a random x (30–70% of width, or the tap position), sets flash=1, and plants a lingering afterglow at the strike tip.
- frame(): paints the sky gradient, then the flash overlay (flash*0.28 alpha, fading 0.08/frame), then the afterglow radial gradient (~3s decay), then the bolt in three strokes (8px rust at 0.25 alpha, 3px warm white at 0.9, 1.2px white core) plus 1.6px branches, drawn with globalCompositeOperation "lighter". Bolt life fades 0.045/frame.
- Auto-strikes: the first bolt lands on the first frame, then every 26–70 frames (~0.4–1.2s); pointerdown on the canvas triggers a strike at the tap x.
- requestAnimationFrame loop with visibilitychange pause/resume.

ACCESSIBILITY AND MOTION
- prefers-reduced-motion: strikes slow to one every 120 frames and fades slow down (flash 0.06/frame, glow 0.9 decay) — flashes still happen, so this is a photosensitivity risk, not a safe static mode.
- The canvas has no aria label (a known limitation); the caption is a plain div.

WHAT BREAKS
- Removing globalCompositeOperation "lighter" kills the bolt glow — strokes paint as flat lines on the dark sky.
- Background color is hardcoded in the script's sky gradient — change both gradient stops to retheme.
- Only one bolt at a time is supported; striking while a bolt is alive replaces it.
