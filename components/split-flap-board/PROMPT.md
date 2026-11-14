Build a "Split-Flap Board" kinetic-type component as a single HTML file: six amber tiles cycle through letters to spell airport-style words (DEPART, ARRIVE, ONTIME, BOARDS) on a dark charcoal background, each letter flipping with a rotateX animation. Zero dependencies, CSS animation, one IIFE.

PALETTE AND TOKENS
- :root tokens: --c03-bg:#161616 (charcoal ground), --c03-flap-bg:#0a0a0a (tile face), --c03-ink:#f4c430 (amber letters), --c03-border:#2a2a2a (tile edge), --c03-shadow:rgba(0,0,0,.5), --c03-divider:rgba(0,0,0,.7) (the split line across each tile), --c03-cap:#f4c430 / --c03-cap-o:.3 (caption).
- A [data-theme="light"] block repoints them (--c03-bg:#f2ede2, --c03-flap-bg:#fffdf6, --c03-ink:#7a5c00 dark amber, softer borders/shadows). A theme-hook script in <head> reads ?theme=light|dark from the URL and stamps data-theme on <html> before first paint, so there is no dark flash. Standalone default stays dark. A {type:'forever:theme'} postMessage switches theme at runtime.

THE BOARD
- A full-viewport .board (flex, centred, gap 8px, padding 20px) holds N=6 .flap tiles built by script. Each tile: width clamp(44px,9vw,64px), aspect-ratio 5/7, 1px border, 6px radius, overflow hidden, box-shadow; a ::after divider line splits it horizontally at 50%.
- Each tile holds a .ch span (bold monospace, clamp(28px,6vw,52px), amber) showing the current letter. .flap.spin .ch runs the flip keyframes (.09s linear): rotateX 0 -> -85deg -> 85deg -> 0.
- A small uppercase monospace caption ("departures", 10px, letter-spacing .08em) sits fixed bottom-right.
- seq=['DEPART','ARRIVE','ONTIME','BOARDS']; pool=' ABCDEFGHIJKLMNOPQRSTUVWXYZ'. stepTo() advances the current letter one alphabet position toward its target. tick() steps every tile one position per frame, re-triggering the spin animation (remove class, void offsetWidth reflow, re-add) only on tiles whose letter actually changed; when every tile matches its target, hold counts down 40 frames, then cur advances to the next word.

SCRIPT (one IIFE, no libraries)
- The board is built once (six .flap divs with .ch spans). draw(t) runs the loop: each frame it calls tick() and re-schedules via requestAnimationFrame while shouldFrame() is true. visibilitychange re-starts the loop when the tab returns.
- syncTheme() re-reads --c03-ink from computed style and applies it to every tile letter, so the board follows the CSS theme.
- With prefers-reduced-motion, init() shows the first word statically (setTarget(seq[0])) and never starts the loop.
- Demo lifecycle: window.__foreverPreview gates the loop (demoEnabled), and a forever:interactive event, pointerdown, click, or keydown takes over (stops the attract loop, paints the current frame).

ACCESSIBILITY AND MOTION
- The board is pure decoration with no text alternative; the caption is decorative. Reduced motion shows the first word statically with no flipping.

WHAT BREAKS
- Words longer than N=6 characters are silently truncated — change N to match the longest word in seq.
- The flip advances through the full alphabet sequentially, so letters far apart take longer to settle; that is the mechanical charm, not a bug.
- The spin animation must only re-trigger on tiles whose letter changed; re-triggering it on every tile every frame restarts the .09s flip before it can play.
- The animation loop must call tick() every frame — without it the board freezes after one step. The draw loop also needs phase and last declared; they were missing from the copied code and are declared here.
- The amber colour lives in --c03-ink; update the token (and the light block) to retheme.
