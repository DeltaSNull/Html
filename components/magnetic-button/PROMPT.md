Build a "Magnetic Button" interactive CTA component as a single HTML file: a dark pill button with a soft idle orbital float; cursor proximity pulls it magnetically, the label has its own inner-parallax, and hover adds an emerald-to-copper glow ring. Zero dependencies, one inline script, dark by default with a light re-theme.

TOKENS AND THEME
- :root tokens (dark defaults; a [data-theme="light"] block re-themes): --bg:#08090d (ground), --glow-a/--glow-b (ambient emerald/copper page glows), --cap:#fff / --cap-o:.28 (caption), --bg-surface:#1c2030 / --text-0:#f4f6fb (button), --border-mid / --border-glow, --grad-iris-cyan:linear-gradient(120deg,#2BAB6B,#C08459) (the hover gradient accent), --r-pill:999px, --glow-iris (hover ring shadow), --ease-out-soft, --font-display:"Space Grotesk",system-ui,sans-serif, --dur-fast:160ms / --dur-base:280ms.
- A theme hook runs before first paint: ?theme=light|dark in the query string (or data-theme on <html>) avoids a dark flash; a {type:'forever:theme'} postMessage switches at runtime.

THE BUTTON
- Full-viewport body (display:grid, place-items:center, overflow:hidden) with two ambient radial glows. A fixed full-viewport #c01-stage holds the button, centered.
- .c01-mag: border:0 pill (border-radius:999px), padding 18px 34px, Space Grotesk 600, font-size clamp(15px,4.5vw,18px), background var(--bg-surface), 1px mid border, transition on transform/border-color/box-shadow with --ease-out-soft, will-change:transform.
- ::before holds the --grad-iris-cyan gradient at opacity 0 behind the label (z-index:-1). On hover: border-color → --border-glow, box-shadow → --glow-iris. The glow appears as a gradient ring bleeding around the button.
- .c01-mag__label is a separate inline-block with its own transform transition — the inner parallax.
- A small uppercase caption ("Magnetic button", 10px, letter-spacing .08em, opacity .28, pointer-events implied by being out of the way) sits bottom-left.

SCRIPT (one IIFE)
- Magnetic pull: on pointermove over the button, translate the button by (cursor − center) × strength (0.4); translate the label by the same × 0.4. pointerenter sets hovering, pointerleave resets transforms (the CSS transition smooths the settle).
- Idle orbit: a rAF loop adds a gentle orbital drift when not hovered — x = cos(e*0.9)*14 + cos(e*0.37)*5, y = sin(e*0.7)*9 + sin(e*1.3)*3.
- demo-v1 preview lifecycle: demoEnabled reads window.__foreverPreview; a forever:interactive event, pointerdown on the button, or Enter/Space/Arrow/Escape keydown stops the attract drift permanently ("takeover"). Manual hover magnetism keeps working.
- visibilitychange stops/starts the loop (and resets transforms on stop). prefers-reduced-motion: no orbit loop, no magnetic apply (hover still changes border/shadow via CSS); the CSS media query collapses the transform transitions.

ACCESSIBILITY AND MOTION
- A real <button type="button"> with Space Grotesk 600 label. Reduced-motion users get a static button with CSS hover feedback only.

WHAT BREAKS
- The CSS transition on transform fights the rAF loop when exiting hover — visually fine, but two systems own transform during the settle; don't "fix" it by removing the transition or the settle looks abrupt.
- The magnetic effect applies only within the button bounding box, not a wider attraction radius — widening it needs a different hit test.
- The hover gradient sits on ::before with z-index:-1: the button needs a background of its own (--bg-surface) or the gradient shows through at rest.
