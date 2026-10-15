Build a "Candlestick Ticker" micro-dataviz component as a single HTML file: 22 green/red candlesticks scroll left continuously on procedurally generated OHLC data, with a dashed last-price line and a live price label.

PALETTE AND TOKENS
- UP='#22c55e' / DN='#ef4444' (up/down candle colors); ground #0a0f0d
  hardcoded in CSS; grid lines rgba(159,184,173,0.08); ui-monospace type for
  the label and chips.

THE CHART
- A full-viewport fixed .mdv03-stage holds canvas #mdv03-c filling it.
  DPR-aware sizing via setTransform, dpr capped at 2.
- N=22 candles; the data array holds {o,h,l,c}. init() seeds it with a random
  walk (drift ±7, wicks extending 0–4); pushCandle() appends a candle and
  shifts the oldest off.
- Layout: padL=W*0.08, padR=W*0.14, padT=H*0.12, padB=H*0.18; candle width
  cw=iw/N; body width bw=max(2,cw*0.55); wick lineWidth=max(1,cw*0.12) with
  shadowBlur 6 on the wicks.
- The price scale is normalized 0–100 and mapped to y; five grid lines.
- A dashed last-price line (setLineDash([5,4])) spans the plot; a live label
  chip shows '₿ ' + (42000+last*310); an 'MDV·OHLC' prefix chip sits top-left
  of the plot.

SCRIPT (one IIFE, no libraries)
- rAF loop: acc+=dt; every 1100ms a new candle is pushed; between pushes the
  whole field shifts by (acc/1100) * one candle slot for smooth scrolling.
- pointerdown resets the entire chart via init() — deliberate, not a bug.
- visibilitychange pause guard; pagehide stop.
- prefers-reduced-motion: freeze the scroll (the shift=0 branch) — the chart
  still draws, but nothing moves.
- demo-v1 preview lifecycle: demoEnabled from window.__foreverPreview;
  forever:interactive / keydown stop the attract loop and never resume it.

ACCESSIBILITY AND MOTION
- prefers-reduced-motion freezes the scrolling; the canvas has role="img"
  with an aria-label.

WHAT BREAKS
- Prices are fake (0–100 normalized, random walk). Never present this as
  market data.
- Removing the setTransform breaks hidpi sharpness.
- pointerdown → init() is the intended reset gesture, not a pause.
- The #0a0f0d ground is hardcoded; repoint the body background for light themes.
