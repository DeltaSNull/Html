Build an "Orbiting Planet System" ambient space component as a single HTML file: four glowing planets orbit a radiant sun on tilted elliptical rings with depth-sorted rendering, drifting subtly toward the pointer. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- Hardcoded dark navy ground #070a18 (CSS html,body background). Orbit rings: rgba(150,170,255,0.13). Sun glow: #fff6d8 / #ffd36b / #ffe9a8. Planets: #9ad0ff, #ffd9a0, #ff9b76, #b8a0ff. Caption: #cfd6ff at opacity .32, system-ui, 10px, uppercase.

THE SYSTEM
- A full-viewport canvas #c (tagged data-section="stage"), display:block, width/height 100%.
- size(): DPR = min(devicePixelRatio, 2); W/H from clientWidth/clientHeight; ctx.setTransform(DPR,0,0,DPR,0,0); cx=W/2, cy=H/2; scale = min(W,H)/420. Re-run on resize.
- Planets: [{r:60,s:1.0,sz:3.2,c:'#9ad0ff'},{r:95,s:0.62,sz:5,c:'#ffd9a0'},{r:140,s:0.4,sz:4,c:'#ff9b76'},{r:185,s:0.27,sz:6.5,c:'#b8a0ff'}], each given a random starting angle a.
- Halo: per planet, pre-render one 64x64 canvas radial gradient (planet color to transparent, alpha .55) once; draw it per frame with drawImage — never per-frame shadowBlur.
- frame(): t += 0.012 (frozen at 0 under reduced-motion); clearRect; tilt=0.45; pointer drift ox=mx*14, oy=my*14 from pointermove over the canvas (reset to 0 on pointerleave); translate(cx+ox, cy+oy).
- Orbit rings: ctx.ellipse(0, 0, r*scale, r*scale*tilt, 0, 0, 2*PI) stroked in rgba(150,170,255,0.13).
- Sun: radial gradient radius 26*scale (#fff6d8 to #ffd36b to transparent) plus a solid core radius 8*scale #ffe9a8.
- Planets depth-sorted: x=cos(a)*r*scale, y=sin(a)*r*scale*tilt, z=sin(a); sort by z ascending so far planets render behind; planet size scales 0.7+0.3*((z+1)/2); halo drawn at sz+8.
- A small uppercase caption ("Orbiting Planet System", 10px, letter-spacing .08em, opacity .32, pointer-events:none) sits bottom-left.

SCRIPT (one IIFE, no libraries)
- rAF loop with start()/stop(); visibilitychange pauses when hidden and resumes when visible.
- prefers-reduced-motion: call frame() once (t frozen) and never start the loop.

ACCESSIBILITY AND MOTION
- matchMedia('(prefers-reduced-motion: reduce)'): a single static frame; the planets appear frozen.
- The canvas is pure decoration; the caption is plain text.

WHAT BREAKS
- Replacing the pre-rendered halo canvases with per-frame shadowBlur tanks performance.
- Dropping the tilt (circular orbits) or the z-sort kills the perspective depth.
- Light themes need the hardcoded #070a18 ground repointed; the glow colors assume a dark field.
