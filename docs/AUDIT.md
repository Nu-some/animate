# GFS Chainline: technical and visual audit

Date: 11 October 2026. Audited build: commit `cd9ae15` (HD by default). Method: static analysis of `index.html`, an instrumented headless Chromium run (renderer statistics, a 300-second fast-forward of the simulation, a road-collision scan for every origin theme), and 32 screenshots covering every area, night mode, all seven origin themes and four destination themes. Every existing feature was exercised and nothing was changed during the audit.

## 1. What the product is today

One HTML file (1.28 MB) that loads three.js r128 from a CDN and builds a continuous 3D map: inland town, forest town, farm, city, port, airport, Bay City over a bridge, the destination port across the sea, plus a flat network map of 21 ports and 86 lanes. A road graph (68 nodes, 122 directed edges) carries 19 planned vehicles; a vessel cycle, a freighter flight, a walking resident, pickers, bin trucks, cranes, a depot prep line and a guided tour sit on top. 33 clickable object types have detail panels. A layout editor, lists, a container tracker, weather conditions, day and night, and seven origin and fourteen destination looks exist.

## 2. Technical audit

### 2.1 Composition

| Part | Size |
|---|---|
| Whole file | 1,279,523 bytes |
| Embedded KayKit models (base64 GLB) | 1,020,520 bytes (about 747 KB binary) |
| Main script | 235,901 bytes, 193 functions, 1 closure |
| CSS | 12,408 bytes |
| HTML and markup | about 10 KB |
| External requests | 16 script files from two CDNs, 2 Google Fonts requests |

The GLB is 80 % of the download and is parsed on the main thread after load. Everything else is small.

### 2.2 Scene statistics (HD, 1280 by 780, after load)

| Metric | Value | Note |
|---|---|---|
| Meshes | 3,367 plain + 71 instanced (4,976 instances) | trees, pallets, cartons, dashes, towers are instanced |
| Groups | 270 | |
| Lines | 176 | bridge cables, vines, crane cables, map lanes |
| Materials | 549 unique | every `M(color)` call allocates a material; many share a colour and could be cached |
| Unique geometries | 1,901 | most `box()` calls make a new BoxGeometry |
| Textures | 68 | all canvas-generated, no image downloads |
| Lights | 7 | sun, hemisphere, 5 point lights |
| Shadow map | 4096 square, PCF soft, bias -0.0006 | one directional light that follows the camera target |
| Shadow coverage | x ±140, z ±120 around the target | the map is now 750 wide, so wide views lose shadows at the edges |
| Post-processing | SSAO, Unreal bloom (night), FXAA | linear colour pipeline, no tone mapping |
| Fog | exponential, density 0.0016 | |

Frame cost on the software renderer used for the audit is about 31 ms for the HD composite against 25 ms without it. That ratio (plus about 25 % for HD) is the useful number; absolute figures on a real GPU will be far lower.

### 2.3 Correctness

- **No page errors** across load, all area views, all origin and destination theme switches, the full tour, the editor, and the fast-forwarded simulation.
- **Simulation invariants hold** over 300 fast-forwarded seconds: 10 containers gated in, 17 in the yard, turnarounds 63 to 73 minutes, local, inland, Bay City and forest deliveries all counted, no NaN positions, no stalled vehicle other than one legitimately queued.
- **Road-collision scan**: zero buildings, trees or props on any road for every origin theme. The only flagged objects are intended (weighbridge plate, gate posts, sign gantry, bridge pylons and cables, bay forklift, depot boom, customs arch).
- **Unused code**: 12 declarations are dead (`PLATES`, `STOP_NAMES`, `addClientTruck`, `airTrucks`, `attachNewContainer`, `bayDoors`, `bayLines`, `blade`, `forceHQ`, `fpsT`, `fw`, `lb`) and the empty `trucks` array survives from the old loop system. Harmless, but should go.
- **Per-frame allocation**: `updateLabels` clones vectors for each of 47 labels every frame; `tick` allocates two vectors per frame. Minor garbage pressure.
- **Camera flights are frame-rate dependent.** `flyTo` advances by the clamped frame time, so a 1.2 s flight takes several seconds on a slow device. Cosmetic.
- **Theme switching disposes geometry** but not materials or textures; repeated origin switching leaks a little GPU memory. Minor.
- **Determinism**: the build runs on a seeded random until the model kit has loaded, then switches to real randomness; this is what makes the layout editor's saved positions stick. Correct, but if the GLB fails to parse the seed is never released (the sim then repeats the same random sequence every visit).

