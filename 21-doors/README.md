# 21 Doors — personalization + 3D solar system

Open through a local web server for the best result (see "Running it"). Everything is still one `index.html`.

## What changed in index.html
| Area | Change |
|---|---|
| Opening screen (`#intro-screen`) | Name (required, validated) + optional photo with preview / replace / remove. Page 1 can't be reached until it's submitted. |
| Personalization | Name appears on pages 1, 12, 22, in the sky message, the Mirror's last reflection, the page-12 messages and the browser tab title. Photo appears in the Mirror of Time, page 1 and the final page. |
| Page 12 | Emoji/CSS orbit replaced by a Three.js scene (Sun shader + corona, textured Earth with day/night, clouds, atmosphere, textured Moon, stars, bloom on desktop). The three poetic messages are now buttons that also steer the camera. |
| Engineering | Three.js loads only when the visitor reaches page 11→12. One renderer, driven by the page's existing animation loop. Cleans up on exit and context loss. |
| Also fixed | Inactive pages are now hidden from keyboard focus (tabbing used to be able to scroll the page sideways). The Mirror no longer depends on placehold.co. |

## External assets
| Asset | Where it comes from | If missing |
|---|---|---|
| Three.js r160 + bloom add-ons | jsDelivr CDN (import map in `<head>`) | Page 12 shows a lightweight CSS version (no emoji) |
| Planet textures (`assets/planets/*`) | Tried in this order: your local `assets/planets/` folder, then the jsDelivr copy of the three.js examples repo | Generated placeholder textures (stylized, clearly lower quality) |
| `Main.jpg` | Your own file next to index.html (original behaviour) | Used only when the visitor skips the photo; a neutral silhouette shows if it's missing |
| Tailwind CDN, Google Fonts | Unchanged from your original | — |

Texture files included: earth_atmos_2048.jpg, earth_lights_2048.png, earth_specular_2048.jpg, earth_clouds_1024.png, moon_1024.jpg.
They come from the three.js examples repository (NASA-derived imagery) — check the licence notes in that repo before using commercially,
or swap in your own (e.g. Solar System Scope maps) using the same filenames, or edit `PLANET_FILES` in the script.

## Running it
- **Double-clicking index.html (file://)**: works, but browsers block local images from WebGL, so the textures load from the CDN instead of `assets/planets/`. Needs internet.
- **Local server / hosting**: `python3 -m http.server 8000` in this folder, then open http://localhost:8000 — local textures are used first.

## Quality tiers (automatic)
`high` (desktop, bloom) → `medium` (phones/low-power, no bloom, lower pixel ratio) → `lite` (CSS only: WebGL missing, data-saver, <=1 GB RAM, failed load, context lost, or sustained slow frame rate).
Reduced-motion users get a single still frame. Force a tier for testing with `?solar=high|medium|lite`.

## Privacy
The photo is resized in a canvas and held as an in-memory blob URL for the tab's lifetime. It is never sent anywhere and nothing is written to localStorage, cookies or IndexedDB.
