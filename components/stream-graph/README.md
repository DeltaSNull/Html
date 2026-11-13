# Streamgraph

Six translucent stacked bands breathe around a central baseline via layered sines; a sweeping readout line tracks across with coloured tick dots per band. Canvas 2D, zero dependencies.

![Streamgraph](preview.jpg)

**Open it:** [`index.html`](index.html) runs straight from disk — one file, every asset inlined, no build step.

**Build it yourself:** [`PROMPT.md`](PROMPT.md) is the one-shot brief this file was built from. Paste it into any AI builder or coding agent.

| | |
| --- | --- |
| Best for | dashboard hero sections, data visualisation backgrounds, ambient charts illustrating trends without real data |
| Style | dark, data-viz, stacked-area, generative, flow, chart, canvas |
| Sections | stage |
| Libraries | none — plain HTML, CSS and canvas 2D |
| Fonts | system monospace (caption only) |
| Uses | canvas, streamgraph, data-viz, stacked-area, reduced-motion, light-ready |
| Size | ~10 KB · brief ~650 tokens |

The demo region is tagged `data-section`, and the file opens with a header comment that maps its design tokens, sections and scripts, and lists what breaks when you edit it. Read that first. The `<script type="application/json" id="forever-agent-meta">` block in `<head>` carries the component's agent-ready metadata: editable areas, elements to preserve, and known limitations. A light theme is available via `?theme=light` or a `{type:'forever:theme'}` postMessage. The band values are synthetic sine waves only — there is no real data binding.

## Built on

- [Streamgraph — Forever Components](https://forevercomponents.com/components/charts-flow/stream-graph/) — the component this demo is adapted from

Keep this attribution if you redistribute the component; the original's own terms apply to the parts taken from it.
