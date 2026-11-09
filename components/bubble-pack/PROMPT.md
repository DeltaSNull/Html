Build a "Bubble Pack" circle-packing bubble chart component as a single HTML file: 64 mint/teal/amber/coral circles settle under gentle gravity with collision physics; size encodes value, large bubbles pull toward the centre with a breathing pulse, and hovering a bubble shows a tooltip. Theme-aware (dark default, light via ?theme=light), one canvas, one script.

PALETTE AND TOKENS
- :root tokens: --bg:#0a0d12 (ground), --accent:#5ad1a0, --ch07-c0:#5ad1a0
  (Mint) / --ch07-c1:#2f9e7e (Teal) / --ch07-c2:#e7a13b (Amber) /
  --ch07-c3:#e06a5a (Coral) (category palette, re-read by JS in syncTheme()),
  --ch07-text:#f4f6f8, --ch07-hud:#9aa7b4, --ch07-panel:rgba(13,17,23,0.72)
  (tooltip). The :root[data-theme="light"] block re-points every token
  (--bg:#f1f4f8, --accent:#237e59, --ch07-c0:#2c9e6f, etc.).
- HUD: 11px system-ui, top-left "Total value" + total + "N bubbles", top-right
  legend with 9px swatches; fades in via .hud.on. Tooltip: fixed panel,
  11px, border-radius 8px, backdrop blur, with a value bar. Caption: 10px
  uppercase monospace, letter-spacing .08em, opacity .42, fixed bottom-left.

THE BUBBLES
- A full-viewport #ch07-stage wrapper holds the canvas (#ch07-c), the HUD,
  the tooltip and the caption; html,body overflow:hidden.
- seed(): N=64 bubbles, categories cycling Mint/Teal/Amber/Coral; value v =
  18 + random^2.2 * 182; radius r = max(1.2, sqrt(v) * maxR / 13.5) with maxR =
  min(W,H)*0.085; spawned near the centre with a random phase. Bubbles are
  sorted largest-first; the HUD total and legend are rendered here.
- step(): each bubble is pulled toward the centre — dx = d.x - cx, dy = d.y -
  cy, scaled by a breathing pulse s = 1 + 0.5*sin(t*1.4 + phase), clamped to
  ±200, then vx += (rx*0.02 - vx)*0.12 (gravity strength 0.02). Pairwise
  collision resolution pushes overlapping bubbles apart by half the overlap
  plus pad=3. Positions are clamped inside the viewport.
- draw(): faint crosshair grid every 48px, a radial vignette from --ch07-vig0
  to --ch07-vig1, then each bubble as a filled circle with a white specular
  highlight (offset up-left, 22% alpha), a 10% white stroke, and its category
  name in white when r > 22.
- Hover (pointermove) finds the topmost bubble under the cursor and shows the
  tooltip (category, value, % of max, value bar), positioned to stay on-screen;
  click re-shows it; pointerleave hides it. Touch works via the same pointer
  events.

SCRIPT (one IIFE, no libraries)
- A head theme-hook IIFE reads ?theme=light|dark from the URL and stamps
  data-theme on <html> before first paint — no dark flash. Standalone default
  stays dark. syncTheme() re-reads --ch07-c0..c3 from computed style into the
  CATS palette (each category reads its own token index).
- size() is DPR-aware (DPR capped at 2); resize re-seeds. start()/stop() wrap
  requestAnimationFrame; visibilitychange pauses when the tab is hidden.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce) is read once into `reduce`: a single
  settled frame is drawn (step + draw once) and no animation loop runs; the
  CSS also collapses all animation durations.
- Labels appear only on hover/tap — no persistent text, and no keyboard
  navigation (the tooltip is pointer-only). Do not use where exact values or
  quick comparisons matter.

WHAT BREAKS
- Removing the settle/collision logic leaves bubbles overlapping or drifting
  off-centre; the gravity pull, breathing pulse and collision resolution are
  one system.
- syncTheme() must read --ch07-c(index) per category — the token index and
  the CATS order are aligned; shifting one miscolours every category.
- Changing the accent means updating --accent AND --ch07-c0 together, then
  the CATS palette, or the legend swatches stop matching the bubbles.
- Bubble data is synthetic and random (seed() re-rolls on every resize) —
  rebind seed() to real values for production.
