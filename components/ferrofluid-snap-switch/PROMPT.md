Build a "Ferrofluid Snap Switch" component as a single HTML file: a magnetic toggle where a ferrofluid drop stretches toward the cursor, merges with its anchors and snaps across on release. Zero dependencies, one inline IIFE, SVG goo filter, light-theme tokens included.

PALETTE AND TOKENS
- :root tokens, all --ffs- prefixed: --ffs-bg:#0A0A0A (ground), --ffs-track:#070707 (switch track), --ffs-text:#EDEDEA, --ffs-muted:#8A8A85, --ffs-faint:#5A5A56, --ffs-border:rgba(255,255,255,0.14), --ffs-accent:#C7F000 (one lime accent, no gradients), --ffs-on:#0A1200, --ffs-mono:ui-monospace,"SF Mono",Menlo,monospace.
- A :root[data-theme="light"] block repoints every token (bg #FAFAF8, accent #4D7A00, on #F0FFD0). The theme is chosen by a ?theme= bootstrap script in <head> or a {type:'forever:theme'} postMessage listener — keep both.

THE SWITCH
- A fixed full-viewport .ffs-root (flex column, centered, gap 1.2em, font clamp(6px,4.4vmin,28px)/1.2 monospace) holds the switch and a status line.
- The switch (.ffs-sw): 14.15em × 5.85em pill (border-radius 999px), track background, 1px border, cursor grab (grabbing while active), touch-action none, overflow hidden so the goo filter never escapes.
- Inside, an SVG (viewBox 0 0 184 76, class .ffs-goo, filter url(#ffs-goo)) with four circles: two anchors (#ffs-al cx 40 r 16, #ffs-ar cx 144 r 10), the main drop (#ffs-main cx 40 r 25) and a satellite (#ffs-sat cx 40 r 15). All share .ffs-blob fill (faint when off, accent when aria-checked="true").
- The goo filter: feGaussianBlur stdDeviation 9, then feColorMatrix "1 0 0 0 0 / 0 1 0 0 0 / 0 0 1 0 0 / 0 0 0 22 -9" (alpha contrast that merges neighbours), composited atop the source graphic.
- An IDLE/FLUX label row (.ffs-lbl) overlays the track, pointer-events none; the active side's label takes --ffs-on when checked.
- A status line (.ffs-info, aria-live polite) reads "POLE: NORTH (0)" / "POLE: SOUTH (1)".
- A small uppercase caption ("Ferrofluid Snap Switch", 10px, opacity .28) sits bottom-left.

SCRIPT (one IIFE, no libraries)
- Pointer x is mapped into the 184-unit viewBox space and clamped to 36–148. While dragging, the target follows the pointer; on hover (not dragging) the satellite eases 25% toward the pointer.
- Spring loop: time-based (dt) integration — vel += (target − cx) × .16 × dt, damped by pow(.74, dt) — painting cx, satellite cx (midpoint pull .55), and anchor radii (8 + up to 11 by proximity to the drop).
- On release: if within 14 units of the current pole, toggle; otherwise snap to whichever side of 92 the pointer is on. Keyboard: Space/Enter toggles, arrows set a pole.
- Attract mode: until the first deliberate input (pointerdown, keydown, or a forever:interactive event), the switch auto-snaps every 2800ms; any takeover clears the timer permanently.
- Visibilitychange sleeps the rAF loop and the attract timer, resuming on return.

ACCESSIBILITY AND MOTION
- role="switch" with aria-checked, tabindex 0, focus-visible outline in accent, aria-label on the group.
- @media (prefers-reduced-motion:reduce): transitions none, and the spring loop jumps straight to the target (input still lands, but the drop jumps instead of gliding).
- The goo filter costs more on low-end GPUs — keep the blur at 9, not higher.

WHAT BREAKS
- Changing the feColorMatrix alpha row (22 / -9) unmerges the blobs or eats them entirely.
- Removing overflow:hidden on .ffs-sw lets the blurred blobs bleed outside the pill.
- Dropping the dt clamp (50ms) makes the spring explode after a tab switch.
- State stays local to each mounted instance — a format/theme remount preserves takeover, not the switch value.
