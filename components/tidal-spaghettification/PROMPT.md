Build a "Tidal Spaghettification" ambient physics component as a single HTML file: 170 particles stream past an unseen gravity well, stretching into a white-hot luminous filament at perihelion; pointer movement and clicks boost the tidal spread. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- Hardcoded dark purple-black ground #0a0510 (CSS html,body background AND the canvas trail fill 'rgba(10,5,16,0.20)' — change both to retheme). Heat color ramp: #fff0e6 (white-hot) > #ffb3a0 > #ff6b6b > #8a3a6a (cool tail). Caption: #ff6b6b at opacity .36, system-ui, 10px, uppercase.

THE STREAM
- A full-viewport canvas #tsg-c (tagged data-section="stage"), display:block, width/height 100%.
- size(): DPR = min(devicePixelRatio, 2); W/H from clientWidth/clientHeight; ctx.setTransform(DPR,0,0,DPR,0,0); cx=W*0.5, cy=H*0.5; S = min(W,H)/400. Re-run on resize.
- N=170 particles, each with s in 0..1 (0 = far tail, 1 = leading head). The gravity well sits off to one side at gx=cx-90*S, gy=cy+18*S.
- path(u, spread): angle a = u*PI*1.55 - 0.35 along a hyperbolic-ish arc; perihelion radius peri=42*S; a gaussian dip exp(-((u-0.5)/0.16)^2) dips the radius at perihelion; spread blows the radius out near peri. Returns [x, y, dip].
- frame(): t += 0.0042 (frozen at 0 under reduced-motion); boost decays *= 0.95 per frame; paint the trail with ctx.fillStyle='rgba(10,5,16,0.20)'; then set globalCompositeOperation='lighter' for the whole particle pass.
- Each particle: u = (s*0.5 + flow*0.5 + 0.001*sin(t*9+i)) % 1 with flow=(t*0.5)%1; differential stretch spread=(0.4+boost)*dip*s; heat=min(1, dip*1.15 + spread*0.5); size=(0.7 + heat*2.6 + s*0.6)*S; color by heat thresholds (0.78 / 0.5 / 0.28) over the 4-step ramp; alpha 0.20+heat*0.7. Connect consecutive particles with a strand stroke (alpha 0.10+heat*0.30, width (0.5+heat*1.4)*S) to draw the continuous filament.
- pointermove: boost = min(boost+0.5, 1.4); click: boost = 1.6. Reset globalCompositeOperation to 'source-over' and globalAlpha to 1 after the pass.
- A small uppercase caption ("Tidal Spaghettification", 10px, letter-spacing .12em, opacity .36, pointer-events:none) sits bottom-left.

SCRIPT (one IIFE, no libraries)
- rAF loop with start()/stop(); visibilitychange stops when hidden and starts when visible.
- prefers-reduced-motion: paint one static frame (fill #0a0510, flow frozen at 0.5) and never start the loop.

ACCESSIBILITY AND MOTION
- Reduced motion shows a single frozen filament frame instead of looping — dramatic motion never auto-plays.
- The canvas is pure decoration; the caption is plain text.

WHAT BREAKS
- Forgetting to restore globalCompositeOperation to 'source-over' makes everything drawn later additively glow.
- Raising N much above 170 or the trail alpha makes the filament over-expose into a white blob.
- The well position is fixed relative to center — on very wide or narrow viewports the arc can clip, so keep overflow:hidden and re-test aspect extremes.
