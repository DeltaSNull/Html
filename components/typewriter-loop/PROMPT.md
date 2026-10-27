Build a "Typewriter Loop" component as a single HTML file: a monospace terminal line that types, pauses, and deletes phrases in a loop behind a glowing green blinking block caret, with randomized keystroke timing. Zero dependencies, one inline IIFE.

PALETTE AND TOKENS
- No :root tokens — hardcoded palette: ground #101418, text #e8eef2, prompt grey #5c6b78, caret green #46e6a0 with a 10px glow (rgba(70,230,160,.6)).
- Font: ui-monospace,"SFMono-Regular","Courier New",monospace, size clamp(24px,9vw,64px), weight 600, letter-spacing .02em. The background is hardcoded near-black — edit CSS to use on light backgrounds.

THE LINE
- Body is a centered flex row; the line (#tw) is white-space:nowrap so the caret never wraps to a second line.
- A prompt span (#pre, "> " in grey) leads, then a body span (#body) that the script fills, then the caret (#caret): inline-block .6ch wide, 1.05em tall, vertical-align -.16em, margin-left 2px, blinking via a 1s steps(1) keyframe that hides it at 50%.
- A small uppercase caption ("Typewriter Loop", 11px, caret green, opacity .4) sits bottom-left.

SCRIPT (one IIFE, no libraries)
- A phrases array ["type.","delete.","repeat.","kinetic_type();"] cycles forever. step() types one char per tick at 90+Math.random()*60 ms, holds 1100ms at the full phrase, deletes at 45ms per char, pauses 420ms, then advances to the next phrase (modulo the array length).
- start() only kicks off when no timer is running; stop() clears the timer. Visibilitychange pauses on hide and resumes on return.
- The timer id doubles as the running flag, so start/stop can't double-schedule.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce): the caret stops blinking and the script shows the first phrase statically instead of animating.
- No ARIA live region — screen readers won't announce the changing text; the line is ambient decoration.

WHAT BREAKS
- Removing white-space:nowrap lets long phrases wrap and the caret jump lines.
- Changing the caret to a border or outline breaks the block-caret look — it must be a filled .6ch box.
- The 1100ms hold and 420ms gap are what make the loop read as typing, not flicker — don't zero them.
- Background is hardcoded near-black; a light theme needs the ground, text, and caret colors repointed together.
