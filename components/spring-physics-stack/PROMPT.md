Build a "Spring Physics Stack" reorderable priority-queue component as a single HTML file: a full-viewport tile holding a drag-to-reorder list where the lifted row tilts with pointer velocity and neighbours spring into their new slots via FLIP. Zero dependencies, pure HTML/CSS/JS with system fonts.

PALETTE AND TOKENS
- Forever Opus tokens on :root: --bg:#0A0A0A, --surface:#111111 / -2:#161616 / -3:#1C1C1C, --text:#EDEDEA / -muted:#8A8A85 / -faint:#5A5A56, --border (8% white), --accent:#C7F000 lime (single accent, no gradients), --accent-line/--accent-soft, --shadow-lg, --font-sans (system), --font-mono (ui-monospace stack), --ease-out:cubic-bezier(0.22,1,0.36,1).
- Light re-theme in :root[data-theme="light"] (bg #FAFAF8, accent #4D7A00); ?theme=light|dark query param is applied before first paint by an early inline script.

THE LIST
- A full-viewport .spk-tile (overflow hidden on html/body) holds a .spk-head header ("Queue Priority" + "N active" badge) and a .spk-list column of .spk-item rows.
- Each row: grab cursor, touch-action:none, will-change:transform, grip icon, name + mono sub-line, and a P0–P3 priority tag.
- Dragged row (.spk-drag): lime border, large shadow + glow ring, slight scale(1.03) and rotate(velocity * 0.35) tilt; FLIP-sprung neighbours (animateFLIP): each neighbour starts at its old slot with transition:none, then the CSS transition springs it home.

SCRIPT (one IIFE, no libraries)
- pointerdown captures the pointer and starts drag; pointermove tilts the row, and when the row's centre crosses a sibling's centre the DOM order swaps, startY is adjusted so the row doesn't jump, neighbours FLIP-spring, and badges re-number P0..P3. pointerup/pointercancel ends the drag.
- Keyboard: Alt+ArrowUp/Down moves the focused row; rows are role="option" in a role="listbox" with focus-visible lime outlines.
- Attract mode: a timed demo swaps neighbours every 1.5 s using the same FLIP; the first deliberate input (pointerdown/keydown, or the forever:interactive event) stops it permanently.
- visibilitychange pauses/resumes the attract clock; animation is dt-based.
- forever:theme postMessage switches themes at runtime.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce): transitions removed, no tilt, no attract mode.
- Rows are focusable listbox options with aria-keyshortcuts="Alt+ArrowUp Alt+ArrowDown".

WHAT BREAKS
- Forgetting to adjust startY after the DOM insert makes the dragged row jump mid-drag.
- Removing will-change:transform or the FLIP setup makes neighbour motion stutter.
- State is local to each mounted instance; a format/theme remount resets the queue order.
