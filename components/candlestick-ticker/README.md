# Candlestick Ticker

22 green/red candlesticks scroll left continuously on procedurally generated OHLC data, with a dashed last-price line and a live price label. Pure canvas 2D, zero dependencies.

![Candlestick Ticker](preview.jpg)

**Open it:** [`index.html`](index.html) runs straight from disk — one file, every asset inlined, no build step.

**Build it yourself:** [`PROMPT.md`](PROMPT.md) is the one-shot brief this file was built from. Paste it into any AI builder or coding agent.

| | |
| --- | --- |
| Best for | finance dashboards, trading UI mockups, ambient data-viz backgrounds for fintech |
| Style | dark, canvas, data-viz, finance, scrolling |
| Sections | stage |
| Libraries | none — plain HTML, canvas 2D and a tiny JS data/scroll loop |
| Fonts | system monospace |
| Uses | canvas-2d, raf-loop, reduced-motion |
| Size | ~9 KB · brief ~600 tokens |

The demo region is tagged `data-section`, and the file opens with a header comment that maps its design tokens, sections and scripts, and lists what breaks when you edit it. Read that first. The `<script type="application/json" id="forever-agent-meta">` block in `<head>` carries the component's agent-ready metadata: editable areas, elements to preserve, and known limitations.

Prices here are procedurally generated — never present this as market data.

## Built on

- [Candlestick Ticker — Forever Components](https://forevercomponents.com/components/micro-dataviz/candlestick-ticker/) — the component this demo is adapted from

Keep this attribution if you redistribute the component; the original's own terms apply to the parts taken from it.
