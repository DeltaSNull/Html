Build a "3D Tilt Parallax" card component as a single HTML file: a dark card tilts in 3D toward the cursor while badge, orb, bar, title and subtitle layers float at distinct Z-depths. A radial shine tracks the pointer; the card auto-rotates until the first deliberate input.

PALETTE AND TOKENS
- :root custom properties (dark defaults; a [data-theme="light"] block
  overrides): --bg-800:#0d0f16, --bg-surface:#1c2030, --text-0:#f4f6fb,
  --text-2:#8a91a8, --accent-iris:#E8B0B5, --accent-cyan:#D8C197,
  --accent-magenta:#BCA9AB, --grad-iris-cyan, --border-mid, --r-md:14px,
  --r-pill:999px, --glow-cyan, --shadow-lift, --font-display (Space Grotesk via
  Google Fonts), --font-body (Inter via Google Fonts), --ease-out-soft,
  --dur-base:280ms. Page ground is a radial --c01-page-a/#12141d →
  --c01-page-b/#08090d wash.

THE CARD
- #c01-stage: fixed inset 0, flex-centered, perspective:1100px.
- .c01-tilt: 46vmin × 59vmin (capped at 230×296px), transform-style:preserve-3d,
  rotateX(var(--c01-rx)) rotateY(var(--c01-ry)), transition transform 280ms
  ease-out-soft. While hovering, .c01-tilt--live switches to a 90ms linear
  transition so the card tracks the cursor tightly.
- Layers, all preserve-3d: the plate (inset 0, radial plate-glow over a
  bg-surface→bg-800 gradient, 1px border, shadow-lift, overflow hidden) with
  a shine child — radial-gradient at var(--c01-mx)/var(--c01-my),
  rgba(255,255,255,0.28)→transparent 45%, screen blend, opacity
  var(--c01-shine); badge (38px, translateZ 60px, grad-iris-cyan, glow);
  orb (56px circle, translateZ 95px, radial magenta→fade, glow); bar
  (translateZ 40px); title (translateZ 55px, Space Grotesk 15px); sub
  (translateZ 45px, 12px text-2).
- The card is tabindex="0" with an aria-label and a focus-visible outline.
  A small uppercase caption sits bottom-left.

SCRIPT (two inline scripts, no libraries)
- Head theme hook, before first paint: reads ?theme=light|dark and stamps
  data-theme on <html> so there is no dark flash. Standalone default is dark.
- One IIFE: pointermove/pointerdown on the card → apply(px,py) sets
  --c01-ry=(px−.5)·28deg, --c01-rx=(.5−py)·28deg (max=14), --c01-mx/--c01-my
  for the shine position, --c01-shine=1. pointerleave resets all four.
- Idle: an rAF loop (t+=0.012) auto-rotates px=0.5+0.32·sin(t),
  py=0.5+0.28·cos(t·0.8) while not hovering.
- Keyboard: arrow keys tilt the card in 0.08 steps.
- First deliberate input (forever:interactive / pointerdown / click / keydown)
  stops the attract loop via stopDemo; manual control keeps working.
- visibilitychange stop/start; a {type:'forever:theme'} postMessage switches
  data-theme live.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce): transition-duration collapses to
  .001ms and the idle loop never starts. Keyboard-operable via arrow keys.

WHAT BREAKS
- Removing transform-style:preserve-3d flattens every layer onto the card.
- translateZ does nothing without the perspective on #c01-stage.
- The shine positions itself with CSS custom properties — modern browsers
  only, no fallback.
- The card is vmin-sized, so it can read small on landscape mobile viewports.
- Dark is the standalone default; ?theme=light or the forever:theme message
  switches to the light token set.
