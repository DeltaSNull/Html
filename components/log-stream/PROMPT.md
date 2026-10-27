Build a "Log Stream" component as a single HTML file: a simulated live server log tail — new entries slide in every 650ms with color-coded INFO/WARN/ERROR/DEBUG levels and real-time timestamps. Zero dependencies, one inline IIFE, purely ambient.

PALETTE AND TOKENS
- No :root tokens — hardcoded palette: ground #0d1117, header and INFO green #4ade80, timestamp grey-green #8ba28b, WARN #ffa500, ERROR #ff5555, DEBUG #81a4bd, message #c8d0c8, header rule #1a2a1a.
- Font: clamp(11px,2.3vmin,12px)/1.55 ui-monospace,Menlo,"Courier New",monospace. The background is hardcoded dark — edit CSS to change theme.

THE LOG
- A full-height .lg03-wrap (padding 8px 10px 24px, flex column, overflow hidden) holds a header and the log column.
- The header (.lg03-header): "⬥ LOG STREAM · app.log" in green, bottom border, opacity .8, wraps anywhere so it never overflows.
- The log column (.lg03-log): flex column with a ::before spacer (flex:1) that bottom-pins entries, gap 1px, overflow hidden.
- Each entry (.lg03-entry): flex row, gap 6px, fade-in animation (opacity 0 + translateY 4px → visible, .25s ease): a timestamp span ([HH:MM:SS] from the local clock), a level span ([INFO]/[WARN]/[ERROR]/[DEBUG], padded to 5 chars with spaces), and a message span (flex:1, wraps anywhere).
- A small caption ("log stream", 10px, green, opacity .35, pointer-events none) sits bottom-left.

SCRIPT (one IIFE, no libraries)
- A 20-entry pool array cycles in order from a random start index. addLine() builds one row (timestamp, level, message spans), appends it, then trims: while more than MAX=10 lines or the column overflows its height, remove the first child.
- On load, 6 lines are seeded instantly; then setInterval adds one every 650ms.
- A resize listener re-trims so the column never overflows after a viewport change.
- Visibilitychange stops the interval when hidden and restarts it on return.

ACCESSIBILITY AND MOTION
- The whole wrap is role="img" with an aria-label describing the simulated log — it is decoration, not a real log.
- @media (prefers-reduced-motion:reduce): entries lose the fade-in animation, and play() fills MAX lines statically instead of streaming.
- Below 300px wide, entries switch to a two-column grid with the message spanning full width.

WHAT BREAKS
- Removing the ::before spacer top-pins the log instead of bottom-pinning it.
- Raising MAX without the scrollHeight trim lets entries overflow the viewport on short screens.
- Timestamps use the local clock, so they jump on tab switch — that is inherent, not a bug.
- No click interactivity — this is purely ambient; don't wire handlers onto rows expecting a terminal.
