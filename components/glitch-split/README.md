# Glitch Split

Bold word renders with red/cyan RGB channel-split; periodic bursts trigger horizontal slice displacement and scanline glints over a dark background. Canvas 2D, zero dependencies.

![Glitch Split](preview.jpg)

**Open it:** [`index.html`](index.html) runs straight from disk — one file, every asset inlined, no build step.

**Build it yourself:** [`PROMPT.md`](PROMPT.md) is the one-shot brief this file was built from. Paste it into any AI builder or coding agent.

| | |
| --- | --- |
| Best for | cyberpunk/hacker hero sections, error states, game UIs, dramatic dark-theme display |
| Style | dark, glitch, rgb-split, chromatic-aberration, scanlines, loop |
| Sections | stage |
| Libraries | none — plain HTML, CSS and a single canvas 2D script |
| Fonts | Arial Black / Impact (system stack) |
| Uses | canvas-2d, additive-blend, reduced-motion |
| Size | ~9 KB · brief ~600 tokens |

The demo region is tagged `data-section`, and the file opens with a header comment that maps its design tokens, sections and scripts, and lists what breaks when you edit it. Read that first. The `<script type="application/json" id="forever-agent-meta">` block in `<head>` carries the component's agent-ready metadata: editable areas, elements to preserve, and known limitations.

## Built on

- [Glitch Split — Forever Components](https://forevercomponents.com/components/text-effects/glitch-split/) — the component this demo is adapted from

Keep this attribution if you redistribute the component; the original's own terms apply to the parts taken from it.
