Build an "X-Ray Blueprint Loupe" card component as a single HTML file: a sealed chip card where a pointer-tracked circular loupe reveals a live copper-bus blueprint with die telemetry underneath. Tap/click toggles narrow/wide lens. Zero dependencies, canvas 2D, one inline script. Light-theme ready.

PALETTE AND TOKENS
- :root Forever Opus tokens: --bg #0A0A0A, --surface* (#111111/#161616/#1C1C1C), --text #EDEDEA / muted / faint, --border rgba(255,255,255,.08); one lime accent --accent #C7F000 (no gradients).
- Light theme (:root[data-theme="light"]): paper #FAFAF8 surfaces, ink #1A1A18 text, deep olive accent #4D7A00.
- Fonts: system-ui stack + ui-monospace. Ease: cubic-bezier(.22,1,.36,1).

THE CARD
- .xrl-stage centers a .xrl-card (28em × 35em, crosshair cursor, touch-action none). Two layers: .xrl-shell (badge "Tensor Core NPU", chip "HX-09", title "Halcyon Die Rev.4", meta row) and .xrl-xray (blueprint grid background, live canvas, CAD tag, telemetry: die temp / VDD / clock / parity).
- The x-ray layer is clipped by clip-path:circle(var(--xrl-r) at var(--xrl-x) var(--xrl-y)); a .xrl-ring outlines the lens.
- Pointer: move/drag tracks the lens (lerped); a press without move is a tap toggling wide (radius .43W vs .244W). Keyboard: arrows move the lens, Space/Enter toggles; an aria-live region announces lens state.

SCRIPT (one IIFE, no libraries)
- Theme hook: ?theme=light|dark bootstrap plus a forever:theme message listener that re-reads the CSS tokens.
- Demo lifecycle: demoEnabled from window.__foreverPreview; attract mode drifts the lens until the window 'forever:interactive' event.
- Time-based (dt) animation with rAF sleep: the loop stops once the lens settles and nothing autonomous is running; visibilitychange cancels.
- prefers-reduced-motion: transitions off; the loop still settles to a static frame.

ACCESSIBILITY AND MOTION
- role="region"/"group" with aria-labels; the card is focusable (tabindex 0) with a :focus-visible outline; telemetry uses aria-live.
- @media (prefers-reduced-motion:reduce): transitions off.

WHAT BREAKS
- Breaking the clip-path circle math detaches the lens ring from the revealed area.
- Forgetting to re-read tokens on theme change leaves the canvas drawing dark colors on the light theme.
- The rAF sleep condition must account for demoEnabled or the attract loop never sleeps.
