Build a "Glitch Split" kinetic-type component as a single HTML file: a bold word rendered on a full-viewport canvas with a red/cyan RGB channel-split, idle shimmer, and periodic bursts of horizontal slice displacement and scanline glints over a near-black background. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- Hardcoded colors: ground #0a0b10, red channel #ff2d4d, cyan channel #2dd6ff, anchor white #f3f6ff, accent gold #f5d76e (caption + baseline tick).
- Font: '800 <px>px "Arial Black",Impact,sans-serif'; size scales as min(20% width, 30% height, 140px), floored at 22px.

THE GLITCH
- Full-viewport canvas `#tte06-c` (role="img" with an aria-label); html/body overflow:hidden prevents horizontal scroll — keep it.
- Every frame the word is stamped three times with globalCompositeOperation='lighter': red shifted left by the channel offset, cyan shifted right, white on top as the legibility anchor.
- Idle state: the channel offset never fully settles — a two-rate sine breath (periods 760ms and 1730ms around 1.4px) drifts the split through registration and back out.
- Bursts: the first lands ~340ms after mount, then every ~2.1s (nextBurst = 1100 + random*1200). During a burst the offset grows by e*(7+sin(t/40)*4) and 2..6 random horizontal slices are re-stamped sideways (displacement = dx*e*26px); intensity e decays over ~300ms with an ease-out curve.
- Scanline glints: up to 5 one-pixel gold lines at intensity e*0.5 during bursts.
- Idle CRT scan band: a soft 1.5px gold band sweeps the word every ~2.1s so the quiet is never a still.
- A gold accent baseline tick underlines the word; a small uppercase gold caption ("Glitch Split", 10px ui-monospace, letter-spacing .08em, opacity .45) sits bottom-left, pointer-events:none.

SCRIPT (one IIFE, no libraries)
- DPR-aware sizing on mount and resize (capped at DPR 2).
- On visibilitychange, the loop pauses; on resize with reduced-motion, the static frame is redrawn.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce) (checked in JS): renders one static frame — a clean, legible word with no offset, no slices, no loop.

WHAT BREAKS
- Removing globalCompositeOperation='lighter' kills the additive RGB glow; the channels turn muddy.
- Light backgrounds need the ground color changed in both the CSS and the JS fillStyle — it is hardcoded #0a0b10 in two places.
- The word may clip on very narrow viewports (<200px); keep the 22px floor.
