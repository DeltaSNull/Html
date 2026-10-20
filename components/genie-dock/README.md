# Genie Dock

A window minimises into its dock slot with a genie squeeze — it pinches to a funnel, pours down into the slot, and a thumbnail pops in — then plays back in reverse to restore. It loops as a preview until first deliberate input; press the amber button and the slot to drive it. Zero dependencies.

![Genie Dock](preview.jpg)

**Open it:** [`index.html`](index.html) runs straight from disk — one file, every asset inlined, no build step.

**Build it yourself:** [`PROMPT.md`](PROMPT.md) is the one-shot brief this file was built from. Paste it into any AI builder or coding agent.

| | |
| --- | --- |
| Best for | window managers, multi-document editors, product tours showing where a minimised thing went |
| Style | dark, css-animation, clip-path, interactive, mobile-ready |
| Sections | stage |
| Libraries | none — plain HTML, CSS and JS |
| Fonts | system-ui |
| Uses | css-animation, clip-path, pointer-interaction, reduced-motion, light-theme |
| Size | ~15 KB · brief ~800 tokens |

The demo region is tagged `data-section`, and the file opens with a header comment that maps its design tokens, sections and scripts, and lists what breaks when you edit it. Read that first. The `<script type="application/json" id="forever-agent-meta">` block in `<head>` carries the component's agent-ready metadata: editable areas, elements to preserve, and known limitations.

## Built on

- [Genie Dock — Forever Components](https://forevercomponents.com/components/navigation/genie-dock/) — the component this demo is adapted from

Keep this attribution if you redistribute the component; the original's own terms apply to the parts taken from it.
