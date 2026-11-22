# Credits and licences

The Storm Study is built from real, scanned-quality assets and open-licensed works. Nothing in the room is low-poly placeholder geometry, and nothing is
AI-generated imagery. Every third-party asset, who made it and under what licence, is listed here.

## 3D models (17) — Khronos glTF Sample Assets

All models come from the [glTF Sample Assets](https://github.com/KhronosGroup/glTF-Sample-Assets) repository (`Models/<Name>`), each model's README is the
source of the licence below. Eleven are **CC BY 4.0** (credit required — this page is that credit) and six are **CC0** (no credit needed, given anyway).

| Model | Used as | Author / rights holder | Licence |
|---|---|---|---|
| ChairDamaskPurplegold | upholstered bench by the desk | Eric Chadwick — © 2021 Wayfair | CC BY 4.0 |
| SheenChair | orange lounge chair | Eric Chadwick — © 2020 Wayfair, LLC | CC0 1.0 |
| GlamVelvetSofa | navy velvet sofa | Eric Chadwick — © 2021 Wayfair, LLC | CC BY 4.0 |
| SpecularSilkPouf | red silk pouf | Eric Chadwick — © 2023 Wayfair, LLC | CC BY 4.0 |
| StainedGlassLamp | the desk lamp | Eric Chadwick — © 2021 Wayfair | CC BY 4.0 |
| ChronographWatch | desk watch, its hands driven by the loop clock | Eric Chadwick — © 2025 Darmstadt Graphics Group GmbH | CC BY 4.0 |
| DiffuseTransmissionTeacup | steaming teacup | Polyhaven.com and Eric Chadwick | CC0 1.0 |
| MosquitoInAmber | amber paperweight | Loïc Norgeot (model), Sketchfab (real-time refraction) — © 2018 / 2019 Sketchfab | CC BY 4.0 |
| GlassHurricaneCandleHolder | candle on the desk | Eric Chadwick — © 2021 Wayfair, LLC | CC BY 4.0 |
| FlightHelmet | the aviator's helmet | Gary Hsu (conversion from Maya) | CC0 1.0 |
| AntiqueCamera | plate camera on a tripod | Maximilian Kamps — © 2018 UX3D | CC0 1.0 |
| PotOfCoals | copper pot of coals in the hearth | Eric Chadwick — © 2023 Darmstadt Graphics Group GmbH | CC BY 4.0 |
| ABeautifulGame | the chess set | MaterialX Project / ASWF (original model, © 2020); Ed Mackey (glTF conversion, © 2022) | CC BY 4.0 |
| GlassVaseFlowers | vase on the mantel | Eric Chadwick (vase), Rico Cilliers (flowers) | CC0 1.0 |
| Lantern | brass lantern on the mantel | Microsoft / sbtron (2017), Frank Galligan (2018, compression) | CC0 1.0 |
| IridescentDishWithOlives | dish on the shelf | Eric Chadwick — © 2020 Wayfair, LLC | CC BY 4.0 |
| DiffuseTransmissionPlant | potted plant | Eric Chadwick, Darmstadt Graphics Group GmbH (© 2024, materials and animation); Rico Cilliers (original asset, CC0) | CC BY 4.0 |

Licence texts: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/legalcode) · [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/legalcode)

### What was changed (CC BY 4.0 asks for this to be said)

* Textures were resized to at most 1024 px (512 px for the chess set) and re-encoded as WebP; geometry was quantised and meshopt-compressed
  (`tools/optimize.mjs`). The chess set's meshes were also simplified to 40 % of their triangles.
* Each model is scaled to a real-world size and placed in the room; lights shipped inside the models were removed and some emissive values are driven by the
  storm timeline (lamp, coals, lantern, candle) so they follow the power cut.
* Branding baked into textures was painted out: the Khronos, glTF, DGG and UX3D logos on the copper pot, the camera tripod and the watch (case back, clasp,
  dial), and the glTF/Khronos label on the hurricane glass (`tools/optimize.mjs`, `src/scene/props.js`). The logos are trademarks of their owners and are
  not part of this work.
* The chess pieces' small "ghost game" hops and the watch's hand animation are driven by the 56-second loop clock.

## Environment lighting

* **St Fagans Interior** (`assets/env/st_fagans_interior_1k.hdr`, 1K) — [Poly Haven](https://polyhaven.com/), **CC0**. Used only as soft room reflections on
  glass, brass and varnish; it is never drawn as a background.

## Paintings (public domain)

* J. M. W. Turner, *The Shipwreck of the Minotaur* (1805) — hangs over the fireplace. Public domain.
* Katsushika Hokusai, *Under the Wave off Kanagawa* ("The Great Wave", c. 1831) — framed print on the west wall. Public domain.

The reproduction images were cropped and resized from public-domain scans found in open-source repositories (`jcjohnson/neural-style`,
`lengstrom/fast-style-transfer`, example image folders). Frames, mounts, linen liners and picture light are modelled here.

## Made here

The room shell (brick cut-away, wallpaper, wainscot, chimney breast and mantel, plinth and glossy display table, window, curtains, rug, desk, bookcase, barometer),
the procedural PBR textures (`tools/textures.mjs`: wood, wallpaper, brick, plaster, marble, felt, leather, rug), the storm (window shader, lightning, rain,
power cut), the volumetric light, the particle effects and the render pipeline are original to this project.

## Software

[three.js](https://threejs.org/) r160 (MIT) · [meshoptimizer](https://github.com/zeux/meshoptimizer) decoder (MIT) · [esbuild](https://esbuild.github.io/) (MIT) ·
[glTF-Transform](https://gltf-transform.dev/) (MIT) · [sharp](https://sharp.pixelplumbing.com/) (Apache-2.0) · [Playwright](https://playwright.dev/) (Apache-2.0)
