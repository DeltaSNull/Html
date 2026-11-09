# Bubble Pack

64 mint/teal/amber/coral circles settle under gravity with collision physics; size encodes value, large bubbles pull toward the centre with a breathing pulse, and hovering a bubble shows its value. Theme-aware (dark default, light via `?theme=light`).

![Bubble Pack](preview.jpg)

**Open it:** [`index.html`](index.html) runs straight from disk — one file, every asset inlined, no build step.

**Build it yourself:** [`PROMPT.md`](PROMPT.md) is the one-shot brief this file was built from. Paste it into any AI builder or coding agent.

| | |
| --- | --- |
| Best for | dashboard highlights, portfolio or market overviews, relative-value summaries |
| Style | dark, data-viz, canvas, ambient |
| Sections | stage |
| Libraries | none — plain HTML, CSS and a small canvas script |
| Fonts | system-ui (HUD and tooltips) |
| Uses | canvas, tooltip, gravity-physics, theme-aware, reduced-motion |
| Size | ~13 KB · brief ~950 tokens |

The demo region is tagged `data-section`, and the file opens with a header comment that maps its design tokens, sections and scripts, and lists what breaks when you edit it. Read that first. The `<script type="application/json" id="forever-agent-meta">` block in `<head>` carries the component's agent-ready metadata: editable areas, elements to preserve, and known limitations.

## Built on

- [Bubble Pack — Forever Components](https://forevercomponents.com/components/charts-flow/bubble-pack/) — the component this demo is adapted from

Keep this attribution if you redistribute the component; the original's own terms apply to the parts taken from it.