### 2.4 Robustness and delivery

- Works without the model kit (fallback boxes), without post-processing (plain render) and without WebGL (a fallback message).
- Pages deploy succeeds on every push; the artifact copy cannot use the editor's download (viewer restriction), clipboard export still works there.
- No analytics, no cookies, no external fonts beyond Google Fonts, no secrets, no freight rates. Container numbers carry valid ISO 6346 check digits.
- Accessibility: 47 `aria-label`s, reduced-motion respected, keyboard shortcuts (T, E, H, arrows). The 3D canvas itself is not screen-reader navigable (expected).
- Responsive CSS at 900 px and 560 px breakpoints.

### 2.5 Things the audit found technically weak

1. 549 materials and about 1,900 geometries for a scene of simple boxes is wasteful: a material and geometry cache keyed by colour/size would cut draw calls and memory with no visual change.
2. Shadow coverage is fixed at ±140 units while the world is 750 wide; the overview shows shadow cut-off lines.
3. One directional light and five point lights for a night scene with about 60 street lamps: lamps do not illuminate anything.
4. The GLB is parsed on the main thread; a short visible stall on slow devices after load.
5. The dest group is rotated by π so every destination coordinate is mirrored, which makes hand-placing destination content error-prone.

## 3. Visual audit

What reads well today: the port (cranes, stacks, straddles, ships), the packhouse interior, the bridge, the city blocks with real low-poly buildings, the Labia theatre, Table Mountain and the Cape Town waterfront, the network map, the night HQ glow, the HUD and labels.

What stops it from looking like a real place, in order of impact:

1. **Flat, uniform ground.** Every land mass is one beige slab with a faint noise texture. Real terrain has fields, grass, dirt, sand near the coast, verges along roads, concrete aprons at industry, hard shoulders and pavements. This single issue makes every wide shot look like a board game.
2. **Shorelines and water.** Land ends in a straight box with a thin beach box; only the main land has the animated surf. Water is one flat colour with a normal map: no shallow-water tint near the coast, no foam elsewhere, no waves.
3. **Night is dark where it should glow.** Street lamps, the port floodlights, the bridge, the airport approach lights, the theatre marquee, the towers of Bay City: none emit light. Only HQ, two spots and the vehicles do.
4. **The destination is a void.** One crane on a straight quay, a loop road in an empty field, generic trees, no town, no port stacks or warehouses, no quay furniture, no second berth. The destination themes differ only by ground colour and a few hills that sit off-camera; Dakar, Port Louis and Douala looked identical in the audit shots.
5. **Washed-out origin ground.** Five of seven origin themes lerp the ground 55 % toward white, so Port Elizabeth, Port Said, Damietta, Port-Vendres and Rotterdam look bleached next to Cape Town's 28 %.
6. **Theme landmarks straddle the new city.** The Damietta and Rotterdam field stripes run over the north street row and blocks; the Port Elizabeth dune shapes crowd the depot.
7. **Roads are flat grey ribbons with hard corners.** No kerbs, no pavements, no edge lines, no stop lines or crossings at intersections, no roundabout markings; the one-way loops in the towns are angular polygons.
8. **Vegetation is one lollipop.** The same sphere tree in the same green at the same scale almost everywhere; no hedges, windbreaks, grass, reeds at the water, bushes along roads, and the big south-west field is empty.
9. **The sea between the coasts is empty.** No islands, no sea marks, no ferries or fishing boats, no wake behind the voyager, no gulls.
10. **Scale and silhouette details.** Trucks are boxes without mirrors, lights, mudguards or tail boards; sheds have no gutters, pipes, air-con units on walls, signage beyond the front; no people outside the farm; no fences around yards and the airport; no gantry lights at the port.
11. **Sky and atmosphere.** A two-colour gradient and flat-shaded cloud blobs; no sun disc, no haze gradient that reads as distance, bloom only at night.
12. **Repetition.** Every town uses the same eight building models in the same scale range with the same random rotations; the three towns and Bay City's low rise look like one town cloned.

