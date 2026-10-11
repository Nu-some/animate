# QA gates — no visual upgrade without evidence

## Phase 0: reproducible baseline
Record exact git branch+commit, build/run method, browser+version, operating system, viewport, graphics adapter where available, seeded simulation state, FPS distribution, draw calls and JS errors. Capture baseline screenshots before code changes. Make a defect list with reproduction steps and impact.

## Functionality tests (automate where feasible)
- Farm citrus/apple/pear/grape harvest → bin cold room → packhouse wash/sort/palletise → pallet cold room.
- Direct export: inspected empty → loading → weighbridge → port gate → allocated vessel stacks → loading/berth → destination clearance, warehouse and empty return.
- Delayed export: weighed loaded reefer → powered holding depot → truck freed → new truck collects on port reopening → allocated vessel.
- Local market/DC and airport ULD/freighter flows proceed concurrently.
- Multiple trucking companies, queueing, vessel conflicts and port stack closure/reopening do not deadlock.
- Lyudmila can walk Orange Court → Labia Theatre → Orange Court, without ever leaving the sidewalk/crosswalk graph.
- Layout editor saves/loads and imports old overrides, with stable IDs, reset, undo/redo and collision warnings when available.

## Geometry/navigation tests
- No static building or prop footprint intersects road carriageway, shoulders or turn sweep. No road centreline intersects building interior.
- All logistics facilities are reachable by the correct vehicle class.
- Heavy trucks cannot route along residential-only streets, cut across home lots, or drive through walls.
- Vehicle intersection clearance works at tight corners, roundabouts and port queues.
- Roads changed by editor regenerate route graph, crossings, stops and turn envelopes; invalid changes are blocked.
- No depot bay accepts two trucks in one occupied slot; pedestrian paths never share road centreline.
- Run across multiple seeds and camera viewpoints; test both normal and delayed vessel conditions.

## Rendering and quality tests
- Compare fixed-angle, fixed-seed before/after screenshots: overview, orchard crops, packhouse loading, residential blocks, quay, Labia Theatre route, airport. Test daylight, overcast, night, desktop and mobile.
- Manual design review: grounded objects, no floating windows, scale consistency, consistent materials, realistic shadow softness, no obvious texture stretching, no distracting z-fighting.
- Browser console has no uncaught WebGL errors, missing GLBs/textures, unhandled promises or broken fetches.
- Measure performance on mid-range desktop and at least one Android-class mobile target or cloud device approximation (label proxy results honestly). Report FPS percentiles and memory/texture footprint.
- Include fallback mode if realistic graphics underperform; mobile need not match desktop texture density.

## Publishing and safety
- Confirm existing GitHub Pages triggers before edits. Non-publishing branches only until explicitly approved.
- If converting single HTML to Vite/modules, verify /animate/ base path and update Pages artifact build path only on a tested rollout branch.
- Keep rollback commit, snapshots and export of layout JSON. Never silently overwrite legacy saved configurations.
- Include license attribution for each imported art pack.

## Evidence required to mark a task complete
Report: **what changed; source commit; screenshots; test commands; pass/fail/blocked results; benchmarks; known issues; whether anything deployed.**
Passing unit tests cannot substitute for visual inspection. Code review cannot substitute for running the simulation.
