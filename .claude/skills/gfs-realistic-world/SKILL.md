---
name: gfs-realistic-world
description: Audit, art-direct, implement, and test realistic Cities Skylines-inspired 3D environments for GFS Chainline. Use for farms and orchards, Cape Town buildings and streets, logistics roads, the port, airfreight, lighting and PBR models, visual regression, Lyudmila's walk to the Labia Theatre, rendering performance, and the existing world layout editor. Preserve all live simulation behavior.
---

# GFS Chainline — Realistic World Designer

## Mission
Make the interactive Three.js GFS Chainline world visually believable at city-building-game scale (reference: Cities: Skylines visual style A). Prioritize realistic geometry, credible scale, PBR materials, sunlight, architecture, terrain, vegetation, and logical roads. Retain an approachable bird's-eye view and the dark-teal/green Chainline dashboard. Use ORIGINAL or appropriately licensed assets, never extracted game artwork.

## Project realities to verify
- Public repository is Nu-some/animate; app documentation describes a single-file Three.js site at index.html with GitHub Pages publishing.
- The repository README describes the existing editor (E), deterministic layout overrides, citrus/apples/trellised grapes, Labia Theatre and Lyudmila's Orange Court walking route.
- It describes shortest-route vehicle traffic, vessels, airfreight, destinations, cold-chain work, delayed-vessel plug-in diversions, and multiple simultaneous jobs.
- All statements above must be independently checked against CURRENT source. Do not assume README is runtime proof.

## Required references (read first)
1. references/ART_DIRECTION.md — the visual target and asset standards.
2. references/WORLD_RULES.md — zoning, vehicle/pedestrian navigation and operational constraints.
3. references/QA_GATES.md — validation and required evidence.
4. docs/IMPLEMENTATION_PLAN.md, docs/WORLD_EDITOR_SPEC.md and docs/CLAUDE_CODE_START.md from repo root when present.
5. Repository README, application code, publishing workflow and current branch.

## First invocation: AUDIT ONLY, unless user explicitly authorizes implementation
1. Determine current branch, site publishing triggers, file structure, dependencies, build scripts, scene entry points, object models, road graph, collision and editor persistence.
2. Start an isolated preview WITHOUT changing production branches. Record what can and cannot be executed in the cloud session.
3. Capture baseline overview, orchard/packhouse, residential road, transporter yard, empty depot, plug-in depot, port, airport, Labia Theatre pedestrian trip; capture desktop and phone widths in daylight and night settings.
4. Collect reproducible defects: overlapping houses/roads, trailers clipping corners, floating objects, missing access lanes, visible low-detail boxes, geometry z-fighting, mis-scaled assets, texture and lighting issues.
5. Inspect actor routes and simulation invariants; find layout save object identifiers and migration risks.
6. Write a Phase 0 findings report with severity, screenshots, benchmarks, prioritized fixes, and a **small showcase district**. Do not describe unrun tests as passed.

## Implement safely in small steps
- Work on a development branch/PR. Preserve all existing local, export, delayed export, empty return, destination, and air cargo operations.
- Before replacing art, establish asset and material standards and one approved district as a quality bar.
- Change one module at a time. Keep stable IDs and legacy JSON import. Every moving vehicle must use the same routable roads visible in the scene.
- Extend the current E editor instead of inventing a separate one. Paused-edit mode must validate collisions and graph connectivity.
- For repeating vegetation use InstancedMesh and LOD. For detailed architecture use GLB/glTF with licensed assets, material maps and performance-aware budgets. Do not rely on primitive shapes as FINAL realistic buildings.
- Screenshot-review each iteration. Check routing, loading, queues, crossings, editor import/export, simulation state, console errors, mobile FPS, memory and Pages path handling.
- Produce before/after evidence and explain unmet acceptance checks before merging. No production publish until approved.

## Mandatory world commitments
- Different-looking citrus, deciduous apple/pear orchards, and trellised table grape vineyards. Keep packing and cold rooms near farms.
- Truck-compatible freight bypass/ring road; no truck travel through residential lots, structures or pavement; safe junction turning radii and queue lanes.
- Transporter yard, empty depot near port, truck weighbridge, reefer power/plug-in depot, port stacks/gate/berth, airport cargo depot; each has functional access.
- Cape Town-inspired expanded urban districts, streets, pavements, parking, architectural variation, terrain and a coherent coastline.
- Labia Theatre in the city; Lyudmila walks Orange Court flat → sidewalk → controlled crossing → cinema → home. Pedestrian graph must remain connected.
- Preserve origin/destination selections, vessel assignments, parallel harvest/packing/transport, customs and warehouse handoff and empty returns.

## Response required after every coding session
**Scope and branch** | **Changes and preserved behavior** | **Before/after screenshots** | **Tests actually run (pass/fail/blocked)** | **Performance/environment** | **Remaining defects and next step**.

Never claim a full debug or runtime test from README review alone. If live simulation, WebGL screenshots or mobile checks are unavailable, explicitly mark them as blocked.
