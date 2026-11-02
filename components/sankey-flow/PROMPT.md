Build a "Sankey Flow" animated data-visualization component as a single HTML file: a three-stage Sankey diagram on a dark canvas — bezier ribbons grow stage-by-stage from origins to hubs to outcomes, pulsing softly in opacity to show flow volume. Zero dependencies, canvas 2D, one small script, light-theme re-theme support.

PALETTE AND TOKENS
- :root tokens (dark defaults): --ch01-bg:#0a0d12, --ch01-glow (static inset glow), --ch01-cap, --ch01-c0..--ch01-c4 (#5ad1a0 teal, #3aa6c9 blue, #d8b24a gold, #d8623a rust, #8a7ad8 violet — the JS palette), --ch01-hub (hub bars), --ch01-rib-a:.14 / --ch01-rib-r:.30 (ribbon alpha). Every colour the canvas draws comes from a CSS token via syncTheme — change the tokens, not the literals.
- A :root[data-theme="light"] block re-points every token (light ground #f1f4f8, deepened palette, higher ribbon alpha) — this is the shipped light theme; preserve it.

THE DIAGRAM
- #ch01-wrap is a fixed full-viewport stage holding a full-bleed canvas with a static inset box-shadow glow (never repaints). role="img" with an aria-label describing the flow.
- Synthetic data: A=[[7,3,1],[2,6,3],[1,2,8]] (3 origins × 3 hubs), B=[[6,3,1,1],[2,5,2,2],[1,2,4,5]] (3 hubs × 4 outcomes). Column heights are proportional to row/column sums; origins take COL[i], hubs take HUB, outcomes cycle COL[(i+1)%5].
- Ribbon geometry: each ribbon stores its source/destination edges; the grow parameter (0..1) draws the right edge progressively along a bezier (mx midpoint control points). Pulse alpha = 0.16 + 0.12*sin(t*1.6 + phase); under reduced motion a flat RIBR alpha is used.
- Staged reveal: a 9s loop — stage 0 (origins→hubs) eases over 3.6s, stage 1 (hubs→outcomes) starts 2.6s in and eases over 3.6s, then the loop resets. Node bars (6px wide fillRect) draw on top at 0.92 alpha.
- A small uppercase monospace caption ("Sankey Flow", 10px, letter-spacing .08em, z-index 2) sits bottom-left.

SCRIPT (one IIFE, no libraries)
- resize(): DPR-aware canvas sizing (DPR capped at 2), then layout() rebuilds node columns and ribbon geometry — geometry caches recompute on every resize.
- syncTheme() reads the --ch01-* tokens and repaints the palette; a forever:theme postMessage listener and the ?theme= head hook drive live re-theme.
- loop()/start()/stop() drive requestAnimationFrame; visibilitychange stops/starts it; under reduced motion a single static draw(2600) frame renders instead.

ACCESSIBILITY AND MOTION
- The stage has role="img" with a full aria-label describing the data flow; it is canvas-drawn and not keyboard-interactive.
- @media (prefers-reduced-motion:reduce): one static full-growth frame, no loop.

WHAT BREAKS
- Editing colours only in the JS literals (COL/BG/HUB) while syncTheme runs will be overwritten — always change the --ch01-* tokens.
- The fill cursors (fillO/fillI) stack ribbon edges from the node tops; reordering ribbons without resetting the cursors overlaps them wrongly.
- The staged reveal has no pause control — it resets every 9s by design.
- The flow data is hardcoded synthetic constants — rebind A/B to real data for production use.
