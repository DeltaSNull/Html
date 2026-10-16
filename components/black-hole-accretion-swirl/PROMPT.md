Build a "Black-hole Accretion Swirl" cosmic canvas component as a single HTML file: 110 heat-colored particles spiral inward across a tilted accretion disk toward a glowing black event horizon; pointer movement boosts the swirl speed. Zero dependencies, canvas 2D, one inline script, dark only.

THE SCENE
- Full-viewport canvas #c02-canvas (display:block, width/height 100%). Hardcoded near-black ground (#05040a in CSS and the per-frame trail wash rgba(5,4,10,0.22)) — this component is dark-only, no light theme.
- DPR-aware sizing (DPR capped at 2, setTransform), center (cx,cy) and scale=Math.min(W,H)/400 recomputed on resize so the disk stays proportional.
- Disk transform per frame: translate(cx,cy), scale(1,0.42) for the tilt, rotate(0.35). All particles are drawn inside this transform; the event horizon is drawn after restore, centered at (cx,cy).

THE SIMULATION
- N=110 particles, each: angle a random 0–2π, radius r = 40+Math.random()*150, speed sp = 0.4+Math.random()*0.5.
- Per frame: paint the trail wash over the whole canvas (this is the motion trail — no clearRect). For each particle: dt = (0.6+boost) * sp * (60/max(r,30)) * 0.55 — inner particles orbit faster (Kepler-ish). a += dt*0.04; r -= 0.18*(1+boost) (inward drift). If r<26, respawn at r=180+Math.random()*30 with a random angle (this can cause a brief visual pop at the rim — accepted).
- Heat = 1-(r-26)/160: color is '#fff1c4' (heat>0.6) → '#ffae4d' (heat>0.35) → '#ff5e2e'; alpha = 0.5+heat*0.5; size = (1.1+heat*1.8)*scale.
- Event horizon: radial gradient (0 → black, .72 → black, .9 → rgba(255,140,40,0.55), 1 → rgba(255,90,30,0)) filled over a 30*scale circle, then a solid #000 21*scale core.
- Pointer boost: pointermove on the canvas sets boost=1; it decays per frame at boost*=0.94, multiplying both orbital speed and inward drift. (Note: any pointer movement over the canvas triggers it — position isn't used.)
- A small uppercase caption ("Black-hole Accretion Swirl", 10px system-ui, letter-spacing .08em, color #ffd9b0, opacity .34, pointer-events:none, user-select:none) sits bottom-left.

LIFECYCLE AND MOTION
- visibilitychange pauses/resumes the rAF loop. prefers-reduced-motion freezes the animation: one static frame is drawn after painting the ground (t never advances, dt=0, so the loop draws the identical frame).

WHAT BREAKS
- Changing the background without also changing the trail wash (rgba(5,4,10,0.22)) leaves mismatched smearing; both are hardcoded, so re-theme both.
- Removing scale=Math.min(W,H)/400 (or drawing the horizon outside the restore at the wrong center) breaks the disk/horizon alignment on non-square viewports.
- The disk tilt is scale(1,0.42) + rotate(0.35) applied together — changing one without the other tilts the wrong axis.
