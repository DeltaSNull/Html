# Stacked Peeking Cards

Three layered gradient cards auto-cycle and fan out with a springy rotation on hover; cards spread left and right, then restack when the cursor leaves. Zero dependencies, light-theme re-theme included.

![Stacked Peeking Cards](preview.jpg)

**Open it:** [`index.html`](index.html) runs straight from disk — one file, every asset inlined, no build step.

**Build it yourself:** [`PROMPT.md`](PROMPT.md) is the one-shot brief this file was built from. Paste it into any AI builder or coding agent.

| | |
| --- | --- |
| Best for | card collection previews, playlist/deck UI, portfolio thumbnails where showing multiple items in compact space matters |
| Style | dark, cards, stack, fan, hover, spring, gradient, ui-component |
| Sections | stage |
| Libraries | none — plain HTML, CSS and a tiny auto-cycle/toggle script |
| Fonts | Space Grotesk, Inter (Google Fonts) |
| Uses | hover-fan, auto-cycle, keyboard-toggle, theme-switch, reduced-motion |
| Size | ~12 KB · brief ~600 tokens |

The demo region is tagged `data-section`, and the file opens with a header comment that maps its design tokens, sections and scripts, and lists what breaks when you edit it. Read that first. The `<script type="application/json" id="forever-agent-meta">` block in `<head>` carries the component's agent-ready metadata: editable areas, elements to preserve, and known limitations.

## Built on

- [Stacked Peeking Cards — Forever Components](https://forevercomponents.com/components/cards/stacked-peeking-cards/) — the component this demo is adapted from

Keep this attribution if you redistribute the component; the original's own terms apply to the parts taken from it.
