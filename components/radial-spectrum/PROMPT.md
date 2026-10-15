Build a "Radial Spectrum" audio-visualizer component as a single HTML file: 96 cyan radial bars radiate from a bass-driven pulsing core around a slowly rotating ring. A synthesized signal drives it by default; a "live" button switches to real microphone input via the Web Audio API.

PALETTE AND TOKENS
- ACC='#22d3ee' (cyan accent) with matching rgba(34,211,238,...) literals; bar
  colors computed per-bar as hsla() — bass end brighter and thicker, cooling
  toward treble. Ground #07080d is hardcoded in CSS. Type is ui-monospace for
  the button and caption.

THE VISUAL
- A full-viewport fixed .aav02-stage (inset:0) holds canvas #aav02-c filling it.
  DPR-aware sizing via setTransform, dpr capped at 2.
- N=96 bars: angle = rot + (i/N)*2π with slow ring rotation rot=time*0.16.
  Bar length = v*maxLen where maxLen=R*1.55 and R=min(w,h)*0.22. Each bar's
  value is smoothed toward its target with 0.22 (synth) / 0.5 (live).
- Pulsing core: the average of bars 0–2 drives the pulse radius — soft stacked
  translucent discs plus a bright inner disc.
- A 1.5px ring (rgba(34,211,238,0.30)) sits at radius R. Bars brighter than
  0.55 get a tip spark.
- A "live" mic button sits top-right (♪ live → ♪ on, or ♪ n/a when mic is
  unavailable); a small uppercase caption sits bottom-left.

THE SIGNAL
- synth(i,time): sum of three drifting Gaussian tonal peaks
  (p1=0.10+0.04·sin(t·0.23), p2=0.34+0.06·sin(t·0.17+1.3),
  p3=0.62+0.05·sin(t·0.31+2.7)), a beat LFO swelling the bass (time·1.7),
  treble shimmer, smoothed noise nz, and a bass-heavy tilt (1−0.42f).
- Live mode: AudioContext + AnalyserNode (fftSize 256, smoothingTimeConstant
  0.78) + getUserMedia, created only inside the click handler. Frequency bins
  are mapped with a pow(f,1.35) curve.

SCRIPT (one IIFE, no libraries)
- rAF render loop gated by demoEnabled (from window.__foreverPreview):
  forever:interactive / pointerdown / click / keydown (non-modifier) stop the
  attract loop; live mic keeps rendering.
- visibilitychange: stop the loop and suspend the audio context when hidden;
  resume when visible.
- pagehide cleanup: stop tracks, disconnect the source, close the audio
  context. The media source is never connected to the destination.

ACCESSIBILITY AND MOTION
- @media (prefers-reduced-motion:reduce) is handled in JS: render one static,
  legible frame (bars[k]=synth(k,3.0); frame(3.0)) and never start the loop.
- The canvas has role="img" with an aria-label; the button has an aria-label
  and a focus-visible outline.

WHAT BREAKS
- Removing setTransform(dpr,...) blurs the canvas on hidpi screens.
- Connecting the media source to the audio destination would blast sound —
  never connect it.
- Live mic needs HTTPS and the browser's permission; it degrades to "n/a".
- The #07080d ground is hardcoded; repoint the body background for light themes.
