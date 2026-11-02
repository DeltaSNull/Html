Build a "Holographic Iridescent" 3D-tilting trading card as a single HTML file: a warm iridescent sheen and conic foil chip that shift with pointer position, drifting idly until first deliberate input. Zero dependencies, one small script, full light-theme re-theme support.

PALETTE AND TOKENS
- :root tokens (dark defaults): --text-0:#f4f6fb, --text-1:#c7ccdb, --accent-iris:#EBD5A2, --border-mid (12% white), --r-md:14px, --space-1:4px, --space-6:24px, --font-display:"Space Grotesk", --font-body:"Inter", --cap-o:.4, --c08-page-a:#101019 / --c08-page-b:#08090d (page gradient), --c08-card-a:#14131c / --c08-card-b:#0a0a12 (card gradient), --c08-sheen-blend:color-dodge, --c08-sheen-1/2/3 (warm gold sheen rgba), --c08-grain, --c08-foil-glow.
- A :root[data-theme="light"] block re-points every token (light page #eef1f6–#f2f4f8, white card, sheen blend multiply, darker gold sheen) — this is the shipped light theme; preserve it.

THE CARD
- A full-viewport #c08-stage (position:fixed, perspective:1100px, overflow hidden by html,body) centers .c08-holo: 46vmin × 59vmin (max 230×296px), preserve-3d, rotated by --c08-rx/--c08-ry custom properties, 110ms linear transition, cursor pointer, border + card gradient, tabindex=0 with focus-visible outline, aria-label.
- Layers inside: .c08-holo__sheen (115deg linear gradient, 240% size, position driven by --c08-sheen, mix-blend-mode from token, opacity .85), .c08-holo__grain (repeating diagonal gradient, blend overlay, opacity .5), .c08-holo__foil (44px rounded chip top-left, conic-gradient gold, glow box-shadow), .c08-holo__body (bottom-anchored title "Holo Foil" + caption "Tilt to shift the rainbow.").
- A small uppercase caption ("Holographic Iridescent", 11px, letter-spacing .08em) sits bottom-left.
- A head IIFE reads ?theme=light|dark before first paint and stamps data-theme (no dark flash). A forever:theme postMessage listener re-themes live.

SCRIPT (one IIFE, no libraries)
- apply(px,py) maps pointer position to --c08-ry/--c08-rx (±12deg max) and --c08-sheen (0–100%). pointermove/pointerdown/pointerleave drive it.
- Idle drift: requestAnimationFrame loop with t+=0.011 applies a sine/cosine wander when not hovering — until the first deliberate input (pointerdown/click/arrow key, or the forever:interactive event) calls stopDemo, which stops attract work while manual feedback keeps settling.
- Keyboard: arrow keys tilt the card by .08 steps (preventDefault).
- visibilitychange stops/starts the loop; prefers-reduced-motion skips starting it and collapses transitions to .001ms.

ACCESSIBILITY AND MOTION
- The card is focusable with a visible outline and keyboard tilt; aria-label on the card.
- @media (prefers-reduced-motion:reduce): transitions collapse, drift loop never starts.

WHAT BREAKS
- Removing the --c08-rx/--c08-ry custom-property plumbing (or the transition) makes the tilt snap instead of easing.
- Dropping the demo-v1 lifecycle marker (meta name="forever-preview-lifecycle") or the forever:interactive handler breaks preview embedding.
- Sheen uses mix-blend-mode:color-dodge tuned for the dark card; recoloring to a light palette without switching the blend token (to multiply, as the light theme does) washes the sheen out.
- Light backgrounds need the [data-theme=light] tokens — the dark gradient is the hardcoded default.
