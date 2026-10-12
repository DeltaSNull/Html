Build an "Aurora Borealis" ambient background component as a single HTML file: three blurred, screen-blended gradient ribbons drifting and skewing over a near-black field, like northern lights in blush, cream, and rose. Zero dependencies, CSS-only animation, one tiny script.

PALETTE AND TOKENS
- :root tokens: --bg-900:#08090d (ground, hardcoded near-black), --accent-iris:#E8B0B5, --accent-cyan:#D8C197, --accent-magenta:#d9a3ac (ribbon gradient stops), --text-2:#8a91a8 (caption), --font-display:"Space Grotesk",system-ui,sans-serif, --ease-in-out:cubic-bezier(0.65,0,0.35,1).

THE RIBBONS
- A full-viewport .stage (overflow:hidden) holds .c01-aur (aria-hidden="true") with three .c01-aur__ribbon divs and a .c01-aur__veil.
- Each ribbon: position absolute, left:-30%, width:160%, height:70%, border-radius:50%, filter:blur(34px), mix-blend-mode:screen, will-change:transform. The 160% width inside overflow:hidden is what prevents horizontal scroll — keep it.
- Ribbon A (top:-10%, opacity .85): linear-gradient(90deg, transparent, iris 35%, cyan 65%, transparent), animation c01-aur-drift-a 9s var(--ease-in-out) infinite alternate.
- Ribbon B (top:18%, opacity .7): gradient transparent → cyan 40% → magenta 70% → transparent, drift-b 11s.
- Ribbon C (top:40%, opacity .55): gradient transparent → magenta 30% → iris 60% → transparent, drift-c 13s.
- Keyframes drift each ribbon with translateX/translateY/skewY/scaleY between two states (e.g. -8%→10% X, -4deg→5deg skew). Different durations keep the motion unsynchronised.
- The veil: linear-gradient(180deg, transparent 40%, rgba(8,9,13,.6) 100%) darkens the lower half so foreground content stays readable.
- A small uppercase caption ("Aurora Borealis", 11px, letter-spacing .14em, opacity .7, pointer-events:none) sits bottom-left.

SCRIPT (one IIFE, no libraries)
- On visibilitychange, set animationPlayState paused/running on all ribbons to save cycles when the tab is hidden.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce): collapse all animations (animation-duration:.001ms, iteration-count:1).
- The ribbon field is aria-hidden; it is pure decoration.

WHAT BREAKS
- Removing mix-blend-mode:screen makes the ribbons muddy grey instead of glowing light.
- Changing the ribbon width/left without the overflow:hidden stage causes horizontal scroll.
- Light themes need --bg-900 and the veil repointed; the dark ground is hardcoded.
