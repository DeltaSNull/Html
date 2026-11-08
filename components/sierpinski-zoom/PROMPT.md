Build a "Sierpinski Zoom" ambient background component as a single HTML file: a Sierpinski triangle fractal endlessly zooms into itself with rainbow-cycling hues in a seamless self-similar loop. Tap to burst the zoom speed. Canvas 2D, zero dependencies, one inline script.

PALETTE AND TOKENS
- :root tokens: --sz-bg:#120c06 (dark brown ground), --sz-cap:#d8c08a (caption), --sz-cap-o:.34, --sz-c0:18,192,100 / --sz-c1:230,190,50 (ends of the size hue ramp the fractal stroke interpolates along), --sz-fill-a:0.05 (shallow-fill alpha).
- A [data-theme="light"] block repoints the tokens to a warm parchment ground (#f6efe1) with green-to-ochre hues; ?theme=light or a {type:'forever:theme'} postMessage switches at runtime.
- A small uppercase caption ("Sierpinski Zoom", 10px system sans, opacity .34) sits bottom-left.

THE CANVAS
- A full-viewport canvas #sz-c (display:block, width/height 100%) is the whole page; it carries data-section="stage".
- size() sets DPR-aware canvas dimensions (DPR = min(2, devicePixelRatio)) and re-runs on resize.
- draw(t): base triangle size s = min(W,H)·0.62, zoom phase z = ((t % period) / period + burst·0.9) % 1 with period=6000, scale = Math.pow(2, z) — the seamless scale wrap that makes the loop endless. The whole drawing is translated to (W/2, H·0.52), rotated by sin(t·0.0002)·0.05, and scaled by the zoom.
- tri(x, y, size, d): recursive — draws one triangle (stroke hsl(hue, 80%, 60%) with hue = (t·0.03 + size·0.5 + d·40) % 360, faint fill interpolated between --sz-c0 and --sz-c1 by hue/360, lineWidth max(0.5, 2.5 − d·0.3)), then recurses into three half-size triangles until depth=6.
- Click on the canvas sets burst=1.8 (decaying by ·0.97 per frame) for a burst of zoom speed.
- requestAnimationFrame loop with visibilitychange pause/resume; theme tokens are re-read from CSS on a theme flip.

ACCESSIBILITY AND MOTION
- prefers-reduced-motion: draws a single static frame (draw(0)) and never starts the loop.
- The canvas has no aria label (a known limitation); the caption is a plain div.

WHAT BREAKS
- Removing the Math.pow(2, z) scale wrap or the modulo on the zoom phase breaks the seamless loop — the zoom jumps instead of cycling.
- Background color is hardcoded in both CSS and the JS fillRect — change both to retheme.
- Raising depth above 7 adds recursive fill calls per frame and may stutter on low-end devices.
