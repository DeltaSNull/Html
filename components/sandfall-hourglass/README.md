# Sandfall Hourglass

Amber cellular-automaton sand grains topple through an hourglass cavity, piling realistically at the base; the frame auto-flips when the bulb drains, and a canvas click reverses gravity. Zero dependencies, light-theme re-theme included.

![Sandfall Hourglass](preview.jpg)

**Open it:** [`index.html`](index.html) runs straight from disk — one file, every asset inlined, no build step.

**Build it yourself:** [`PROMPT.md`](PROMPT.md) is the one-shot brief this file was built from. Paste it into any AI builder or coding agent.

| | |
| --- | --- |
| Best for | meditative loading screens, time-based visualizations, physics playgrounds, ambient background art |
| Style | dark, canvas, physics, cellular-automaton, particles, generative, ambient |
| Sections | stage |
| Libraries | none — plain HTML, CSS and a tiny canvas-2D simulation script |
| Fonts | system font stack (caption only) |
| Uses | canvas-render, click-reverse-gravity, theme-switch, reduced-motion, mobile-ready |
| Size | ~13 KB · brief ~600 tokens |

The demo region is tagged `data-section`, and the file opens with a header comment that maps its design tokens, sections and scripts, and lists what breaks when you edit it. Read that first. The `<script type="application/json" id="forever-agent-meta">` block in `<head>` carries the component's agent-ready metadata: editable areas, elements to preserve, and known limitations.

## Built on

- [Sandfall Hourglass — Forever Components](https://forevercomponents.com/components/particles-physics/sandfall-hourglass/) — the component this demo is adapted from

Keep this attribution if you redistribute the component; the original's own terms apply to the parts taken from it.
