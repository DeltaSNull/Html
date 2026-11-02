Build "Stacked Peeking Cards" as a single HTML file: three layered gradient cards auto-cycle a springy fan in and out, and fan out with rotation on hover or focus; cards spread left and right, then restack when the cursor leaves. Zero dependencies, one small script, full light-theme re-theme support.

PALETTE AND TOKENS
- :root tokens (dark defaults): --bg-800:#0d0f16 / --bg-surface:#1c2030 (page + front-card grounds), --text-0:#f4f6fb / --text-2:#8a91a8, --accent-iris:#75894D / --accent-cyan:#A57855 / --accent-magenta:#5C6670 (layered gradient dots), --grad-iris-cyan, --border-mid (12% white), --r-md:14px, --space-4:16px, --shadow-lift, --font-display:"Space Grotesk" / --font-body:"Inter" (Google Fonts), --ease-out-soft, --ease-spring:cubic-bezier(0.34,1.56,0.64,1) / --dur-slow:520ms, --cap-o:.4, --c09-page-a/b + --c09-card2/3-a/b (page and back-card gradients).
- A :root[data-theme="light"] block re-points every token (pale page, white cards, soft shadows, pastel back-card gradients, deeper accents) — this is the shipped light theme; preserve it.
- A head IIFE reads ?theme=light|dark before first paint and stamps data-theme (no dark flash). A forever:theme postMessage listener flips data-theme live.

THE STACK
- #c09-stage (position:fixed, inset:0, flex-centered; tagged data-section="stage") centers .c09-stack: 42vmin × 53vmin (max 210×268px), cursor pointer, role="button", tabindex=0, aria-expanded, aria-label.
- Three .c09-stack__card layers: back card (scale .9, amber-dark gradient), middle (scale .95, blue-dark gradient), front (scale 1, surface/800 gradient) — each with a rounded gradient dot and placeholder labels ("Layer 1..3", "Front/Middle/Back").
- Fan: the auto-cycle class .c09-stack--auto rotates card 1 to -9deg/translate(-2px,6px), card 2 to scale .97/translateY(-4px), card 3 to +9deg/translate(2px,6px) — all on the spring easing with bottom-center transform-origin.
- A small uppercase caption ("Stacked Peeking Cards", 11px, letter-spacing .08em) sits bottom-left.

SCRIPT (one IIFE, no libraries)
- Demo auto-cycle: a 100ms interval toggles the fan every ~1.6s closed / ~1.9s open (skipped while hovered or focused). The first deliberate input (pointerdown on document, card click, or forever:interactive) calls stopDemo, ending the attract cycle; manual click toggles the fan and paints aria-expanded; Space/Enter keydown triggers click.
- visibilitychange stops/starts the timer; prefers-reduced-motion sets transition-duration:.001ms and never starts the timer.

ACCESSIBILITY AND MOTION
- The stack is focusable with a visible outline and Space/Enter toggles the fan; aria-expanded tracks state; focus-within fans like hover.
- @media (prefers-reduced-motion:reduce): transitions collapse to .001ms and the auto-cycle never runs — the stack sits static until clicked.

WHAT BREAKS
- Dropping the demo-v1 lifecycle marker (meta name="forever-preview-lifecycle") or the __foreverPreview.demoEnabled initialization breaks preview embedding.
- Removing the :root[data-theme="light"] block strands the component in dark mode; the light palette is not derivable from the dark tokens.
- The hover/fan selectors depend on the [aria-expanded] attribute being set by paint() — if aria-expanded is never stamped, the CSS state model drifts.
- Card content labels are placeholder text only; wire real content in before shipping.