## 4. Plan (ordered, each step tested and committed on its own)

Functionality is preserved throughout: no simulation, UI, tour, editor or data change; only materials, geometry and decoration are added or adjusted, and every step re-runs the road-collision scan and the error check.

| # | Step | What changes | Risk |
|---|---|---|---|
| 1 | Ground and verges | Multi-tone ground texture (grass, dirt, sand), coastal sand gradient, grass verges and pavements along every two-way road, concrete aprons at industry, hard shoulder shading | low |
| 2 | Shorelines and water | Beach ring plus surf on every land mass, shallow-water tint around coasts, gentle wave motion, wake behind moving ships | low |
| 3 | Night lighting | Emissive lamp heads plus additive light pools under every street lamp, port floodlights, bridge deck lights, runway and approach lights, lit towers in Bay City, marquee glow; a small number of real point lights at hubs | medium (performance, kept instanced) |
| 4 | Destination port | A proper quay with bollards and fenders, two cranes, a container stack with straddles, warehouses, a small town with kit blocks, trees per climate, quay lights; destination landmarks moved into view | medium |
| 5 | Origin theme fixes | Ground lerp capped at 0.35 for all themes, field stripes moved north of the new blocks, dunes shifted off the depot | low |
| 6 | Road details | Kerbs, edge lines, stop lines and zebra crossings at city intersections, roundabout markings, rounded town loops, lane arrows at the port approach | low |
| 7 | Vegetation | Mixed species per region (round, umbrella, pine, cypress, palm by theme), hedges along orchard edges, windbreak rows, grass tufts, reeds at the shore, bushes along roads, the empty south-west field planted | low |
| 8 | Sea life | Two rocky islets with a lighthouse, a ferry between the port and Bay City, fishing boats, a buoy line at the harbour mouth, gulls over the port | low |
| 9 | Detail pass | Truck mirrors, lights, mudguards; shed gutters, wall units, side signs; fences around the yard, the airport and the depot; people at the port and city; gantry lights | low |
| 10 | Performance | Material and geometry cache, shadow frustum sized to the view, dead code removed, label update without allocation | medium (regression risk, done last with full re-test) |

## 5. Outcome

All ten steps were carried out the same night, each tested headless (no page errors, zero road collisions for every theme, the delay/collect cycle, the full tour, every theme switch, every clickable type, the editor) and committed on its own:

1. Ground: a painted multi-tone grass and earth texture, crop fields on the empty south-west ground, grass verges and pavements along every two-way road, hard shoulders along the loops.
2. Shorelines: wet sand, a shallow turquoise halo and a double surf band on every land mass; a wake behind the sailing vessel; calmer water.
3. Night: every street lamp, port mast, bridge light, runway light, floodlight and tower now glows and throws a warm light pool, plus real point lights at the hubs.
4. Destination: quay furniture, a second crane, general stacks, port warehouses, a cold store, a port town of kit buildings, a palm promenade, masts.
5. Origin themes: ground washout capped, Damietta and Rotterdam fields moved north of the new blocks, Port Elizabeth dunes clear of the depot.
6. Roads: stop lines and zebra crossings at every city intersection, a marked roundabout, smooth curved one-way loops and port approach.
7. Vegetation: hedges round the citrus block, cypress windbreaks by the apples, poplars, umbrella trees, pines, reeds on the south shores, verge bushes, palms, olives in the inland town.
8. Sea: two islets, a lighthouse that shines at night, a buoy line at the harbour mouth, fishing boats, gulls over the port, and a clickable ferry that crosses between the port and Bay City.
9. Details: mirrors, indicators, roof markers, side skirts and tail lights on trucks; gutters, downpipes and wall units on sheds; fences round the yard, the depot, the airfield and the plug-in depot; people at the gate, HQ, the theatre, the market and the cargo terminal.
10. Performance: box geometries are cached (about 1,900 unique geometries became about 90 shared ones), the shadow frustum follows the camera distance so wide views keep shadows, dead code removed, per-frame label maths without allocation.

