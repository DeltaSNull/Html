Build a "DNA Helix" loading spinner as a single HTML file: two columns of 10 glowing dots swing left-right in opposite phases, tracing a rotating DNA double helix, with quirky cycling status messages fading in below. Zero dependencies, pure CSS animation, one tiny script.

PALETTE AND TOKENS
- :root tokens (dark defaults): --bg-900:#08090d, --accent-iris:#DA3E8C / --accent-cyan:#C0B459 / --accent-magenta:#A18794 (node colors), --glow-a/--glow-b (background glows), --text-0:#e8ecf6 / --text-2:#aeb6cc, --cap:#C0B459 / --cap-o:.45, --amp:clamp(20px,7vmin,42px) (swing amplitude), --node:clamp(9px,2.4vmin,15px) (node size). The viewport-clamped --amp/--node mean no JS measurement is needed.
- A :root[data-theme="light"] block re-points every token (light ground #f2f4f8, deeper magenta/gold node colors) — this is the shipped light theme; preserve it.

THE HELIX
- body is display:grid, place-items:center on --bg-900 with two radial background glows; .c04-wrap stacks the helix and status line with 24px padding.
- .c04-dna is a positioned field min(40vmin,220px) tall. JS builds 20 spans (.c04-dna__node), left:50% with negative half-node margin, round, glow via a static box-shadow whose colour rides currentColor (keyframes only touch transform/color/z-index so the blur never repaints).
- Keyframes c04-dna-swing / -swing-b (1.8s cubic-bezier(.65,0,.35,1) infinite): column A goes from translateX(-amp) scale(.6) in iris to translateX(+amp) scale(1.1) in cyan with z-index 1→3; column B does the mirror in magenta→iris. Each node gets a negative animation delay of -i*(1.8/COUNT) so the phase cascades down the strand.
- .c04-msg status line (role="status", aria-live="polite") fades between messages every 2600ms via an .is-out opacity transition.
- A small uppercase monospace caption ("DNA Helix", 10px, letter-spacing .08em) sits bottom-left.

SCRIPT (one IIFE, no libraries)
- Builds the 20 nodes: top = t*(h-node) with t=i/(COUNT-1), stagger delay as above. Reads --node via getComputedStyle.
- visibilitychange pauses both the CSS animations (animationPlayState) and the MSGS timer, restarting the timer when visible again.
- A head IIFE reads ?theme=light|dark before first paint; a forever:theme postMessage listener re-themes live.
- prefers-reduced-motion skips the fade transition and sets text directly; the CSS guard collapses all animations.

ACCESSIBILITY AND MOTION
- The helix field carries role="img" and aria-label="DNA double helix loader"; the status line is aria-live="polite".
- @media (prefers-reduced-motion:reduce): all animations collapse to one .001ms iteration; message fades become instant text swaps.

WHAT BREAKS
- Changing the node top math or dropping the negative delay stagger breaks the helix phase cascade into a flat row.
- The z-index crossover needs stacking-context isolation if embedded in complex layouts.
- Removing the currentColor glow pattern and animating box-shadow color directly causes repaint jank on the blur.
- The MSGS messages are quirky demo copy ("Bribing the GPU with extra electrons…") — replace or remove them before production use.
