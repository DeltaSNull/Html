# Weight Morph Wave

Each letter of a word pulses font-weight and vertical scale in a staggered sine wave, shifting hue from cyan to blue. CSS + one rAF script, zero dependencies.

![Weight Morph Wave](preview.jpg)

**Open it:** [`index.html`](index.html) runs straight from disk — one file, every asset inlined, no build step.

**Build it yourself:** [`PROMPT.md`](PROMPT.md) is the one-shot brief this file was built from. Paste it into any AI builder or coding agent.

| | |
| --- | --- |
| Best for | hero text, brand name displays, ambient kinetic typography |
| Style | dark, typography, kinetic-type, sine-wave, ambient |
| Sections | stage |
| Libraries | none — CSS animation via rAF + one script, zero dependencies |
| Fonts | Helvetica Neue / Arial (system stack) |
| Uses | kinetic-type, sine-wave, reflow-safe, reduced-motion |
| Size | ~7 KB · brief ~1.1 KB |

Designed for short words (≤8 characters) on viewports ≥280px — not for body copy.

The demo region is tagged `data-section`, and the file opens with a header comment that maps its design tokens, sections and scripts, and lists what breaks when you edit it. Read that first. The `<script type="application/json" id="forever-agent-meta">` block in `<head>` carries the component's agent-ready metadata: editable areas, elements to preserve, and known limitations.

## Built on

- [Weight Morph Wave — Forever Components](https://forevercomponents.com/components/kinetic-type/weight-morph-wave/) — the component this demo is adapted from

Keep this attribution if you redistribute the component; the original's own terms apply to the parts taken from it.
