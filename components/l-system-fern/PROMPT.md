Build an "L-System Fern" fractal component as a single HTML file: a fern draws itself branch by branch via an L-system turtle grammar, colouring segments brown-to-gold by depth, holding 2.2s when complete, then restarting. Zero dependencies, one canvas, one IIFE.

PALETTE AND TOKENS
- :root tokens: --fr05-bg:#06070b (near-black ground), --fr05-g1:rgba(178,146,52,.16) / --fr05-g2:rgba(200,170,110,.05) (radial glow), --fr05-cap:#fff / --fr05-cap-o:.42 (caption), --fr05-tip:#F7EFD9 (growing-tip gold), --fr05-p0..p7 (by-depth stroke palette: #2e2210 deep brown trunk -> #6b4a1f -> #C08459 -> #B29234 -> #A19A87 -> #D9B878 -> #EAD9A8 -> #F7EFD9 cream tips).
- A [data-theme="light"] block repoints them (--fr05-bg:#f6f2ea pale paper, the palette inverted so tips are the deepest sepia ink and the trunk sits just off the paper). A theme-hook script in <head> reads ?theme=light|dark from the URL and stamps data-theme on <html> before first paint, so there is no dark flash. Standalone default stays dark. A {type:'forever:theme'} postMessage switches theme at runtime.

THE FERN
- A full-viewport canvas #fr05-c (position:absolute, inset:0, width/height 100%) inside #fr05-stage (fixed inset:0, radial-gradient glow background); the stage is role="img" with an aria-label describing the growing fern. A small uppercase monospace caption ("L-System Fern", 10px, letter-spacing .14em) sits bottom-left.
- turtle(x,y,ang,len,gen) walks the fern recursively: at each node it stores one segment, then branches left (ang-ANG, len*0.74), trunk up (ang, len*0.83), branch right (ang+ANG, len*0.74), trunk up again (ang, len*0.62). DEPTH=5, ANG=23deg. Segments are pre-computed once into typed Float32Array buffers (segX0/segY0/segX1/segY1, segGen) capped at 9000.
- build() normalises the stored steps so the fern always fills the same viewport fraction: it finds the bounding box, scales by 0.98*min(W/rangeX, H/rangeY), then centres. GROW_PER_SEC=SEGN/3.4 after build.
- drawTo(n) strokes the first n segments with lineCap round, colour PAL[min(7,segGen[i])], lineWidth 2.2 for gen<3 else 1.1, plus a soft gold radial-gradient tip dot at the newest segment end.
- frame(t) runs a grow/hold loop: in 'grow' it draws up to floor(phaseT*GROW_PER_SEC) segments, then holds 2.2s (holdUntil=t+2200) and restarts. phaseT advances 1/60 per frame.

SCRIPT (one IIFE, no libraries)
- syncTheme() re-reads --fr05-p0..p7 from computed style into PAL, so the canvas follows the CSS theme. resize() sizes the canvas with DPR capped at 2 and repaints when ready.
- start/stop via requestAnimationFrame; visibilitychange restarts the grow phase when the tab returns. With prefers-reduced-motion, init() draws the full fern once (drawTo(SEGN)) and never starts the loop; the stage background gradient is also removed under reduced motion.
- Demo lifecycle: window.__foreverPreview gates the loop (demoEnabled), and a forever:interactive event, pointerdown, click, or keydown takes over (stops the attract loop, paints the current frame).

ACCESSIBILITY AND MOTION
- The stage is role="img" with an aria-label; the caption is decorative text. Reduced motion shows one complete static fern with no animation.

WHAT BREAKS
- DEPTH=5 is near the 9000-segment cap; depth 6+ silently truncates at the cap, so the fern stops growing new branches.
- The draw cycle takes 3.4s, so content appears progressively — do not use where an instant full visual is needed.
- Changing the palette means updating both the :root tokens and the PAL defaults; syncTheme() re-reads the tokens, so the tokens are the source of truth.
- The background colour lives in --fr05-bg; update the token (and the light block) to retheme.
