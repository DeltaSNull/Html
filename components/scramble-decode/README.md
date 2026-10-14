# Scramble Decode

Gold monospace text resolves from random glyphs left-to-right with a tracking caret, holds with micro-flickers, then re-scrambles through a 6-phrase loop. Canvas 2D, zero dependencies.

![Scramble Decode](preview.jpg)

**Open it:** [`index.html`](index.html) runs straight from disk — one file, every asset inlined, no build step.

**Build it yourself:** [`PROMPT.md`](PROMPT.md) is the one-shot brief this file was built from. Paste it into any AI builder or coding agent.

| | |
| --- | --- |
| Best for | terminal-style hero text reveals, loading screens, hacker-aesthetic landing sections |
| Style | dark, typography, glitch, canvas, loop, monospace |
| Sections | stage |
| Libraries | none — plain HTML, CSS and canvas JS |
| Fonts | ui-monospace (system) |
| Uses | canvas-2d, reduced-motion, light-ready |
| Size | ~12 KB · brief ~800 tokens |

The demo region is tagged `data-section`, and the file opens with a header comment that maps its design tokens, sections and scripts, and lists what breaks when you edit it. Read that first. The `<script type="application/json" id="forever-agent-meta">` block in `<head>` carries the component's agent-ready metadata: editable areas, elements to preserve, and known limitations. Append `?theme=light` to preview the light re-theme.

## Built on

- [Scramble Decode — Forever Components](https://forevercomponents.com/components/text-effects/scramble-decode/) — the component this demo is adapted from

Keep this attribution if you redistribute the component; the original's own terms apply to the parts taken from it.
