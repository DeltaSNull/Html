Build a real-time ocean renderer as a single self-contained HTML file, written from scratch on raw WebGPU with no engine or rendering library: a sunset sea with simulated waves, whitecaps, sun glitter, and a physically based sky with clouds, flown with a free camera.

THE WATER
- Tessendorf FFT wave simulation: six cascades of 256×256 samples, patches from 1103 m down to 0.26 m, so wavelengths run from about a kilometre to a few millimetres. Fetch-limited JONSWAP wind sea with Donelan-Banner directional spreading plus a narrow long-crested swell; dispersion includes the capillary term; choppy horizontal displacement sharpens crests. Inverse FFTs are radix-2 compute shaders in workgroup memory.
- Surface mesh: a CDLOD quadtree with 17 levels — vertices 6.25 cm apart near the camera, mesh reaching 160 km, continuous vertex morphing between levels.
- Shading: mip-mapped first and second moments of wave slopes (LEAN mapping) for correct anisotropic roughness at every distance; anisotropic Gaussian sun glitter with Smith masking sized to the solar disc; sky reflection from a per-frame rebuilt environment cube map with roughness-filtered mips; turbid blue-green water body lit by the sky hemisphere; subsurface glow on backlit crests; wind-gust patches modulating short ripples.
- Whitecaps start where the total surface Jacobian folds. Foam lives in two camera-centred world-space maps (128 m and 1 km), decays over time, drifts downwind, breaks into streaks with age; a procedural bubble texture gives it structure.

THE SKY
- Physically based sky with procedural clouds; sun disc with glare; haze and turbidity controls. Sky parameters feed the water reflection.

THE CAMERA AND UI
- Free camera: click the canvas to capture the mouse (Esc releases), mouse looks, WASD moves, Space/E up, Q/C down, Shift for 5x speed, wheel changes base speed. The camera keeps itself above the displaced water surface.
- HUD top-left (monospace): FPS + one-line help. H opens a settings panel: wind, swell, choppiness, turbidity, foam, sun, haze, clouds, exposure, bloom.
- URL params for testing: ?w=&h= forces render resolution, cam=x,y,z,yawDeg,pitchDeg sets the camera, fov=deg sets FOV, ui=0 hides the HUD, any settings key works (windSpeed=12&swellHs=2&sunElev=8).

STRUCTURE
- One inline <script type="module"> bundling the renderer: WebGPU init (fail with a full-screen message if navigator.gpu or the adapter is missing), WGSL modules (common, sim, sky, ocean, post), main loop, camera, HUD/panel, URL params.
- No external assets at all — everything procedural. Must open straight from file://.

WHAT BREAKS
- No WebGPU (old browser, no capable GPU, some headless environments): the page shows its error overlay and stops — environmental, not a code bug.
- The bundle is generated from upstream sources (index.html + js/ via build.mjs); hand-editing the 100 KB bundle directly is asking for drift — edit sources and rebuild.
