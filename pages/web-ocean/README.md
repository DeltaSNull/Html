# Web Ocean

A real-time ocean renderer written from scratch on raw WebGPU — no engine. Sunset sea with FFT-simulated waves, whitecaps, sun glitter, and a physically based cloudy sky, flown with a free camera.

![Web Ocean](preview.jpg)

**Open it:** [`index.html`](index.html) runs straight from disk — one file, every asset inlined, no build step. Requires a recent Chrome with WebGPU and a reasonably capable GPU.

**Build it yourself:** [`PROMPT.md`](PROMPT.md) is the one-shot brief describing this build. Paste it into any AI builder or coding agent.

| | |
| --- | --- |
| Best for | WebGPU showcases, procedural nature demos, graphics programming references |
| Style | dark, webgpu, ocean, procedural, sunset, realistic |
| Sections | stage — one full-viewport canvas |
| Libraries | none — raw WebGPU, no engine or rendering library |
| Fonts | none — system monospace HUD |
| Uses | webgpu, compute-shaders, fft-waves, quadtrees |
| Size | 102 KB · brief ~600 tokens |

The file opens with a header comment that maps the demo's structure and scripts, and lists what breaks when you edit it. Read that first. Controls: click the canvas for mouse look, WASD to move, Space/E up, Q/C down, Shift for 5x speed, wheel for base speed, H for the settings panel.

## Built on

- [web-ocean — GitHub](https://github.com/Noniv/web-ocean) — the demo this page is adapted from (MIT © 2026 Maksymilian Dendura)

Keep this attribution if you redistribute the page; the original's own terms apply to the parts taken from it. The preview image above is the upstream project's screenshot.
