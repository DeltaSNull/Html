Build a "Light Caustics Lens" interactive card component as a single HTML file: a card that tilts toward the pointer while soft lime and neutral caustic light shifts behind it. Zero dependencies, CSS + canvas 2D, one inline script.

PALETTE AND TOKENS
- Forever Opus tokens on :root: --accent #C7F000 (lime — the one accent, no gradients), --accent-line rgba(199,240,0,.32), --bg #0A0A0A, --surface #111111, --text #EDEDEA, --text-muted #8A8A85, --border-strong rgba(255,255,255,.14), --shadow-lg 0 16px 48px rgba(0,0,0,.55).
- :root[data-theme="light"] repoints everything pale (--bg #FAFAF8, --accent #4D7A00); switch via ?theme=light|dark URL param or a postMessage {type:'forever:theme', theme}.
- Type: system-ui stack, ui-monospace for data readouts. Card size scales with viewport: .cpr-stage font-size max(4.5px, 2.5vmin), card is 28em x 34em.

THE CARD AND THE LIGHT
- Full-viewport .cpr-stage (perspective:900px, display:grid place-items:center) holds .cpr-wrap with a caustics canvas (oversized by 3.2em so the light spills past the card edges) and the .cpr-card.
- Card: 1em radius, surface at 62% alpha over the canvas, strong border, inner content (mono readout row "IOR // 2.417" + live "Δλ: N.Nnm" in lime, title "Light caustics", sub copy). role="group", tabindex=0, focus-visible lime outline.
- Pointer: tilt to ±14deg rotateX/rotateY, shadow offsets --cpr-sx/--cpr-sy (lime line one side, border line the other), all lerped with time-based (dt) easing. Arrow keys tilt too; card stops its attract demo on first pointer interaction or keypress.
- Caustics: 3 blobs x 18 flat concentric discs at low alpha (lime at .035, text color at .02) on a 200x200 canvas — intentionally low-res and gradient-free so per-frame cost stays flat. Discs drift on sine paths offset by the tilt.
- Demo attract loop tilts the card gently until the "forever:interactive" event fires or the user touches it.

SCRIPT (one IIFE, no libraries)
- Tokens read from computed style so theme switches apply to canvas colors instantly.
- visibilitychange sleeps the loop; prefers-reduced-motion collapses animation (snap to target, no drift).

ACCESSIBILITY AND MOTION
- Card is keyboard-operable (arrow keys), focusable with a visible outline, labeled as a group; canvas aria-hidden.
- prefers-reduced-motion: no attract drift, animation snaps directly to the target tilt.

WHAT BREAKS
- Replacing the stacked flat discs with gradients or blur filters destroys the per-frame budget and the intentionally soft look.
- Removing the demo-v1 lifecycle (window.__foreverPreview + forever:interactive) leaves the attract loop running over user input.
- Removing overflow:hidden on html/body allows horizontal scroll — keep it.
