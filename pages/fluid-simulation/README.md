# Fluid Simulation

GPU Navier-Stokes fluid solver in the browser — drag to paint swirling color with bloom, vorticity and sunrays. Runs from file://, works in mobile browsers.

![Fluid Simulation](preview.jpg)

**Open it:** [`index.html`](index.html) runs straight from disk — one file, every asset inlined, no build step. Needs WebGL.

**Build it yourself:** [`PROMPT.md`](PROMPT.md) is the one-shot brief describing this build. Paste it into any AI builder or coding agent.

| | |
| --- | --- |
| Best for | hero backgrounds, playful interactive pages, GPU technique references |
| Style | dark, webgl, fluid, particles, glow, interactive |
| Sections | stage — one full-viewport canvas |
| Libraries | dat.GUI (inlined, control panel only) |
| Fonts | none — system fonts |
| Uses | webgl, navier-stokes, pointer-events, touch |
| Size | 118 KB · brief ~400 tokens |

Drag anywhere to paint the fluid; the panel top-right tunes dissipation, vorticity, bloom and more. The file opens with a header comment that maps the demo's structure and scripts, and lists what breaks when you edit it. Read that first.

## Built on

- [WebGL-Fluid-Simulation — GitHub](https://github.com/PavelDoGreat/WebGL-Fluid-Simulation) — the demo this page is adapted from (MIT © 2017 Pavel Dobryakov)

Keep this attribution if you redistribute the page; the original's own terms apply to the parts taken from it. Vendored adaptation: the upstream multi-file demo (separate JS files) is bundled into this single file; its Google Analytics snippet and mobile-app promo overlay were removed.
