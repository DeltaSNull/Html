Build a "Dungeon Raycaster" 8-bit dungeon crawler component as a single HTML file: a Wolfenstein-style raycast 3D view next to a live minimap, in chunky pixel-art styling with Press Start 2P type. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- Page CSS: Press Start 2P (Google Fonts), monospace; 8-bit palette — gold #FFCC00, cyan #00E8D8, red #E52521 on black #000; grey #808080 borders with hard offset shadows (6px 6px 0).
- All classes namespaced .bg03-*.

THE DEMO
- A .bg03-wrap region (max-width 400px, 3px white border) holds: a header (DUNGEON CRAWLER // FOV + X/Y position readout), a viewport row (90x100 minimap canvas + raycast 3D view canvas), three controls (◀ TURN / ▲ STEP / TURN ▶), and a footer (FLOOR: B1, TORCH: LIT).
- Minimap: 8x8 dungeon grid (1=wall), player dot in red, cyan sight cone.
- 3D view: per-column raycaster, 60° FOV, fish-eye correction (multiply distance by cos(rayAngle - angle)), distance-based 8-bit wall shading (#cccccc near → #2a2a2a far), ceiling #111 / floor #222.
- Controls: turn ±45°, step 0.6 with wall collision; position readout updates on move.
- Attract mode: auto-rotates every 4s until any pointer/key interaction; stops permanently on the window 'forever:interactive' event.
- Container queries (@container fit-21-dungeon-crawler-fov-raycaster) shrink the layout at 300px and 210px widths.

SCRIPT (one IIFE, no libraries)
- Demo lifecycle: initialize demoEnabled from window.__foreverPreview; stop attract work on 'forever:interactive', pointerdown, click, and non-Tab keydown (capture).
- Pause CSS animations when document.hidden via a data-paused attribute.

ACCESSIBILITY AND MOTION
- role="region" with aria-label; controls are real <button>s with a :focus-visible outline.
- @media (prefers-reduced-motion:reduce): kill all animations/transitions.

WHAT BREAKS
- Removing the fish-eye correction makes walls warp at the edges of the view.
- Changing canvas CSS size without image-rendering:pixelated loses the 8-bit look.
- The attract timer must be cleared on interaction or it fights the user's input.
