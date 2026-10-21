Build a "Weight Morph Wave" kinetic-type component as a single HTML file: the word MORPH on deep navy, each letter pulsing font-weight and vertical squash in a staggered sine wave, shifting hue from cyan to blue. CSS + one rAF script, zero dependencies.

PALETTE AND TOKENS
- :root --kw-* tokens (dark defaults): --kw-bg:#0d1b2a (deep navy), --kw-ink:#e0fbfc, --kw-glow:rgba(120,200,255,.25), --kw-l:72% (lightness fed to the JS hue loop).
- :root[data-theme="light"]: pale bg #eef4f9, teal ink #15707e, --kw-l:34%. ?theme=light|dark query param before first paint, or a {type:'forever:theme'} postMessage at runtime; syncTheme() re-reads --kw-l and forces a recolor pass.
- Letters: clamp(38px,18vw,140px), Helvetica Neue/Arial stack, text-shadow glow 0 0 24px var(--kw-glow), transform-origin:center bottom.

THE STAGE
- body is a centred flex container; #kt01-word holds one span per letter (display:flex, line-height .9, letter-spacing -.02em); a small uppercase caption ("Weight Morph Wave", opacity .28) sits bottom-left absolute.
- html/body overflow:hidden — what prevents horizontal scroll at 390px. font-size clamps with 18vw so the word fits narrow viewports; designed for words ≤8 characters, viewports ≥280px.

SCRIPT (one IIFE, no libraries)
- frame: t += 0.045; per letter i, phase p = sin(t − i*0.6); weight 200..900 quantized to 25-step snaps (fewer reflows), scaleY 0.9..1.25, hue = 180±40 at --kw-l lightness.
- Redundant style writes are skipped (lastW/lastHue caches) to avoid needless reflow/repaint.
- visibilitychange pause; reduced-motion sets static font-weight 600 and never starts the loop.

ACCESSIBILITY AND MOTION
- prefers-reduced-motion: no rAF loop; letters render at weight 600 in the theme-aware ink colour.

WHAT BREAKS
- There is no variable-font axis — weights snap in 25-unit steps. Swapping in a real variable font needs font-variation-settings instead of quantized font-weight.
- Changing the word beyond 8 characters overflows at 18vw on 390px viewports.
- Removing the lastW/lastHue skip-write caches causes a style write + reflow on every frame per letter.
