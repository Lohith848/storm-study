# Design notes — The Storm Study

## The brief

"A new diorama, but full realistic, no low poly, full real assets, high quality, the best diorama." The earlier dioramas were stylised; this one had to
read as a photograph of a miniature, not as a game. So: real models, real paintings, physically based light, no flat-colour stand-ins, no empty corners.

## The idea

A cut-away room on a glossy display table: an aviator's study at the moment a storm takes out the power. Warm lamp light against cold lightning, a hearth,
a chess game left mid-move, a barometer falling. It is a *miniature* — the brick cut-away shows the wall thickness, the plinth has a brass plaque, the table
mirrors it — which also explains the dark surround without leaving the composition empty: the room fills the frame and the table's reflection and light pool
carry the rest.

No people: nothing in the scene needs a figure, and the helmet, the cold tea and the half-played game say the aviator stepped out.

## Composition and camera

* Fixed isometric-ish camera (azimuth 45 degrees, elevation 29 degrees, narrow field of view 20.5 degrees so verticals stay parallel and detail stays sharp). The
  distance is fitted by bisection so the whole plinth fills the window at any aspect ratio.
* The two visible walls: the window wall on the right (window, curtains, desk) and the fireplace wall on the left (hearth, bookcase, Hokusai). The chess table
  and the rug sit at the optical centre, where the eye lands first.
* Zoom (to 4.5x) and pan are part of the piece: textures, glass and the painting hold up at that scale.

## Assets

* 17 glTF models from the Khronos sample set, chosen for material range rather than for subject alone: velvet and silk (sheen), stained glass and amber
  (transmission, volume, IOR), the hurricane glass, iridescent glaze, brass and copper (metal), leather, the chess set. Each is scaled to a real size and set
  down on its surface (`LAYOUT` in `src/scene/props.js`).
* Architecture is procedural but physically textured: tileable value-noise maps for wood, wallpaper, brick, plaster, marble, felt, leather and rug are
  generated at build time (`tools/textures.mjs`) as base-colour, normal and roughness maps. Mouldings are extruded from real profiles.
* Paintings are public-domain reproductions (Turner over the fireplace, Hokusai on the west wall) with canvas-weave normal maps, varnish clearcoat, gilt frames,
  linen liners and a brass picture light.
* Room reflections come from a CC0 Poly Haven interior HDR, used only as an environment map, never shown.

## Light

Seven lights, all physical units: the storm (a directional light through the window with a 4096 shadow map), the desk lamp (spot, shadowed), and small
point lights for the lamp's glow, the coals, the lantern and the candle, plus a faint hemisphere fill.

Containing the storm light was the hard part: it must come in only through the window. Invisible, shadow-only casters (a ceiling, an east wall and a cap over
the window wall; they overlap generously so no slit passes light) stop it entering over the cut-away sides and top.

The timeline (`FLASH` in `src/config.js`) is six strikes of different strength, each a fast attack and a slower double-decay; the lamps stutter out at 23 s,
the room runs on fire and lightning, and they stutter back by 42.6 s. An eye-adaptation exposure curve brightens the dark and tames each flash.

## Render pipeline (`src/core/hq.js`)

1. **Scene** into an MSAA half-float target with a depth texture.
2. **SSAO** at half resolution (12 hemisphere samples at the high rung), bilaterally blurred.
3. **Volumetric light**: per half-resolution pixel, march the ray through the room's air (20 to 36 steps) and add the light scattered toward the camera, using the
   lights' own shadow maps for visibility, so window bars, curtains and furniture cut real shafts out of the storm light and the lamp throws a soft warm cone.
   Henyey-Greenstein phase, dust-density noise drifting on a loop-periodic circle, interleaved-gradient jitter. Thinned toward the open cut-away sides so the
   haze has no hard front. Upsampled with a depth-aware cubic B-spline so it neither grains nor leaks over silhouettes.
4. **Bloom** (unreal-style, additive, alpha-safe).
5. **Grade**: per-tap tone mapping (ACES) with a 2x2 supersample resolve, contrast-adaptive sharpening, studio backdrop gradient, vignette, dither.

The buffer is premultiplied-alpha (the page background is the studio gradient); overlays that add light, such as glass reflections and sparks, use custom blending
that leaves alpha alone, and the grade pass divides by it.

## The storm

The window is one shader on a pane (`src/scene/storm.js`): layered clouds that churn and glow when a bolt strikes, a distant tree line and sodium street lamps
that die with the power, rain streaks, and raindrops drawn as inverted-refraction lenses with dark rims and tiny glints, plus runs that slide down. It is a
shader instead of a video because it has to react to the same clock as the room's lights.

## Making a loop

Everything that moves is a pure function of one clock, `t`, wrapped at 56 seconds, with whole-number cycles per loop: rain scroll speeds, curtain sway, dust
drift, steam (period 8) and sparks (period 7). One-off events (the strikes, the power cut, the chess hops) are placed on the timeline and end where they began.
`qa/seam.mjs` compares every matrix, light, material and uniform at t=0 with t=56; frames rendered at 0, 56 and 112 s are pixel-identical.

## Quality and performance

Six rungs, eco to max (`src/core/governor.js`). Rule one: never render below the screen's native pixel ratio. When frames are missed the effects go first
(supersampling, glow, AO, volumetrics, real glass, shadow size, MSAA), and the controller can climb back, with failed trials backing off. Shaders for the moments
that introduce new ones (a flash, the power cut) are compiled up front so nothing hitches mid-loop. Software GL (headless checks) starts on the light rung and
skips the governor. Reduced motion opens on a still frame mid-flash.

## What was left out on purpose

People; an audio track (the piece is silent by design); interactivity beyond zoom and pan; any pre-rendered video.
