# Game of Life

Conway's Game of Life with glowing magenta cells aging through three colour tiers; the field reseeds automatically when the population collapses. Canvas 2D, zero dependencies.

![Game of Life](preview.jpg)

**Open it:** [`index.html`](index.html) runs straight from disk — one file, every asset inlined, no build step.

**Build it yourself:** [`PROMPT.md`](PROMPT.md) is the one-shot brief this file was built from. Paste it into any AI builder or coding agent.

| | |
| --- | --- |
| Best for | ambient backgrounds for tech/generative-art themes, screensavers, loading screens needing emergent organic motion |
| Style | dark, cellular-automata, pixel, generative, loop, canvas |
| Sections | stage |
| Libraries | none — plain HTML, CSS and canvas 2D |
| Fonts | system monospace (caption only) |
| Uses | canvas, cellular-automata, pixel, grid, reduced-motion, light-ready |
| Size | ~10 KB · brief ~600 tokens |

The demo region is tagged `data-section`, and the file opens with a header comment that maps its design tokens, sections and scripts, and lists what breaks when you edit it. Read that first. The `<script type="application/json" id="forever-agent-meta">` block in `<head>` carries the component's agent-ready metadata: editable areas, elements to preserve, and known limitations. A light theme is available via `?theme=light` or a `{type:'forever:theme'}` postMessage.

## Built on

- [Game of Life — Forever Components](https://forevercomponents.com/components/fractals-automata/game-of-life/) — the component this demo is adapted from

Keep this attribution if you redistribute the component; the original's own terms apply to the parts taken from it.
