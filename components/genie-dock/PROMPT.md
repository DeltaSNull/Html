Build a "Genie Dock" window-minimise component as a single HTML file: a window pinches to a funnel, pours down into a dock slot with a thumbnail popping in, then plays back in reverse to restore. It loops autonomously as a preview until the first deliberate input. Zero dependencies, pure CSS keyframes + a small script.

PALETTE AND TOKENS
- :root dark tokens: --bg:#0d0f16 with a radial --glow wash, --win / --win-line / --win-bar / --txt (window dressing), --thumb / --thumb-line (dock thumbnail), --accent:#4FB3F0, --amber:#E8AB30 / --red / --green (traffic lights), --dock-bg / --dock-hl / --border / --slot-line, dock item gradients --g1…--g3, --cap (caption).
- Light re-theme in :root[data-theme="light"]; ?theme=light|dark before first paint, plus forever:theme postMessage at runtime.

THE GENIE
- .c15-scene holds the .c15-win (left/right 14%, top 10%, height 50%, transform-origin:50% 100% — the pour starts from the window's bottom edge) and the .c15-dock (centred bottom, blur backdrop) with three gradient items and a dashed .c15-slot button.
- minimise adds .is-min: keyframes c15-genie 1.05s — at 45% the window translates a third of the way and clip-path pinches to a funnel polygon (0 0, 100% 0, 62% 100%, 38% 100%), ending translated (--tx,--ty) and scaled .14/.22 at opacity .15.
- .is-back replays the same keyframes 0.9s in reverse, so restore mirrors minimise exactly.
- When the pour lands, .is-in on the slot pops in a thumbnail (scale .3→1 with a spring curve); it clears on restore.
- Slow the pour by raising the 1.05s on .is-min (and the 1000 ms timeout that follows it); change the funnel shape in the 45% keyframe's clip-path polygon; move the slot anywhere and aim() re-targets automatically.

SCRIPT (one IIFE, no libraries)
- aim() measures the slot centre relative to the window's bottom centre on load and resize — never mid-animation; it sets --tx/--ty.
- busy/minimised guards so a press mid-animation cannot double-fire.
- Autonomous idle: a 100 ms phase clock minimises at phase 12 and restores at phase 34 (of 58); pressing the amber button or the slot drives it manually and takes over the loop; pointerdown/keydown/forever:interactive stops the demo.
- visibilitychange stops/starts the phase clock.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce): the genie degrades to a fade (is-min → opacity 0, is-back → opacity 1), thumbnail transition and amber pulse removed.
- Buttons have aria-labels and focus-visible outlines; the window is role="img" with an aria-label.

WHEN TO USE / NOT TO
- Use for window managers, multi-document editors, and product tours that need to show where a minimised thing went.
- Not for layouts where the window and dock aren't both on screen (the pour needs a visible destination), or reduced-motion audiences.

WHAT BREAKS
- Changing transform-origin breaks the pour start point; measuring aim() mid-animation mis-targets the slot; removing the busy guards allows double-fire presses.
- The genie is an approximation — one clip-path funnel plus a scale, not a true mesh warp.
