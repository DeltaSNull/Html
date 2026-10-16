Build a "Swipe Card Stack" microinteraction component as a single HTML file: a Tinder-style draggable card stack — drag the top card left/right to reveal NOPE/LIKE stamps, fling-away animates it off-screen and cycles to the next card. Zero dependencies, one inline script.

PALETTE AND TOKENS
- :root tokens (dark defaults): --c05-bg:#0F1B15 (ground), --c05-shadow:rgba(0,0,0,.4), --c05-like:#2BAB6B / --c05-nope:#ff5c7a (stamp colors, constant across themes), --c05-swatch-0:#ff8e5c / --c05-swatch-1:#5cc8ff / --c05-swatch-2:#87A194 / --c05-swatch-3:#5cffa0 / --c05-swatch-4:#ffd45c (card colors), --c05-cap:#fff / --c05-cap-o:.28 (caption).
- Light variant in :root[data-theme="light"] — pale green ground; swatches and stamps stay constant.
- Theme hook: ?theme=light|dark query param or a {type:'forever:theme'} postMessage switches at runtime.

THE STACK
- #um05-wrap (data-section="stage", role="group", aria-label, tabindex 0, cursor:grab, touch-action:pan-y pinch-zoom) centers #um05-stage: width min(54%, 180px, (100vh-96px)*.74), aspect-ratio .74.
- Three cards visible: each absolute, border-radius 18px, 42px emoji face, LIKE/NOPE stamps (13px, 800 weight, rotated ±12deg, opacity 0 until dragged), box-shadow, will-change:transform. Depth i: translateY(i*10px) scale(1-i*.06), z-index 10-i.
- Drag: pointerdown on the top card captures the pointer; move translates + rotates (x/12 deg) and fades the stamps in by |dx|/60. Release past 50px flings (400px, -40px, ±30deg, 420ms ease-in, fades out), then the deck rebuilds with idx+1. Short drags spring back with a .3s transition.
- Skip/Keep buttons and ArrowLeft/ArrowRight keys fling the same way. Buttons disable during flight.
- Idle demo: first card auto-flings 600ms after mount, then one every 2500ms alternating LIKE/NOPE, through the same fling(). Deliberate input stops the demo for the visit; the drag guard in tick() means it never fires while a card is held.

SCRIPT (one IIFE, no libraries)
- Single pointer ownership with pointer capture; pointercancel / lostpointercapture roll back the drag. Vertical pan and pinch zoom remain available via touch-action.
- visibilitychange pauses the demo timer (and any in-flight animation), resumes on visible.
- prefers-reduced-motion: no auto-swipe — the stack rests, drag still works; fling() completes instantly.
- demo-v1 preview lifecycle: demoEnabled from window.__foreverPreview; forever:interactive / pointerdown / keydown stops the attract loop permanently for the visit.

ACCESSIBILITY AND MOTION
- role="group" with an aria-label explaining the arrow-key controls; buttons are real <button>s with :focus-visible outlines.
- Reduced motion: static resting stack, drag and buttons fully usable.

WHAT BREAKS
- Removing the drag=null null-guard before fling lets a released-then-recaught pointer double-fling.
- Deleting the drag guard in tick() lets the auto-demo fire mid-drag and fight the pointer.
- shade() is a no-op stub — the card gradient's end color equals its start; write a real shader for depth.
- The pool is 5 cards and wraps modulo data.length; short lists repeat quickly.
