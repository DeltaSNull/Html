Build an "Iron Filings Field" component as a single HTML file: a grid of short rods aligns to a roaming dipole magnetic field like iron filings, tinted red/gold by nearest pole, reacting to the pointer. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- :root tokens: --c10-bg:#0c0c0e (ground), --c10-cap:#C43131 / --c10-cap-o:.55 (caption), --c10-fade:rgba(12,12,14,0.35) (per-frame trail wash, must match the page bg), --c10-s:85 / --c10-l:60 (filing hsl), --c10-pos:#f87171 / --c10-neg:#C08459 (pole colors).
- A :root[data-theme="light"] block holds the light palette (ground #f4f3f2, poles #db0b0b / #98613a, trail wash rgba(244,243,242,0.35)); ?theme=light in the URL or a {type:'forever:theme'} postMessage switches at runtime, and a theme hook script in <head> stamps data-theme before first paint so there is no dark flash.

THE STAGE
- A full-viewport canvas (#c10-canvas, 100% x 100%, display:block) is the stage, tagged data-section="stage" with role="img" and an aria-label describing the field.
- The filing grid: step = clamp(13, min(W,H)/15), rods start at step/2 offsets, each rod length step*0.42, lineWidth 2, lineCap round.
- Two poles orbit the center (radius 0.28 of viewport, second pole opposite phase, q +1 / -1), drawn as 4px dots in the pole colors.
- A small uppercase caption ("Iron Filings Field", 10px monospace, letter-spacing .08em, opacity .55, user-select:none) sits bottom-right.

SCRIPT (one IIFE, no libraries)
- DPR-aware sizing (min(devicePixelRatio, 2)) with setTransform; resize re-inits the grid.
- field(px,py): sums dipole contributions (s = q/d2, d2 floored at 200) plus a pointer attractor (80/d2) when the pointer is over the canvas; returns atan2 of the summed vector.
- Each frame: wash the canvas with the --c10-fade trail color, advance t += 0.012, recompute poles, then ease each rod's angle toward the field angle with wrapped interpolation (da wrap + f.a += da*0.2).
- Rod tint: hue 15 (red) when nearer the positive pole, hue 30 (gold) when nearer the negative pole, at hsl(hue, --c10-s%, --c10-l%); theme sync reads the tokens from getComputedStyle on every theme switch.
- Loop: requestAnimationFrame, skipped while document.hidden; the demo-v1 preview lifecycle reads demoEnabled from window.__foreverPreview and stops attract work on the window forever:interactive event (pointerdown/click/keydown also take over).

ACCESSIBILITY AND MOTION
- With prefers-reduced-motion, only two static frames render, then the loop stops — no looping static fallback.
- The canvas is role="img" with an aria-label; the caption is decorative.

WHAT BREAKS
- Removing the angle-wrap interpolation makes rods spin wildly instead of easing toward the field.
- Changing --c10-fade without matching --c10-bg leaves ghost trails in the wrong color.
- Raising the DPR cap past 2 or shrinking the step divisor much below 15 makes the grid heavy on low-end devices.
- The reduced-motion path renders static frames only; do not expect a looping fallback.
