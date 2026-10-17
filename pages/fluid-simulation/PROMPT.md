Build a WebGL fluid simulation as a single HTML file: a full-viewport GPU Navier-Stokes solver — drag to paint swirling color dye with bloom, vorticity and sunrays. Zero build step, works in mobile browsers.

THE SIMULATION
- WebGL1 solver: advection, pressure projection (configurable iterations), vorticity confinement, dye + velocity dissipation. Six shader programs (advection, divergence, curl, vorticity, pressure, gradient-subtract) plus display shaders with bloom and sunrays passes.
- Double-FBO ping-pong for velocity, dye and pressure. Resolution tiers: SIM_RESOLUTION 128, DYE_RESOLUTION 1024, CAPTURE_RESOLUTION 512.
- Pointer/touch input paints splats: each drag stamps velocity (direction × force) and dye (color) into the fields. An initial center splat fires on load so the page is alive immediately.
- dat.GUI control panel (top-right): resolution, dye/velocity dissipation, pressure iterations, curl (vorticity), splat radius/force, shading, bloom, sunrays, pause.

STRUCTURE
- One inline <script> for dat.GUI (control panel library), one inline <script> for the solver. Full-viewport <canvas>, no other DOM.
- The `config` object at the top of the main script holds every tunable: SIM_RESOLUTION, DYE_RESOLUTION, DENSITY_DISSIPATION, VELOCITY_DISSIPATION, PRESSURE_ITERATIONS, CURL, SPLAT_RADIUS, SHADING, BLOOM, SUNRAYS.

WHAT BREAKS
- WebGL context creation failing (very old browsers): the page needs WebGL; guard with a fallback message if targeting them.
- Halving DYE_RESOLUTION quadruples dye memory bandwidth — the 1024 default is the mobile-safe pick.
- Removing the pressure iterations collapses the incompressibility and the fluid looks gummy.
