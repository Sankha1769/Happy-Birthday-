# 21 Doors — a personalized, cinematic birthday experience

Everything is still one `index.html` (plus `assets/planets/`). Open it through a local server for the best result (see "Running it").

## What changed

| Area | Change |
|---|---|
| **Gate** (`#intro-screen`) | Name **and** photograph are mandatory. *Begin Journey* stays disabled until the trimmed name is non-empty and a JPEG/PNG/WebP has loaded successfully. Drag-and-drop or browse, preview, replace, remove, loading state, specific error messages (wrong type, over 15 MB, corrupt/unreadable). No skip. |
| **No bypass** | `journeyUnlocked` is checked in `goToPage`, the door click, background clicks and the keyboard handler; `#mainContent` is `inert` until unlock and the submit handler re-validates. Reloading returns to the gate (nothing is persisted). |
| **Personalization** | The visitor's name appears on page 1, the Mirror's final reflection, the sky message, page 12, the final page and the tab title. The photo appears in the Mirror, page 1 and the final page. There is no stock/`Main.jpg` fallback. |
| **Page 4 – Mirror of Time** | CSS-built frame, glass sheen and depth; the visitor's photo emerges from fog as the mirror is tapped; word cycling kept; last reflection is the name. |
| **Page 9 – Bridge** | Three.js scene, fully procedural (canvas-painted stone/bump textures, instanced blocks, moss, wind-swayed grass, fir trees, drifting mist layers, floating motes, lanterns, sun glow). Roots = green morning light, Journey = silver-blue fog, Now = warm gold. The three stages are real buttons; each moves the camera, lights its lantern and shows the original message. |
| **Page 10 – Room of Rain** | One full-screen GLSL shader: rain on glass with drops and sliding trails, refraction of a blurred night-city, rain streaks at three depths, a wet street with ripples and reflections, window frame. EMPATHY / DEPTH / ALCHEMY are buttons that change emphasis (the glass, the reflective pool, a refracted spectrum) while the rain keeps running; messages preserved. |
| **Page 12 – Solar system** | Kept and refined (Sun shader + corona, day/night Earth with city lights, clouds, atmosphere, textured tidally-locked Moon, stars, bloom). |
| **Page 13 – Butterfly** | SVG butterfly with flapping wings on a slow, smooth path that eases down under the pointer; SVG meadow. Emoji removed. |
| **Page 15 – Window** | Four-pane window with condensation; wipe (pointer or keyboard) to reveal each of the four messages. |
| **Page 16 – Letter** | CSS 3D envelope, paper texture, wax seal, larger readable letter, opening animation; keyboard operable. |
| Also | Candle (page 11) and balloons (page 18) are SVG. The gift box and bouquet (page 20), the secret heart (page 3), a feather (page 14) and a few ✨ accents still use emoji (see limitations). Inactive pages are hidden from keyboard focus. |

## Writing upgrade (copy pass)
All story copy was rewritten except **page 14 (The Silent Poem), which is byte-for-byte unchanged** (its HTML and its script block were hashed before and after). Address is "you" throughout; the visitor's name is used sparingly (page 1, mirror finale, sky blessing, Moon message, letter greeting, finale line).

