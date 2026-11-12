Build a "Phyllotaxis Sunflower" organic component as a single HTML file: 620 amber-gold seed dots grow outward in a Fermat spiral via the golden angle, rotating slowly from a glowing nucleus — a sunflower head as a warm, living hero background. Zero dependencies, one canvas, one script.

PALETTE AND TOKENS
- :root tokens: --phs-bg:#1a1206 (dark brown ground), --phs-cap:#ffc845 / --phs-cap-o:.34 (caption), --phs-g0:#241800 / --phs-g1:#1a1206 / --phs-g2:#120c04 (radial ground gradient), --phs-blend:lighter (additive glow stack), --phs-a:1 (seed opacity), --phs-nuc:rgba(255,200,69,.9) (nucleus glow), seed hsl ramp --phs-h0:44 / --phs-h1:9 / --phs-s0:96 / --phs-s1:1 / --phs-l0:50 / --phs-l1:11, crisp-core offsets --phs-sd:6 / --phs-ld:-6.
- The seed colour ramp is tokenised as hue = h0 + f*h1, sat = s0 + f*s1, lig = l0 + (1-f)*l1 where f = i/N — so a light theme can run a dark-on-cream ramp. A [data-theme="light"] block repoints everything (--phs-bg:#fbf7ef, --phs-blend:normal, --phs-a:.72, --phs-h0:44 / --phs-h1:-13, --phs-l0:32 / --phs-l1:18).
- A theme-hook script in <head> reads ?theme=light|dark from the URL and stamps data-theme on <html> before first paint, so there is no dark flash. Standalone default stays dark.

THE SPIRAL
- A full-viewport canvas #sunflower (position:fixed, inset:0) holds the whole component; a small uppercase caption ("Phyllotaxis Sunflower", 11px, letter-spacing .08em) sits fixed bottom-left.
- Constants: N=620 seeds, SPREAD=0.94, goldenAngle=137.508°, centre at (0.5, 0.5) of the viewport.
- Each seed i is placed by the Fermat formula: theta = (i*goldenAngle + rotation) % 360, r = R*sqrt(i/N) where R = min(W,H)/2 * SPREAD. The 137.508° step is what makes the spiral look natural — do not round it.
- Each seed is drawn twice: a soft glow disc (radius*2.8, 50% alpha) then the solid seed (radius = 1.2 + (R/N)*i*0.02 + sin(i*0.5)*0.15, pulsing by 1 + sin(now*0.8 + i*0.1)*0.05). The first 28 seeds (the core) are drawn in a fixed bright gold rgba(255,200,69,0.9). The nucleus is a glowing disc of radius R*0.04 at the centre.
- rotation advances by speed=0.0007 per frame, so the parastichy arms rotate continuously.

SCRIPT (one IIFE, no libraries)
- DPR-aware sizing: size() caps DPR at 2 and sets canvas width/height; resize listeners re-run it and re-render.
- seedColor(i) reads the --phs-* tokens from getComputedStyle each call, so theme changes take effect without reloading.
- start/stop via requestAnimationFrame; visibilitychange pauses when the tab is hidden. With prefers-reduced-motion, a single static render() is drawn and the loop never starts.

ACCESSIBILITY AND MOTION
- The canvas is pure decoration; the caption is pointer-events:none. Reduced motion shows one static spiral.

WHAT BREAKS
- Changing the golden angle or the r = R*sqrt(i/N) radius formula breaks the Fermat spiral — the seeds clump or ring instead of flowing.
- Seed count above ~900 strains low-end CPUs (620 seeds × 2 arcs × per-seed getComputedStyle each frame).
- The pointer speed-up the source page describes is not wired in the copied code: pointerX/pointerY/targetSpeed are declared but unused, and speed stays 0.0007. Hook pointermove up to targetSpeed yourself if you want it.
- The background is hardcoded dark brown in CSS; repoint --phs-bg and the --phs-g* gradient (and the light block) for other themes.
