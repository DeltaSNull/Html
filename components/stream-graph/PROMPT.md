Build a "Streamgraph" charts-flow component as a single HTML file: six translucent stacked bands breathing around a central baseline via layered sines, with a sweeping readout line tracking across and coloured tick dots per band. Zero dependencies, one canvas, one IIFE.

PALETTE AND TOKENS
- :root tokens: --ch05-bg:#0a0d12 (dark slate ground), --ch05-accent:#e2e8f0, --ch05-sep:rgba(10,13,18,0.85) (band separators), --ch05-sweep:rgba(226,232,240,0.92) (readout line), --ch05-dim:rgba(226,232,240,0.28) (tick rings), --ch05-cap:#fff / --ch05-cap-o:.42 (caption).
- A [data-theme="light"] block repoints them (--ch05-bg:#f7f9fc, --ch05-accent:#0f172a, separators/sweep/dim in dark ink). A theme-hook script in <head> reads ?theme=light|dark from the URL and stamps data-theme on <html> before first paint, so there is no dark flash. Standalone default stays dark. A {type:'forever:theme'} postMessage switches theme at runtime.

THE CHART
- A full-viewport canvas #ch05-c (position:absolute, inset:0, width/height 100%) inside #ch05-stage (fixed inset:0, overflow:hidden); a small uppercase monospace caption ("Streamgraph", 10px, letter-spacing .08em) sits fixed bottom-left.
- N=6 bands in COL=['#60a5fa','#34d399','#fbbf24','#f472b6','#a78bfa','#22d3ee'], M=46 columns, breathing rates FRQ=[0.25,0.34,0.28,0.4,0.3,0.38]. compute(t) fills a preallocated Float32Array data[N*M] with y = 0.5 + 0.25*sin(x*6.2831*FRQ[j] + t*0.09) + 0.08*sin(x*12.5663*0.4 + j + t*0.12).
- draw(t) plots into gw=W*0.9, gh=H*0.8 at gx=W*0.05, gy=H*0.1. Each column is totalled first, then bands stack cumulatively around the centre: top = 0.5 + (a - tot/2)*0.25, bot = 0.5 + (a + data - tot/2)*0.25, so the six bands span ~75% of the plot height around the baseline. Each band is filled with COL[j]+'4d' (30% alpha) and stroked with SEP.
- The sweep line runs at sx = gx + ((t*0.13)%1)*gw with a coloured dot per band at the band's midpoint in that column; it is disabled under prefers-reduced-motion.

SCRIPT (one IIFE, no libraries)
- stackTop/stackBot/data are preallocated Float32Arrays — no per-frame allocation. syncTheme() reads the --ch05-* tokens from computed style into BG / SEP / SWEEP / DIM, so the canvas follows the CSS theme.
- size() sizes the canvas to the stage; a debounced (120ms) resize listener re-runs it and re-renders under reduced motion. frame() advances tt by dt/1000 and draws at t = tt + 0.75.
- start/stop via requestAnimationFrame; visibilitychange pauses when the tab is hidden (skipped under reduced motion). With prefers-reduced-motion, init() computes and draws one static frame at t=0.75 and never starts the loop.

ACCESSIBILITY AND MOTION
- The chart is role="img" with an aria-label describing the bands and sweep; the caption is pointer-events:none. Reduced motion shows one static frame with no sweep line.

WHAT BREAKS
- The data is synthetic sine waves only — there is no real data binding and no axis labels; do not present it as a real chart.
- The stacking loop must total each column before stacking (tot/2 centre, 0.25 scale) — the copied code centred on M*0.5 with an undefined h2 and threw on the first frame.
- Changing N without extending COL and FRQ leaves bands uncoloured or unbreathing; changing M without the preallocated buffers breaks the column math.
- The background colour lives in --ch05-bg; update the token (and the light block) to retheme.
