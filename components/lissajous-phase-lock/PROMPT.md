Build a "Lissajous Phase Lock" loader component as a single HTML file: an oscilloscope trace cycles frequency ratios until it phase-locks; pointer position sets the sweep speed, click or Enter changes ratio. Zero dependencies, canvas 2D, one inline script.

PALETTE AND TOKENS
- :root tokens: --lsj-bg:#0A0A0A (ground), --lsj-text:#EDEDEA / --lsj-muted:#8A8A85 (text), --lsj-line:rgba(255,255,255,0.08) (grid lines), --lsj-accent:#C7F000 (one lime beam, no gradients), --lsj-mono:ui-monospace,"SF Mono",Menlo,monospace.
- A :root[data-theme="light"] block holds the light palette (ground #FAFAF8, accent #4D7A00); ?theme=light in the URL or a {type:'forever:theme'} postMessage switches at runtime, and a theme hook script in <head> stamps data-theme before first paint so there is no dark flash.

THE STAGE
- A fixed, full-viewport root (.lsj-root, data-section="stage", role="group", tabindex 0, cursor crosshair) centers the scope and HUD in a column.
- The scope (.lsj-scr, width 24em max 90%, aspect-ratio 210/170, 1px border, radius .6em, overflow hidden) has a CSS grid background (12.5% columns, 15.44% rows) and holds the canvas (.lsj-cv, 100% x 100%).
- The HUD (.lsj-hud, same width as the scope, space-between, letter-spacing .1em) shows the ratio (e.g. "RATIO 3:2") and the lock state ("ACQUIRING...", "TUNING...", "LOCKED ●").
- A small uppercase caption ("Lissajous Phase Lock", 10px monospace, letter-spacing .08em, opacity .28) sits bottom-left; the root gets a 2px accent outline on focus-visible.

SCRIPT (one IIFE, no libraries)
- Presets cycle [[3,2],[5,4],[3,4],[1,1]] with labels; the 1:1 preset is the "PLL 1:1 LOCK" state. Preset cycling every ~9s (cycle > 540 frames) is the attract work, stopped by the demo-v1 preview lifecycle (demoEnabled from window.__foreverPreview, stops on forever:interactive).
- draw(): 260-point trace, x = sin(a*t+phase)*68, y = sin(b*t)*54, centered at (105,85) in a 210x170 logical space, beam 2.2px in --lsj-accent, plus a dot at the live phase point in --lsj-text; canvas backing store scales by sc = w*dpr/210 with DPR capped at 2, re-sized by a ResizeObserver.
- Interaction: pointermove over the root sets speed = .004 + nx*.035 and shows "Δφ <deg>°"; click or Enter/Space advances the preset. Theme sync reads --lsj-accent/--lsj-text from getComputedStyle on theme messages.
- Loop: time-based (dt) animation via requestAnimationFrame; visibilitychange sleeps and resumes; reduced-motion change stops the loop and draws one static frame.

ACCESSIBILITY AND MOTION
- The root is a focusable group with an aria-label ("click or press Enter to change ratio"); the lock state is aria-live="polite"; the canvas is aria-hidden.
- With prefers-reduced-motion, one static frame is drawn and the loop never starts.

WHAT BREAKS
- Removing the dt clamp (min(now-last, 50)/16.667) makes the phase jump after tab switches.
- Changing the 210x170 logical space without updating sc, the grid percentages, or the trace amplitudes distorts the trace.
- The useWhen/avoidWhen/agentPrompt metadata is empty on the source page; do not invent guidance for it — the SAFE TO EDIT / KEEP INTACT / KNOWN LIMITATIONS lists are authoritative.
- State is local to each mounted instance; a format/theme remount preserves the takeover, not the component values.
