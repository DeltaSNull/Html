Build a "Glyph Corruption Cascade" ambient canvas component as a single HTML file: a green monospace glyph grid over a near-black field, with a descending wavefront that corrupts clean ASCII into flickering box-drawing mojibake; the cursor nudges the corruption band. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- Hardcoded palette (no :root tokens): BG #021207 (ground), ACC #39ff14 (corrupt glow and flash blocks), #2fae3a (clean glyph green).
- Type: ui-monospace stack (SFMono-Regular, Menlo, monospace), bold, sized from the smaller viewport dimension (min(W,H)/26, floor 9px).

THE GRID
- A full-viewport canvas (#gcc-c) holds the grid: cell size from fontPx (width 0.62x, height 1.18x), then re-divided so the grid exactly fills the viewport (cw = W/cols, ch = H/rows).
- Two glyph sets: CLEAN (lowercase, digits, punctuation) and CORR (block, shade, box-drawing and mojibake characters). Each cell re-rolls on its own timer (3–13 frames when corrupt, 20–110 when clean).
- The corruption wavefront (front, fspeed starting 0.004) sweeps down and resets to a random start above the viewport; within the 0.34 band below the front, corruption probability rises quadratically with band proximity and intensity.
- Corrupt glyphs fake bloom with four faint offset draw copies plus a bright center glyph; flashing cells invert to an ACC block with the glyph knocked out in BG. Clean glyphs render in dim green, brightening near the band.
- A gradient band glow (10% ACC fading to transparent over 0.34 of viewport height) follows the front.
- Pointer move nudges the front to the cursor (front = cursorY − 0.05, intensity 1); click resets the front near the top with intensity 1.2.
- A small uppercase caption ("Glyph Corruption Cascade", 10px monospace, letter-spacing .14em, opacity .32, pointer-events:none) sits bottom-right.

SCRIPT (one IIFE, no libraries)
- DPR-aware sizing (min(devicePixelRatio, 2)) with ctx.setTransform and a resize listener that rebuilds the grid.
- drawFrame(step) advances the wavefront, re-rolls cells, and draws; rAF continues only while step is nonzero and reduced motion is off.
- visibilitychange pauses (cancelAnimationFrame) and resumes the loop.

ACCESSIBILITY AND MOTION
- With prefers-reduced-motion, a single static frame is drawn: mostly clean glyphs with one corrupted band at front 0.4 — no loop ever starts.
- The canvas is pure decoration; there is no text a screen reader must read.

WHAT BREAKS
- Changing BG in only one place breaks the contract: the background color is hardcoded in both CSS and the JS BG constant — change both.
- Removing DPR-aware sizing blurs the glyphs on retina screens.
- Raising the band width or fspeed too far turns the whole grid into noise; the 0.34 band is what keeps clean text readable above the wavefront.
- Light backgrounds do not work — the palette is hardcoded dark, and the motion competes with any overlaid text.
