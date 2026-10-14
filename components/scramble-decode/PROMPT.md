Build a "Scramble Decode" text-effect component as a single HTML file: gold monospace text resolves from scrambling random glyphs left-to-right with a tracking caret, holds with micro-flickers, then re-scrambles through a 6-phrase loop. Canvas 2D, zero dependencies, one inline script, dark by default with a light re-theme.

TOKENS AND THEME
- :root tokens (dark defaults; a [data-theme="light"] block re-themes): --tte01-bg:#0a0b10 (ground), --tte01-acc:#f5d76e (gold accent), --tte01-dim:#3a3d4a (unsettled glyph color), --tte01-cap-o:.4 (caption opacity).
- A theme hook runs before first paint: ?theme=light|dark in the query string (or data-theme on <html>) avoids a dark flash; a {type:'forever:theme'} postMessage switches at runtime.
- JS color constants (BG/ACC/DIM) are re-synced from the tokens via syncTheme() at load and on every theme message — edit the tokens, not the literals; the canvas fill uses the synced values every frame.

THE LOOP
- Full-viewport canvas #tte01-c (display:block, width/height 100%) inside an explicit 100%x100% [data-section="stage"] wrapper (overflow:hidden) so the canvas height resolves to the full viewport. DPR-aware sizing (DPR capped at 2, setTransform); resize re-sizes and re-renders.
- PHRASES = ['DECODE','SIGNAL','ENTROPY','RESOLVE','SCRAMBLE','CLARITY']; GLYPHS = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789#%&@$/<>*+=?'.
- Timeline per phrase (seconds): DEC=0.78 decode-in → HOLD=1.55 hold → OUT=0.5 scramble-out, then advance to the next phrase.
- Per-char decode: each char gets lock=i*0.052 (left-to-right stagger); over the next .34s it settles with easeOutCubic — unsettled chars render seeded random glyphs (rnd from char seed + time, so it shimmers) in mix(DIM→ACC), with jitter up to glyphSize*0.09 and an anticipation scale overshoot (1.18→1).
- Tracking caret: an accent bar under the decode front (alpha pulsing with sin(now/130)), parked at the right end during hold and out.
- Hold phase: resting micro-flickers — a couple of chars randomly swap to a glyph when (floor(now/90)+seed)%37===0, so the locked text feels alive.
- Out phase: re-scramble with a right-to-left feel (reversed lock q over OUT seconds), alpha fading to .5.
- Glyph cell: glyphSize=min(H*0.22, (W*0.78)/(maxLen*0.62)) so the longest phrase fits ~74% of width; cell width = glyphSize*0.62; font '700 <size>px ui-monospace,Menlo,Consolas,monospace'.
- A small uppercase monospace caption ("Scramble Decode", 10px, letter-spacing .08em, color var(--tte01-acc), opacity var(--tte01-cap-o), pointer-events:none) sits bottom-left, fixed.

LIFECYCLE AND MOTION
- visibilitychange pauses/resumes the rAF loop. prefers-reduced-motion draws one static frame (first phrase fully decoded, caret parked) and never loops; resize re-renders that frame.

WHAT BREAKS
- The cell width factor (0.62 × glyphSize) assumes a true monospace font — a non-monospace fallback mis-aligns the glyph columns.
- Bypassing syncTheme() (hardcoding colors in JS) breaks the light re-theme; the canvas reads the synced tokens every frame.
- Phrase cycling is fully automatic — there is no click-to-advance; adding one means resetting phase, start, and calling loadPhrase().
