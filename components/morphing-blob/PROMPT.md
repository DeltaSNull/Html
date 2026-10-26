Build a "Morphing Blob" loader component as a single HTML file: an aurora-gradient blob slowly morphs its border-radius and rotates, with an inset dark ghost layer counter-rotating, glowing on a dark backdrop. Zero dependencies, CSS-only animation, one tiny script.

PALETTE AND TOKENS
- :root tokens: --bg-900:#08090d (ground), --bg-600:#181b27 (ghost layer), --accent-iris:#2BAB6B / --accent-magenta:#87A194 / --accent-cyan:#C08459 (aurora gradient stops), --grad-aurora:linear-gradient(135deg, iris 0%, magenta 55%, cyan 100%), --glow-a/b (ambient radial glows, 16% opacity), --blob-glow:rgba(43,171,107,0.6) (box-shadow glow), --cap:#C08459 / --cap-o:.45 (caption).
- A :root[data-theme="light"] block holds the light palette (ground #f2f4f8, ghost #ffffff, glow rgba(32,126,79,.25)); ?theme=light in the URL or a {type:'forever:theme'} postMessage switches at runtime, and a theme hook script in <head> stamps data-theme before first paint so there is no dark flash.

THE STAGE
- The body is a grid (place-items:center) over the dark ground plus two ambient radial glows (60vw/50vw, top corners, fading to transparent).
- The blob (.c02-blob, data-section="stage", role="img", aria-label "Morphing blob loader"): width min(34vmin,150px), aspect-ratio 1, position relative.
- The shape (.c02-blob__shape, absolute inset 0, background var(--grad-aurora), saturate 1.1, box-shadow 0 0 40px -8px var(--blob-glow)) runs two animations: c02-blob-morph 6s cubic-bezier(.65,0,.35,1) infinite (three border-radius states: 42/58/63/37, 63/37/41/59, 38/62/56/44) and c02-blob-rot 9s linear infinite.
- The ghost (.c02-blob__shape--ghost, inset 11%, background var(--bg-600), no shadow, no filter) runs c02-blob-morph2 (mirrored radius states) and c02-blob-rot 9s linear infinite reverse — counter-rotating against the main shape.
- A small uppercase caption ("Morphing Blob", 10px monospace, letter-spacing .08em, opacity .45, user-select:none) sits bottom-left.

SCRIPT (one IIFE, no libraries)
- On visibilitychange, toggles animationPlayState paused/running on both shapes to save cycles when the tab is hidden.
- A forever:theme message listener stamps data-theme on <html> for live re-theming; no animation code — motion is CSS keyframes only.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce): collapses all animations (animation-duration:.001ms, iteration-count:1).
- The blob is role="img" with an aria-label; it is purely decorative with no progress semantics.

WHAT BREAKS
- Removing the c02- namespace prefix risks class collisions when lifting the blob into another page.
- Changing the ghost inset without the mirrored morph2 keyframes breaks the counter-rotation illusion.
- The light re-theme lives in the [data-theme=light] token block; the catalog bg/accent describe the dark default.
- This is decorative only — do not use it where progress indication is needed.
