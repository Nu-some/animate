# World Editor v2 — Technical and UX specification

## Existing functionality
The documented E-key editor selects/moves/rotates/hides scene objects and imports/exports layout overrides. Roads, actors, ships are not directly editable. Build on this system; preserve old files and stable IDs.

## Modes
- **View**: run the logistics simulation; click to follow entities.
- **Design**: pause simulation; edit geographic definition with handles and collision/route diagnostics.
- **Preview**: resume a draft copy of the layout; accept or revert changes.

## Editing operations
Select one or multiple lots/buildings/orchard blocks/props/roads/pedestrian links; move, rotate, duplicate, hide, lock, undo/redo, snap to grid, adjust dimensions where safe, and edit attributes in a property inspector. Add camera focus, layer visibility and search. On mobile use touch handles and a bottom-sheet inspector.

## Placement and routing guarantees
Always validate polygon footprint and setback against all active road carriageways, turning envelopes, neighbouring parcels, warehouses and pedestrian zones. Show red invalid placement previews and explain WHY an edit is blocked.

Road and sidewalk edits must atomically regenerate their meshes, lane graph, vehicle permissions, junction turn paths, crosswalks and pedestrian graph. Replan moving traffic safely. Invalid edits that disconnect weighbridge, port gate, empty depot, refrigerated holding depot, airport or Lyudmila’s route cannot be saved.

## Save schema
Versioned JSON with stable ID, category, asset ID, position/rotation/scale, footprint, district, parcel, road access link and optional crop type. Support export/import, browser draft, preview, reset to published, failure rollback, unsaved-state warning and migration from legacy `LAYOUT_OVERRIDES`.

## Test stories
1. Drag a house inside its lot; no road overlap; reload retains position.
2. Move an orchard group; tractor/farm-road and packhouse connections remain valid.
3. Edit freight road; vehicle graph and collision envelopes update; truck reaches port without traversing residential roads.
4. Relocate theatre; pedestrian graph recomputes and invalid disconnected journey is rejected.
5. Mobile user selects warehouse, rotates, undoes and saves.
6. Legacy layout JSON imports without remapping objects incorrectly.

Do not treat a movable decorative mesh as sufficient for a *functional* editable road network.
