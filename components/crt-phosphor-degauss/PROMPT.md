Build a "CRT Phosphor Degauss" arcade-monitor component as a single HTML file: a black 8-bit panel with a green-phosphor CRT screen, scanlines, vignette, and a DEGAUSS CRT COIL button that warps a glowing vector ship sprite. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- Hardcoded palette (no :root tokens): #000 panel and ground, #041008 CRT screen, #00E8D8 cyan (button border, phosphor stroke), #FFCC00 amber (degauss state, thrust flame), #8BAC0F green footer, #808080 hard shadow, #fff borders and header text.
- Font: Press Start 2P via Google Fonts for all panel text; the small caption is 10px system-ui.

THE PANEL
- The body is a flex-centered, overflow:hidden viewport holding .bg05-wrap (max-width 380px, 3px white border, 20px padding, 14px gaps, 6px 6px #808080 hard shadow, user-select:none).
- Header (.bg05-hdr, 12px): "ARCADE MONITOR 15KHZ" left, "NTSC 240P" in cyan right.
- CRT screen (.bg05-crt-screen, 140px tall, 2px white border, 8px radius, #041008): a 180x100 canvas (#bg05-crtCv) overlaid with .bg05-scanlines (4px repeating gradient, 45% black) and .bg05-vignette (radial darkening to 80% at the edges).
- Button (.bg05-btn): black ground, cyan text and border, 2px 2px shadow; :active translates 2px and drops the shadow. :focus-visible gets a 2px white outline offset 4px.
- Footer (.bg05-foot, 12px green): "SHADOW MASK: RGB" left, "60 FPS SYNCHRONIZED" in amber right.
- A small caption ("CRT Phosphor Degauss", 10px, opacity .55, pointer-events:none) sits fixed bottom-left.
- Container queries (container fit-22-crt-scanline-phosphor-glitch-pad on html/body) shrink the panel below 300px and hide header/footer/caption below 210px.

SCRIPT (one IIFE, no libraries)
- The render loop draws the ship: a cyan vector shield outline (amber while degauss > 0.2), a red core dot, and a flickering amber thrust flame, over a dark green fill and a rolling sync hum line.
- Clicking DEGAUSS sets degauss to 1.0; it decays 0.03 per frame and warps the ship (translate up to 18px, rotate up to 0.4 rad) while it lasts.
- Demo-v1 preview lifecycle: while demoEnabled (from window.__foreverPreview.demoEnabled, default true) the ship idles with a sine bob; the first pointerdown, click, or forever:interactive event stops the demo and keeps manual feedback.
- A frame scheduler (scheduleFrame) gates rAF on document.hidden and reduced-motion; visibilitychange pauses CSS via a data-paused attribute and stops the loop.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce) kills all CSS animations/transitions; the script's reduced-motion branch renders a static frame and repaints on input.
- The panel is role="region" with an aria-label; the button is keyboard-focusable with a visible outline.

WHAT BREAKS
- Removing the visibilitychange pause lets the loop run hidden tabs.
- Changing the canvas size without the screen's fixed height stretches the phosphor field.
- Control state is local to each mount; a format/theme remount preserves takeover, not component values.
