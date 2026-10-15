# 3D Tilt Parallax

A dark card tilts in 3D toward the cursor while badge, orb, bar, title and subtitle layers float at distinct Z-depths. A radial shine tracks the pointer; the card auto-rotates until the first deliberate input. CSS 3D, one tiny script, Google Fonts.

![3D Tilt Parallax](preview.jpg)

**Open it:** [`index.html`](index.html) runs straight from disk — one file, every asset inlined, no build step.

**Build it yourself:** [`PROMPT.md`](PROMPT.md) is the one-shot brief this file was built from. Paste it into any AI builder or coding agent.

| | |
| --- | --- |
| Best for | product cards, profile cards, hero feature panels |
| Style | dark (light-ready), 3d, card, pointer, shine, depth |
| Sections | stage |
| Libraries | none — plain HTML, CSS 3D and a tiny JS tilt/idle loop |
| Fonts | Space Grotesk, Inter (Google Fonts) |
| Uses | css-3d, pointer-parallax, theme-switch, reduced-motion |
| Size | ~13 KB · brief ~700 tokens |

The demo region is tagged `data-section`, and the file opens with a header comment that maps its design tokens, sections and scripts, and lists what breaks when you edit it. Read that first. The `<script type="application/json" id="forever-agent-meta">` block in `<head>` carries the component's agent-ready metadata: editable areas, elements to preserve, and known limitations.

Dark is the standalone default; open with `?theme=light` for the light token set.

## Built on

- [3D Tilt Parallax — Forever Components](https://forevercomponents.com/components/cards/tilt-parallax/) — the component this demo is adapted from

Keep this attribution if you redistribute the component; the original's own terms apply to the parts taken from it.
