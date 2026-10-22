Build a "Starfield Warp" ambient background component as a single HTML file: stars streak outward from the center as cyan/purple/blue light trails, simulating hyperspace warp, with auto-spawning that keeps density constant on any viewport. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- :root tokens: --bg-900:#08090d (ground, hardcoded near-black — in both the CSS and the JS), --text-2:#8a91a8 (caption), --font-display:"Space Grotesk",system-ui,sans-serif.
- Trail tint palette lives in the JS tint() function: bright cyan rgba(174,246,255, (t>0.66), purple rgba(124,92,255, (t>0.33), blue rgba(34,211,238, (rest).

THE WARP
- A full-viewport .frame holds .stage (overflow:hidden) with the .c08-warp canvas (aria-hidden="true") and a small uppercase caption ("Starfield Warp", 11px, letter-spacing .14em, opacity .7, pointer-events:none) bottom-left.
- Stars spawn near the center and accelerate outward: speed = 0.4 + random*1.6, radius advances by speed*(1+r/maxR*3), so outer stars trail longer.
- Each frame: motion-blur fill rgba(8,9,13,0.35), then each star drawn as a line from its previous to its current radius. Alpha ramps with radius (min(1, r/(maxR*0.5))); line width = min(2.4, 0.4+r/maxR*4).
- Star count = Math.round((w*h)/1600), clamped 80..220; stars that exit the frame respawn near the center, keeping density constant at any viewport size.

SCRIPT (one IIFE, no libraries)
- DPR-aware canvas sizing (capped at DPR 2) via ResizeObserver; rebuilds the field on resize.
- runLoop ticks requestAnimationFrame and pauses on visibilitychange.

ACCESSIBILITY AND MOTION
- prefers-reduced-motion: no loop — draws one static starfield of 90 soft dots instead.

WHAT BREAKS
- Removing the 0.35-alpha fill (or replacing it with a clearRect) destroys the trail blur; the warp reads as scattered dots.
- Light themes need --bg-900 repointed in CSS *and* both hardcoded colors in JS (the 0.35 fill and the reduced-motion fill) — three places, not one.
- The 220-star cap keeps 4K fill cost sane; raising it scales per-frame line draws linearly.
