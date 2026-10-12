Build a "Vapor Condense" kinetic-typography component as a single HTML file: 300 teal particles drift as a glowing haze, then condense into the crisp word VAPOR via cosine easing, hold, and dissolve back — looping continuously. Zero dependencies, canvas 2D, one inline script. Click toggles between haze and condensed word.

TOKENS AND THEME
- :root tokens (dark defaults; a [data-theme="light"] block re-themes): --vc-bg / --vc-bg-rgb (field), --vc-blend ('lighter' additive on dark, 'source-over' on light), --vc-vapor-rgb:80,170,180 / --kt19-core-rgb:168,230,220 (particle color endpoints, read by JS via syncTheme()), --vc-accent (caption), --vc-alpha-boost, --cap-o.
- A theme hook runs before first paint: ?theme=light|dark in the query string (or data-theme on <html>); a {type:'forever:theme'} postMessage switches at runtime.

WORD SAMPLING
- buildAnchors(): draw WORD ("VAPOR") at 800 weight, 42% of min(viewport) in ui-monospace on an offscreen canvas, scan pixels with alpha>128 on a step grid, then pick N=300 evenly spaced anchor points. Run only after positive element layout; repaint on ResizeObserver; zero-size mounts sleep.
- Font size scales with min(W,H)/400 (the `scale` factor); particle radii and drift multiply by it so the effect holds at any viewport.

THE PARTICLE LOOP
- Each particle has a random start, a phase, and a speed. Cycle t advances 0.0052/frame in demo mode; phase cond = 0.5-0.5*cos(clamped cycle) gives smooth gather/scatter, sharp = cond² lags for the crispness.
- Per frame: paint bg wash rgba(bgRgb, 0.28) (motion trails), set globalCompositeOperation to the blend token, then for each particle compute a haze target (anchor + sinusoidal wander × drift, drift=18*scale*(1-cond)+3*scale) and a dispersed cloud offset; ease position toward cond*anchor + (1-cond)*cloud with ease 0.045+0.08*cond (+nudge boost).
- Radius: (1.05 + (1-sharp)*3.7 + nudge*1.5) * scale — big and blurred when vapor, tight when sharp. Alpha: (0.05 + sharp*0.5) * wobble + alphaBoost. Color lerps from vapor rgb to core rgb by sharp.
- Loop sleeps when nothing moves (no rAF churn at rest). pointermove sets nudge=1 and restarts; click toggles t between haze/condensed and calls takeover() to end demo mode.

LIFECYCLE AND MOTION
- visibilitychange pauses/starts. prefers-reduced-motion: paintReduced() draws one static fully-condensed word; click still snaps particles to anchors.
- demo-v1 preview lifecycle: demo reads window.__foreverPreview; pointerdown/keydown/forever:interactive ends the attract loop.
- Canvas is keyboard-focusable (tabindex=0, role=button, aria-label="Activate kinetic text." — update the label if you change WORD), Enter/Space activates, :focus-visible outline. Caption bottom-left, pointer-events:none.

WHAT BREAKS
- Long words overflow the canvas — the word mask is sized for short words in the local monospace font.
- Removing the ResizeObserver repaint leaves anchors sampled at the old size after rotation/resize.
- Changing particle color must go through the r/g/b token formula in the frame loop, not just CSS, because the canvas paints its own pixels.
