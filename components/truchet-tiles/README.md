# Truchet Tiles

An 8×8 grid of rainbow quarter-arc Truchet tiles flips orientation in slow sine waves, weaving shifting interlocked curves across a dark violet background. Canvas 2D, zero dependencies.

![Truchet Tiles](preview.jpg)

**Open it:** [`index.html`](index.html) runs straight from disk — one file, every asset inlined, no build step.

**Build it yourself:** [`PROMPT.md`](PROMPT.md) is the one-shot brief this file was built from. Paste it into any AI builder or coding agent.

| | |
| --- | --- |
| Best for | generative art backgrounds, creative portfolio heroes, math/design project visuals |
| Style | dark, geometric, generative, rainbow, hypnotic, canvas |
| Sections | stage |
| Libraries | none — plain HTML, CSS and canvas 2D |
| Fonts | system monospace (caption only) |
| Uses | canvas, truchet, sine-wave, pointer, reduced-motion |
| Size | ~7 KB · brief ~600 tokens |

The demo region is tagged `data-section`, and the file opens with a header comment that maps its design tokens, sections and scripts, and lists what breaks when you edit it. Read that first. The `<script type="application/json" id="forever-agent-meta">` block in `<head>` carries the component's agent-ready metadata: editable areas, elements to preserve, and known limitations. Note: the meta block and the source page's guidance describe an older variant of this file (identifiers `#truchet`, `drawTile`, `--gg03-arc-1`/`--gg03-arc-2`); the file itself uses `#c`, `drawArc`, `N=8` and `--gg03-arc-s`/`--gg03-arc-l` — trust the code.

## Built on

- [Truchet Tiles — Forever Components](https://forevercomponents.com/components/generative-geometry/truchet-tiles/) — the component this demo is adapted from

Keep this attribution if you redistribute the component; the original's own terms apply to the parts taken from it.
