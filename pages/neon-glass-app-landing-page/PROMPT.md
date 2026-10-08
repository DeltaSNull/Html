Build a single-file HTML landing page for "Arcline", a fictional platform that connects a company's tools and answers questions from its own data. Dark, blue-black and glassy. Everything inline - no CDN, no webfont, no image file, not one network request - and say in the footer that the company and every figure are invented.

THE GROUND
Near-black with blue in it rather than grey: --gray900 hsl(226 45% 3.5%) up to --gray50 hsl(214 45% 97%), on a hue ramp that warms slightly as it lightens (226 down to 214), so the neutrals never read as flat charcoal. One accent family: --blue hsl(218 95% 56%) with a lighter --blue-hi and a --blue-tint at 12% alpha for chips. Wash the page with two enormous off-centre radial gradients pinned behind everything at z-index -2 - one blue at 88% 12%, one cyan at 8% 78% - because a backdrop-filter has nothing to catch on flat colour. That wash is what makes the glass read as glass.
Set every panel's own background as three stacked gradients: a hue1 wash from the top-left corner, a hue2 wash from the bottom-right, and a near-opaque dark base underneath. Two corners lit in different hues is the whole trick; one flat tint looks like a grey box.

THE EDGE LIGHT - four layers, and they have to be separate elements
Each panel carries nine absolutely positioned spans, and they are not decoration you can collapse into pseudo-elements: a sheen, a sweep, a sweep bloom, two shines and four glows. They stack because each does one thing the others cannot.
- shine: a conic-gradient clipped to a 1px border box by the mask-composite trick - mask: linear-gradient(transparent), linear-gradient(black) with mask-clip: padding-box, border-box and mask-composite: subtract. That subtract is what leaves only the border ring. Give it border-top-right-radius: inherit and border-bottom-left-radius: inherit and place one at the top-right and one at the bottom-left, so each lights one corner of the panel rather than ringing the whole thing.
- glow: the same conic ring at 22px of border width, blurred 12px, in mix-blend-mode: plus-lighter, masked by an inline SVG feTurbulence so the bleed is grainy rather than a clean halo. Plus-lighter is the one that adds light; screen and lighten both go milky over a dark panel.
- glow-bright: a sharper 5px ring hugging the shine, blur 2px, no noise mask.
- sweep: one comet of light that races around the border. Register --sweep with @property as an <angle> with an initial value, or the conic-gradient will not interpolate and you get a jump cut instead of motion. Animate it -60deg to 300deg over about 1.7s, with a 6px blurred copy behind it in plus-lighter.
- sheen: a single diagonal glint travelling across the face, background-size 260% and background-position going 130% to -30%.
Order the arrival: the sweep runs first, then the corner lights ignite as it passes them - hold the shine and glow animations back by a --corner-lead of 0.65s on top of the panel's own --fx-base delay. Give each of the nine a slightly different --fx-d so they do not all snap on together.

THE HERO INPUT
A chat bar that rearranges itself. Three states driven by CSS alone:
- Idle: four attachment icons sit to the left of the field, and the field is indented past them with margin-inline-start on a --gap-closed of 7.75rem.
- Typing: :focus and :valid pull the field left to --gap-open, blur the icons away, slide in a round plus button, and swap the microphone for a send button. Use :valid with a required input - that is what makes "has text" a CSS state rather than a JS one. Put the transition on a cubic-bezier(0.175, 0.885, 0.32, 1.05) so it overshoots.
- Voice: a hidden checkbox expands the microphone button into a 19rem panel and reveals an orb. The orb is four radial-gradient circles orbiting inside an overflow-hidden core on different durations (4.8s to 6s, deliberately coprime so the pattern does not repeat), a backdrop-blurred glass cap over the top, and two conic rings rotating in 3D under perspective. Two more blurred rings pulse outward from the centre on a 1.5s loop. The "listening" label gets a moving gradient text clip.

THE PAGE
Sticky header, a hero with three entering widgets, then blocks: features, a three-step how-it-works, stats, a developer section with a fake code window, pricing with a monthly/yearly switch, an FAQ built from native details/summary, and a closing CTA. Reveal each on scroll with an IntersectionObserver that disconnects after firing once, and drive the panel's light show off the same is-visible class.
The three step cards each get their own inline-SVG illustration, animated with SMIL and CSS: dotted flow lines with animateMotion dots running along them, a document being scanned into a point cloud, and an agent run log with a spinner and a progress bar that grows on reveal. Draw the point cloud from a seeded PRNG rather than Math.random so the layout is the same on every load.

TWO THINGS THAT WILL BITE
Set text-transform: lowercase on html, body, inputs and svg text to match the wordmark - then exempt the code block and its title, or the sample turns into unreadable all-lowercase JavaScript.
Every animation in the edge-light system and the orb needs a prefers-reduced-motion branch. Do not just stop them: the sweep and sheen must be display: none and the corner lights must be set to opacity 1, or a reduced-motion visitor gets panels with no edges at all.
