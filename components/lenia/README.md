# Lenia

A smooth-life cellular automaton: glowing teal-to-cyan blobs drift and pulse on a dark field via a typed-array convolution sim, rendered at 96x96 and scaled up with CSS. Canvas 2D, zero dependencies, light theme included.

![Lenia](preview.jpg)

**Open it:** [`index.html`](index.html) runs straight from disk — one file, every asset inlined, no build step.

**Build it yourself:** [`PROMPT.md`](PROMPT.md) is the one-shot brief this file was built from. Paste it into any AI builder or coding agent.

| | |
| --- | --- |
| Best for | ambient hero backgrounds, science and generative-art themes, dark-mode showcases |
| Style | dark, generative, organic, canvas, simulation, ambient |
| Sections | stage |
| Libraries | none — plain HTML, CSS and a canvas 2D script |
| Fonts | ui-monospace (system stack) |
| Uses | canvas-2d, cellular-automata, reduced-motion, theme-switch |
| Size | ~12 KB · brief ~600 tokens |

The demo region is tagged `data-section`, and the file opens with a header comment that maps its design tokens, sections and scripts, and lists what breaks when you edit it. Read that first. The `<script type="application/json" id="forever-agent-meta">` block in `<head>` carries the component's agent-ready metadata: editable areas, elements to preserve, and known limitations.

## Built on

- [Lenia — Forever Components](https://forevercomponents.com/components/fractals-automata/lenia-smooth/) — the component this demo is adapted from

Keep this attribution if you redistribute the component; the original's own terms apply to the parts taken from it.
