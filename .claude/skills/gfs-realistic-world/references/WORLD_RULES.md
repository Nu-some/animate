# Operational world rules: geography, routing and placement

## Single navigable world
Treat rendered road geometry and its driveable lane graph as a single source of truth. A road move must update its mesh, junctions, allowed vehicle classes, stop lines, crosswalks, routing weights, lane centre lines, turn paths and obstacle envelopes. If the current engine stores these separately, establish a compatibility adapter before editing roads.

## Zoning and street safety
- Lots, houses, façades, trees, fences and street furniture cannot overlap driveable lanes or junction turning envelopes.
- Buildings need actual footprint and setback checks, not centre-point checks.
- Freight bypass/industrial arterials connect farms, packhouses, empty depot, weighbridge, reefer depot, port and airport. Residential streets exclude articulated trucks by default.
- Model truck length and turning sweep at crossings/roundabouts; prevent clipping and illegal U-turns.
- Use distinct entry, exit, waiting, pickup, discharge and power-bay zones at depots. Prevent queue spillback into intersections.
- Public sidewalks do not overlap truck loading areas. Allow foot crossings only at modeled safe links.
- If movement is impossible, hold and report blocked route; never teleport through houses.

## Simulation invariants
1. Harvested fruit bins travel from crop block to bin cold storage, then packhouse.
2. Packed cartons move into pallets and cold rooms, then either export or local-distribution dispatch.
3. Export truck obtains inspected empty, collects loaded pallets, weighs, and either goes direct to correct vessel's port stack or to plugged reefer storage if port cannot receive it.
4. A parked/plugged reefer releases the truck for other jobs; later dispatch retrieves the container.
5. Port gate queue, stack placement, vessel assignment/berth, ship sailing, discharge, destination customs, warehouse unloading, retail delivery and designated empty return all remain possible.
6. Local trucks serve supermarket/DC/local cold rooms. Airfreight serves airport cargo and destination separately.
7. Multiple independent trucks, harvest and packing jobs, vessels and clients operate simultaneously. A single stalled actor must not freeze the network.
8. Container, truck and cargo state transitions are auditable; preserve deterministic IDs and simulation seeds unless accompanied by migration.

## Existing locations and relative placement
- Crop orchards, packhouse, bin and pallet cold rooms grouped near farm with separated farm/truck traffic.
- Empty depot close to port access; transporter dispatch yard has freight bypass access.
- Weighbridge lies on route to port and delayed holding diversion; plug-in depot needs adequate truck access and powered bays.
- Airport air-cargo zone and its cold storage must be safely accessible without vehicles cutting through terminal/public zones.
- Expand residential/commercial city on realistic street blocks; keep freight access on designated arterials, not inside home lots.
- Cape Town setting can be condensed and stylised geographically; retain recognizable landmark roles.

## Labia Theatre / Lyudmila
- Orange Court apartment doorway and theatre entrance are nodes in a dedicated pedestrian graph.
- Walking journey: leave apartment → sidewalk → zebra crossing/signal → theatre → return home. Do not use vehicle road centre lines as walking paths.
- If editor relocates theatre/flat or changes footpath, require navigation rebuild and connectivity validation before saving.
- Pedestrian timing may be simulated; avatar is a generic character, not a biometric representation.

## Deterministic layout and editor
- Assign stable IDs per lot, building, road, depot, crop block, signage and pedestrian link.
- Keep legacy layout overrides intact; migrate before changing generation order or the random seed.
- Reject placements that collide with navigation/lot envelopes, break key freight routes, block emergency access or disconnect pedestrian landmark routes.
- On road edit, pause traffic, validate updated graph, replan actors, preview and commit atomically; rollback on failure.
- Preserve valid old saves and distinguish authored layout from transient actor positions.