| Page | What changed |
|---|---|
| 1 | One extra invitation line; clearer door prompt |
| 2 | 8 whispers (was 5), 4.2 s each so longer lines can be read |
| 3 | New subtitle, secret-heart note, 21 new floating wishes (learning, travel, creating, laughing, ordinary joys) |
| 4 | 8 reflections ending with a personal greeting; the first tap now shows the first reflection (previously it skipped it); photo logic unchanged |
| 6 | 7 typed lines (wonder → confidence → possibility), closing blessing; line area allows two lines on phones |
| 7 | Books now show a cover title; tapping reveals a joke in a readable panel under the shelf (keyboard accessible) |
| 8 | 12 unique one-liners, one per hour. (The old list repeated one line three times and had no 6 or 7 o'clock message.) |
| 9, 10 | Two-line intros; every stage/theme message rewritten as one connected thought (roots → journey → now) |
| 11 | Five-line warm reflection, no "eternity" language (title kept as in the original) |
| 12 | Three new messages: Stardust, Phases, Gravity |
| 13 | 9 butterfly messages in a shuffled bag (no repeats until all are seen) |
| 15 | Four new pane messages, plus a closing line once all four are revealed |
| 16 | Full letter; the sheet widens and centres itself on opening so nothing is clipped (scrolls if a screen is very short) |
| 17 | Four directions (curiosity, courage, rest, discovery); the message always matches where the needle actually stops |
| 18 | 20 messages in a shuffled bag; the 12th pop gets a finale line |
| 19, 20 | Three distinct secrets; staged three-line gift note |
| 21, 22 | New pause-before-the-finale passage; a rhymed birthday message ending on the name line |

Longer messages use a readable serif (`.msg-read`, same stack the letter already used) instead of the script font.

## Engineering notes
- **One animation loop.** All scenes register per-frame hooks on the page's existing `requestAnimationFrame` loop (`frameHooks`). `createSceneHost` gives each WebGL scene lazy loading, a single renderer, pause when not on screen, fade-out then GPU disposal after leaving, context-loss recovery (restore within 4 s, else lite), and an FPS-based downgrade high → medium → lite.
- **Preloading.** Three.js is requested only one page before it is needed (page 8 for the bridge, page 9 for the rain, page 11 for the solar scene).
- **Reduced motion.** WebGL scenes render a single still frame; the butterfly rests; transitions are shortened.
- **Canvases never take pointer events**; all interactive stage elements are real `<button>`s with focus styles.
- Debug/test handle: `window.__doors21`.

## Quality tiers
`high` (desktop: bloom, shadows) → `medium` (phones / low-power: no post, lower pixel ratio, fewer particles) → `lite` (CSS-only scenes: no WebGL, data-saver, ≤1 GB RAM, load failure, context lost, or sustained slow frames).
Force one with `?quality=high|medium|lite` (`?solar=` still works).

## External assets and credits
| Asset | Source | If missing |
|---|---|---|
| Three.js r160 + bloom/post add-ons | jsDelivr CDN (import map in `<head>`); MIT licence | All 3D scenes fall back to CSS versions |
| Planet textures `assets/planets/*` (earth_atmos_2048.jpg, earth_lights_2048.png, earth_specular_2048.jpg, earth_clouds_1024.png, moon_1024.jpg) | three.js r160 `examples/textures/planets` (NASA-derived imagery). **I did not verify the exact licence terms — check the three.js repository before commercial use, or swap in your own maps under the same filenames.** | Local file → jsDelivr copy → generated placeholder |
| Bridge and rain scenes | No downloaded assets; every texture is generated in code | — |
| Tailwind CDN, Google Fonts | Unchanged from the original | — |

## Running it
- **Local server (recommended):** `python3 -m http.server 8000` in this folder, then open http://localhost:8000.
- **Double-click (`file://`)**: works, but Chrome blocks local images in WebGL, so planet textures come from the CDN copy (needs internet).

## Privacy
The photo is downscaled in a canvas (max 1000 px) and kept as an in-memory blob URL for the tab's lifetime (revoked on replace/remove). It is never uploaded; nothing is stored in localStorage, cookies or IndexedDB. Tests confirmed zero non-GET requests and no request bodies.

## Known limitations
- Frame rate and GPU cost were **not measured on real hardware**. Tests ran in headless Chromium with software WebGL, which only proves correctness, not smoothness. The built-in FPS guard downgrades the tier automatically on slow devices.
- Three.js and (on `file://`) planet textures come from jsDelivr; the live CDN could not be reached from the test sandbox, so a local copy of the same r160 build was served in its place.
- Bridge geometry/shader look was reviewed from software-rendered screenshots only.
- The gift box and bouquet (page 20), secret heart (page 3), feather (page 14) and a few ✨ accents still use emoji; they were left because the simple interactions work well.
- Planet sizes and distances are artistic, not to scale.
