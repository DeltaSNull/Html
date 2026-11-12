Build a "Constellation Connect" cosmic component as a single HTML file: 46 drifting star nodes auto-connect with faint magenta lines when close; the cursor draws lines to nearby stars and brightens them on hover — a constellation network for dark hero backgrounds. Zero dependencies, one canvas, one tiny script.

PALETTE AND TOKENS
- No :root tokens — colours are hardcoded: ground #060a16 (dark navy, in the CSS and in the frame() fillStyle — change both together), link lines rgb(218,62,140) magenta, cursor lines rgb(224,100,170), stars rgba(255,235,245) warm white, caption #cfe2ff at opacity .34.
- There is no light theme variant.

THE NETWORK
- A full-viewport canvas #c (display:block, width/height 100%) holds the whole component; a small uppercase caption ("Constellation Connect", 10px, letter-spacing .08em) sits fixed bottom-left.
- Nodes: N=46, each seeded with a random position (x, y in 0..1), drift velocity vx/vy = (random-0.5)*0.0006, radius 0.6–2.2, and a random twinkle phase ph. Nodes bounce off the unit-square edges by flipping their velocity.
- Each frame: the ground is repainted (#060a16 fillRect); nodes drift; then an O(N²) proximity check links every pair closer than LINK=0.2 with a magenta line whose alpha is (1 - d/LINK)*0.5, lineWidth 0.7.
- Each node twinkles: alpha = 0.5 + (0.6 + 0.4*sin(t + ph))*0.5, t advancing 0.02 per frame. Near the cursor (within 1/4 of the viewport, factor *4), a node grows up to 2.5× its radius and a line is drawn from it to the cursor at alpha near*0.6.
- pointermove/pointerleave track the cursor in normalised coordinates (mx, my, -1 when absent). On touch devices there is no persistent pointer, so cursor lines simply do not appear.

SCRIPT (one IIFE, no libraries)
- DPR-aware sizing: size() caps DPR at 2, sets canvas width/height; a resize listener re-runs it. seed() (re)creates the nodes after sizing.
- start/stop via requestAnimationFrame; visibilitychange pauses when the tab is hidden. With prefers-reduced-motion, node positions freeze and a single frame is drawn — the rAF loop still runs, but the picture is effectively static since nothing moves except the twinkle phase.

ACCESSIBILITY AND MOTION
- The canvas is pure decoration; the caption is pointer-events:none. Reduced motion freezes the drift.

WHAT BREAKS
- Changing the proximity check breaks the constellation — the O(N²) loop is what connects nearby stars.
- Node counts above ~120 strain low-end CPUs (the proximity check is quadratic).
- Changing the twinkle calculation (t += 0.02) changes the star brightness rhythm.
- The background is hardcoded dark navy in two places (CSS and frame()); change both or the repaint will flash the old colour every frame.
