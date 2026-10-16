Build a "Liquid Toggle" UI control component as a single HTML file: a teal gooey SVG toggle whose knob slides with trailing liquid droplets while the track fades between off and on colors. Zero dependencies, one inline script.

PALETTE AND TOKENS
- :root tokens (dark defaults): --c01-bg:#0d1117 (ground), --c01-cap:#3fe0c5 / --c01-cap-o:.4 (caption), --c01-track-off:#1d2a33 / --c01-track-on:#0e8f7a (track), --c01-knob-a:#eafff9 / --c01-knob-b:#7df0db (knob radial gradient).
- Light variant in :root[data-theme="light"] — pale ground, deeper teal track.
- Theme hook: ?theme=light|dark query param or a {type:'forever:theme'} postMessage switches at runtime.

THE TOGGLE
- .c01-wrap (data-section="stage") centers a native <button> (.c01-stage, 300px max, aspect 1/1, aria-label "Liquid toggle", aria-pressed synced) containing an inline SVG (viewBox 0 0 200 110, aria-hidden).
- SVG: rounded-rect track (188×66, rx 33) filled with var(--c01-track-off/on) and a .5s fill transition; a goo group (feGaussianBlur stdDeviation 6 + feColorMatrix 24 -11 + feBlend) holding the main knob (r 26) and two droplets (r 13, r 9) filled with the knob radial gradient.
- paint(): knob x = 55 + 90*p; droplets lag behind at x − 16/27·sin(p·π) on the trailing side. p eases toward target (on?1:0) at .085/frame; snaps instantly under reduced motion.
- Click toggles; the knob slides and droplets stretch then settle. Track cross-fades via the fill transition.
- Idle demo: auto-toggles every ~100 frames until the first deliberate input (pointerdown, click, non-modifier keydown, or forever:interactive event) disables it permanently for the visit; manual settling always stays active.

SCRIPT (one IIFE, no libraries)
- rAF loop with wake()/visibilitychange suspension; the loop only runs while settling or demoing.
- prefers-reduced-motion: no animation, instant state paint; track transition disabled in CSS.
- demo-v1 preview lifecycle: demoEnabled from window.__foreverPreview.

ACCESSIBILITY AND MOTION
- Native <button> with aria-pressed kept in sync; :focus-visible outline; keyboard operable (click/Enter/Space).
- Reduced motion: instant toggle, no droplets animation.

WHAT BREAKS
- Removing the feColorMatrix alpha contrast (24 -11) un-goos the droplets into soft blobs.
- Breaking the aria-pressed sync leaves screen readers announcing the wrong state.
- The goo filter is a real GPU cost — don't multiply this toggle across dense forms.
- Boolean UI example: wire the on state to application data for production.
