Build a "Chord Diagram" ambient data-viz component as a single HTML file: eight ring nodes whose curved ribbons swell and shrink as relationship weights rebalance every ~3s, additive blending creating glowing colour overlaps. Canvas-drawn, zero dependencies, one script.

PALETTE AND TOKENS
- :root --ch02-* tokens (dark defaults): --ch02-bg:#0a0d12, --ch02-cap:#fff, --ch02-blend:lighter (additive glow), --ch02-rib-a:.5, five hues --ch02-c0:#5ad1a0, c1:#4aa3d6, c2:#e6b450, c3:#d96b6b, c4:#9b7fd4.
- :root[data-theme="light"]: pale bg (#f1f4f8), --ch02-blend:source-over (normal blending), darker hues, --ch02-rib-a:.44. ?theme=light|dark query param or {type:'forever:theme'} postMessage switches at runtime; JS reads the tokens in syncTheme() so CSS stays the single source of colour truth.
- 8 arcs cycle the 5 hues (PAL[i] = base[i%5]).

THE STAGE
- Fixed full-viewport #ch02-stage (role=img with an aria-label describing the diagram) holds a canvas filling it; a small uppercase mono caption ("Chord Diagram") sits bottom-left.
- Ring radii R=m*0.40, RI=m*0.34 (m = min(W,H)); node arcs stroked with lineWidth max(4, m*0.022); angular gap GAP=0.045.

SCRIPT (one IIFE, no libraries)
- Synthetic symmetric N×N weight matrix (no self-links), seeded LCG RNG (s=20240626) for the initial state; each frame cur eases toward tgt with lerp 0.018; every t>3.2s a new symmetric target is re-seeded (weights pow(rng(),2.1), boosted when rng()<0.4).
- layout(): each node's arc span is proportional to its total weight; endAngle(): each partner's ribbon endpoint is packed within its node arc by relative weight.
- ribbon(a,b,c,d): arc at RI from a→b, quadratic curve to endpoint (c) through the centre, arc c→d, curve back through centre — filled with per-pair alpha min(RIBA, 0.10+w*0.9), colour PAL[(i+j)%len].
- DPR-aware canvas sizing (DPR clamped to 2), resize listener.
- visibilitychange pause/resume; reduced-motion draws one settled static frame (drawStatic) and returns.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce): collapse CSS animations; script draws a single static frame instead of looping.
- The diagram is decorative (role=img, descriptive aria-label); there are no labels or tooltips.

WHAT BREAKS
- Data is synthetic — to display real data, replace the seed/rng matrix logic and the t>3.2 re-seed with actual values.
- Removing the globalCompositeOperation=BLEND block kills the glowing overlaps (light theme intentionally uses source-over).
- Changing N requires trimming PAL to match and re-checking GAP so arcs don't overlap.
- Light theme needs --ch02-blend:source-over; additive blending on the pale bg washes out.
