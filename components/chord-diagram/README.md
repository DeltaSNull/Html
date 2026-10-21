# Chord Diagram

Eight ring nodes with curved ribbons that swell and shrink as weights rebalance every ~3s. Additive blending creates glowing color overlaps. Canvas-drawn, zero dependencies.

![Chord Diagram](preview.jpg)

**Open it:** [`index.html`](index.html) runs straight from disk — one file, every asset inlined, no build step.

**Build it yourself:** [`PROMPT.md`](PROMPT.md) is the one-shot brief this file was built from. Paste it into any AI builder or coding agent.

| | |
| --- | --- |
| Best for | ambient data-viz backgrounds, network relationship storytelling, dashboards needing a living connections motif |
| Style | dark, canvas, data-viz, additive-blend, generative, ambient |
| Sections | stage |
| Libraries | none — canvas + one script, zero dependencies |
| Fonts | ui-monospace (caption only) |
| Uses | canvas-animation, seeded-rng, theme-tokens, reduced-motion |
| Size | ~11 KB · brief ~1.3 KB |

The demo region is tagged `data-section`, and the file opens with a header comment that maps its design tokens, sections and scripts, and lists what breaks when you edit it. Read that first. The `<script type="application/json" id="forever-agent-meta">` block in `<head>` carries the component's agent-ready metadata: editable areas, elements to preserve, and known limitations.

Weights are synthetic and auto-cycling — not a real chart until wired to actual data.

## Built on

- [Chord Diagram — Forever Components](https://forevercomponents.com/components/charts-flow/chord-rebalance/) — the component this demo is adapted from

Keep this attribution if you redistribute the component; the original's own terms apply to the parts taken from it.
