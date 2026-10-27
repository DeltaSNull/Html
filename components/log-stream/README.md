# Log Stream

A simulated live server log tail — new entries slide in every 650ms with color-coded INFO/WARN/ERROR/DEBUG levels and real-time timestamps. Purely ambient, zero dependencies.

![Log Stream](preview.jpg)

**Open it:** [`index.html`](index.html) runs straight from disk — one file, every asset inlined, no build step.

**Build it yourself:** [`PROMPT.md`](PROMPT.md) is the one-shot brief this file was built from. Paste it into any AI builder or coding agent.

| | |
| --- | --- |
| Best for | developer dashboards, terminal-aesthetic hero sections, ops-themed loading screens, ambient backgrounds |
| Style | dark, terminal, monospace, ambient, typewriter |
| Sections | stage |
| Libraries | none — plain HTML, CSS and one inline script |
| Fonts | ui-monospace |
| Uses | ambient-animation, reduced-motion, mobile-ready |
| Size | ~6 KB · brief ~500 tokens |

The demo region is tagged `data-section`, and the file opens with a header comment that maps its design tokens, sections and scripts, and lists what breaks when you edit it. Read that first. The `<script type="application/json" id="forever-agent-meta">` block in `<head>` carries the component's agent-ready metadata: editable areas, elements to preserve, and known limitations.

## Built on

- [Log Stream — Forever Components](https://forevercomponents.com/components/terminal/log-stream/) — the component this demo is adapted from

Keep this attribution if you redistribute the component; the original's own terms apply to the parts taken from it.
