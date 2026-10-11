# GFS Chainline — Realistic 3D implementation plan

## Outcomes
A realistic Cities: Skylines-inspired Cape Town logistics city with authentic agricultural, industrial, residential, airfreight and port environments, **without losing the continuous shipping simulation**. This is a design target, not permission to use copyrighted Cities: Skylines assets.

## Phase 0 — source and runtime audit (NO CHANGES to gameplay)
1. Inventory source files, entry point, dependencies, world generation, shaders/materials, original asset licenses, renderer, camera, input, simulation clock, vehicle state, route graph, editor and layout persistence.
2. Check the actual running simulation against README claims. Screenshot the city, three distinct crop blocks, port, truck routes, Labia Theatre and airport at phone/desktop resolutions.
3. Confirm why trucks appear to pass through buildings: placement footprint vs road, graph geometry mismatch, route clearance, visual artifacts and intersections.
4. Benchmark startup, average/P5 FPS, draw calls, triangles, texture memory where available; log browser/GPU and visible scene. Record warnings and WebGL errors.
5. Establish scenario-based smoke tests for direct export, vessel delay/refrigerated depot, local delivery, airfreight, destination customs, empty return, and Lyudmila’s walking loop.
6. Identify safe extraction boundaries before touching the existing one-file application.
**Exit:** audit report, reproducible bugs, screenshots, performance baseline, small vertical-slice plan.

## Phase 1 — polished vertical slice, not a whole-city rewrite
- Select one realistic city block (including pavements, one junction, 8–12 plausible building lots), Orange Court/Labia-facing streets and one crop+packhouse sample.
- Establish PBR/GLB asset manifest: source, author, license, dimensions, textures, collision volumes, LOD and mobile fallback.
- Introduce controlled sky/sun/ambient balance, shadows, material and texturing calibration, fog and quality levels.
- Replace final-art placeholder shapes in the chosen area with carefully selected/licensed or Blender-authored assets. Test mobile early.
**Exit:** owner-approved day/dusk/night views and preserved movement.

## Phase 2 — reliable data-driven geography and freight routing
- Separate canonical road geometry, lane graph, parcel polygons, buildings, vehicle permissions and entrances into versioned scene data with a legacy adapter.
- Introduce freight bypass from orchards/packhouse to weighbridge/port and links to the empty/plug-in depots and air cargo; avoid residential side roads.
- Validate full footprints, road easements, turning envelopes and truck clearances; preserve pedestrian access.
- Expand mixed-use town blocks, GFS HQ, parks and residential areas in coherent districts.
**Exit:** no known houses/roads collision, all logistics destinations routable, dedicated truck lanes.

## Phase 3 — World Editor v2
- Extend existing E editor; retain legacy LAYOUT_OVERRIDES and deterministic mapping.
- Add movable lots, group/orchard objects, editable roads and pedestrian paths, selection/rotation/duplication, snapping, layers, undo/redo and touch controls.
- Road edits must pause simulation, rebuild lane mesh/graph/turn geometry, validate route connectivity, then commit or revert atomically.
- New versioned JSON save schema and legacy migration with import/export and rollback.
**Exit:** edit/reload round trip, invalid placement blocked, port and walking trips remain connected.

## Phase 4 — finish distinctive districts and motion
- Realistic citrus, deciduous apples/pears and trellised grapes; forklifts/bin tractors, detailed cold rooms and loading.
- Transporter office/yard, empty depot PTI/wash, reefer plugged bays, weighbridge queue, port gate/stacks/vessels/cranes and airfreight ULDs.
- Orange Court-to-Labia Theatre animated, plausible pedestrian journey on sidewalks with crossing signals.
- Parallel continuous operations: cargo movements, shipments assigned to vessels, customs, local and destination distribution.
**Exit:** all scenario tests and visual gates pass.

## Phase 5 — modularisation and deployment
- Gradually extract the single HTML program into ES modules. Potential structure:
  - `src/core/{clock,state,simulation}.js`
  - `src/world/{districts,roads,parcels,navigation,collision}.js`
  - `src/assets/{manifest,loader,materials,lod}.js`
  - `src/scenes/{orchards,packhouse,city,port,airport,destination}.js`
  - `src/actors/{trucks,ships,pedestrians,lyudmila}.js`
  - `src/editor/{history,inspector,persistence,validation}.js`
  - `tests/{unit,integration,e2e,visual}/`
- Prefer incremental extraction, not a disruptive framework rewrite. Vite is optional. If introduced, configure `base: '/animate/'` and change Pages artifact from `.` to `dist` only in a tested deployment PR.
- Use Playwright visual snapshots and browser tests; unit tests for graph/geometry; measure performance on multiple devices. Consider instancing, LOD and compressed KTX2 only with measured benefit.
**Exit:** regression/visual checks and roll-back plan confirmed before a production branch update.

## Acceptance across all phases
No stolen or improperly licensed game assets. No truck driving through houses, sidewalks or unrealistically tight corners. All existing flows continue, including delayed reefer handling. All editor saves are compatible or explicitly migrated. Provide actual test logs/screenshots and device-specific benchmarks; never claim success based solely on static code review.

## Release safety
The current workflow builds GitHub Pages on pushes to `main` and `claude/wonderful-gauss-m11r8c`. The improvement branch should stay separate. An unmerged pull request must not silently deploy production. Only merge after written owner approval.
