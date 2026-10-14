Build a "Plasma Sine Field" demoscene component as a single HTML file: five interfering sine waves rendered low-res and upscaled for a molten blur; a 256-entry palette cycles through cyan, magenta, and violet. Canvas 2D, zero dependencies, one inline script, dark only.

THE PLASMA
- Full-viewport canvas #psf-c (getContext('2d',{alpha:false})) inside an explicit 100%x100% [data-section="stage"] wrapper (overflow:hidden). DPR-aware sizing (DPR capped at 2, setTransform); resize re-inits the buffers.
- Low-res render: CELL=max(3, round(min(W,H)/72)) — the adaptive coarseness divisor (72) is both the look and the performance budget; cols/rows cover the backing store plus one.
- Per cell, v is the classic plasma sum of five interfering sines: sin(fx*1.3+t1) + sin(fy*1.7+t2) + sin((fx+fy)*0.9+t1*1.4) + sin(dist((x,y),(cxo,cyo))*inv*1.1+t2*1.7) + sin(fx*fy*0.8+t3), with inv=4.0/min(cols,rows), t1=t*0.013, t2=t*0.021, t3=t*0.009. Map v from [-5,5] to a palette index: ((v*25.5+128+t*1.6)|0)&255 — the +t*1.6 term cycles the palette over time (hue-rotate feel).
- Palette: 256 entries precomputed once; channel sine phases 0.0 (r), 2.1 (g), 4.2 (b); biased toward accent cyan (#19f0ff) and deep violet with the floor lifted off pure black (r=0.06+r*0.82, g=0.04+g*0.94, b=0.18+b*0.82), plus accent flares from pow(sin(a*1.5)*0.5+0.5, 2).
- Molten blur: cells are written to a tiny ImageData, put to an offscreen canvas, then drawn upscaled to the full backing store with imageSmoothingEnabled=true and imageSmoothingQuality='high'. That upscale IS the blur — do not render at full res.
- Interaction: pointermove sets the plasma center (px,py, normalized) and boost=1; click sets boost=2.4. Each frame t += 1+boost*4 and boost *= 0.93 — the boost decays back to ambient quickly, so it is a pulse, not persistent state.
- A small uppercase monospace caption ("Plasma Sine Field", 10px, letter-spacing .14em, color #19f0ff, opacity .34, pointer-events:none) sits bottom-left, fixed.

LIFECYCLE AND MOTION
- visibilitychange pauses/resumes the rAF loop. prefers-reduced-motion: set t=40, render once, blit once, never loop.

WHAT BREAKS
- Rendering at full resolution (removing the low-res buffer) turns the molten blur into hard pixels and tanks the frame rate — the coarseness divisor (72) is the look AND the perf budget.
- The palette is tuned to the hardcoded dark-navy bg (#05031a); a light background needs the whole PAL table re-tuned, not just the CSS bg.
- The per-pixel CPU loop is heavy on low-end devices; keep cols×rows small (the divisor 72 does this automatically).
