# GFS Chainline — Art direction (Style A)

## Target
A realistic, polished, playable-looking city-scale logistics simulation. Inspiration: the density, scale coherence, road readability, sunlight and urban variation of Cities: Skylines. Do not copy their proprietary buildings, textures, brands or maps. It is a Cape Town-*inspired* operational composite, not an assertion of survey-accurate street geography.

## Visual pillars
1. **Correct scale.** First audit the existing world-to-metre convention; introduce one consistent authored unit mapping. Validate truck length, width, clearance, orchard row spacing, apartment floors, container sizes, vehicle turning envelope, cranes, quays and ships.
2. **Believable materials.** Use PBR metal/rough, normal and albedo maps where visually useful. Calibrate sRGB/linear textures, roughness, exposure, colour management and tone mapping. Asphalts, wet tarmac, stucco, glass, metal containers, leaves, earth and water should not look identical.
3. **Excellent natural lighting.** Directional sun, sky fill, plausible haze, contact/depth shadowing and soft shadow edges. Distinct day, dusk and night modes; realistic urban and port practical lighting after sunset.
4. **Real city design.** Properties have setbacks, curb cuts, roofs, windows, pavement, gardens, fences, parking, streetlights, signs and coherent building heights. Avoid repetitive grey box grids; vary façade, roof, use and age by district.
5. **Readable operations.** Forklifts, bins, pallets, containers, gate queues and loading docks must be recognisable at overview and close-up. Labels and green route arrows complement physical reality rather than replace it.
6. **Cape Town character.** Mountains and a coastal harbour, urban blocks, residential streets, mixed-use commercial cores, agricultural hinterland and working industrial yards. Geography may be condensed for product usefulness.
7. **Stable performance.** Near/mid/far LOD tiers, textured instancing for repeating crops, frustum-culling and tiered lighting. Optimize from captured metrics, not speculation.

## District and asset requirements
### Agriculture
- **Citrus**: dense evergreen round canopies, orange/lemon fruit detail near camera, distinct trunk, bins and tractors.
- **Deciduous**: apple and pear blocks with branching silhouettes, seasonal leaf and fruit colours, orchard rows, bins and ladders/harvest props where appropriate.
- **Table grapes**: trained vine rows, trellis poles, wires, leafy canopy, visible bunches close-up, grading crates. Never use generic fruit-tree spheres for vineyards.
- **Packhouse**: industrial long-span roof, dock canopies, sorting/wash lines visible where feasible, fruit pallets, forklift circulation, cold room equipment, high-bay doors.

### City and people
- Townhouses, mid-rise flats, detached houses, commercial corner units, office campus, parks and urban landscaping with district variation.
- Correct separation of sidewalk, cycle lane (where drawn), travel lanes, parking and industrial access.
- **Orange Court** flat, a legible entrance and sidewalk path to a recognisable **Labia Theatre** marquee. Lyudmila's figure should be in scale, with walk animation and safe crossing logic; no personal likeness invented.

### Logistics and transport
- Transporter office with truck yard, maintenance, marked truck bays and separate pedestrian access.
- Empty-container depot near the port with wash, inspection/PTI, empty stacks and container-handling equipment.
- Weighbridge with truck-length approach and safe queuing.
- Reefer/plug-in holding depot with powered pedestals, cables, refrigerated container bays, plug state and collection/discharge lanes.
- Port gate, queue channels, designated stacks, reach-stackers, quayside cranes, vessel berth and maritime navigation.
- Airport cargo terminal, cold-chain ULD staging, loading equipment, apron, taxi/runway separations.

## Asset authoring contract
Preferred runtime model: glTF 2.0 GLB. Record in an asset manifest: id, path, source URL, author, license, intended scale (m), bounds, collision shape, category, textures, LOD variants, mobile fallback. Keep a separate credits/attribution manifest for non-CC0 assets.

Avoid unlicensed game packs, image-generated 'models' that are only flat pictures, huge unoptimized textures, and geometry with unknown origin or scale.

## Rendering and review
- Use measured budgets on target hardware, not a universal invented polygon count.
- Target smooth interaction and reduced-cost presets on phones; progressive asset loading is preferred.
- Curated review captures: overview, street eye-level, farms, packhouse, truck yard, port, airport and theatre at daylight, dusk/night, desktop and mobile widths.
- A scene is not approved just because it contains more assets: appearance, layout, scale, legibility, speed and collision must all improve.
