# GFS Chainline

A live 3D operations view of the GFS export chain, built as a single HTML file with three.js. Open `index.html` in a browser.

**Website:** every push publishes the page to GitHub Pages at `https://nu-some.github.io/animate/` (see `.github/workflows/pages.yml`). GitHub Pages must be enabled for the repository under Settings → Pages → Source: GitHub Actions.

A guided, plain-language tour exists but is tucked away: press **T** on the keyboard, or use the small "Guided tour" link at the end of the About panel (the ⓘ button).

## Scroll story (the landing page)
The page opens as a scrolling story, in the style of emons.de: the live city map stays fixed in the background and each scroll step flies the camera from the big picture into one operation, in order:

picking → bins to the bin cold room → washing, grading and packing → storing at +4 °C → preparing the empty container (PTI) → truck dispatched → loading the back of the truck → through the city → weighbridge (VGM) → container off-loaded into the port stack → plug-in depot when the vessel is late → loaded onto the vessel → crossing the sea → container discharged → customs → pallets into the client's cold room → empty container picked up → road and air routes.

Between two distant steps the camera lifts back out towards the city map before dropping into the next one. On steps inside a building (bin cold room, packhouse, pallet cold store, client warehouse) the roof lifts off and fades away so you look inside. The side rail jumps to any step; arrow keys, Page Up/Down and Space step through. **Explore the live map** (or `index.html?explore`) opens the free interactive view with all panels.

## Every port has its own look
Pick an **Origin** and a **Destination** in the top bar. The landscape and landmarks change with each port:

| Loading ports | | Destination ports | |
|---|---|---|---|
| Cape Town | Table Mountain, Lion's Head, stadium, red-and-white lighthouse, fynbos | Kribi | Rainforest hills, Lobé falls into the sea, pirogues |
| Port Elizabeth | Campanile, Donkin pyramid and lighthouse, stadium, aloes | Douala | Wouri bridge, mangroves, tin-roof town |
| Durban | Moses Mabhida arch, Golden Mile towers, beach, green hills, palms | Apapa (Lagos) | Lagos skyline, Third Mainland Bridge, tank farm, danfo buses |
| Port Said | Suez Canal with passing ships, Canal Authority domes, lighthouse, dunes | Libreville | Seafront palace, rainforest, palm beaches |
| Damietta | Nile Delta fields, palm groves, mosque and minarets | Cotonou | Amazon statue, Ganvié stilt village, red earth |
| Port-Vendres | Snow-capped Pyrenees, vineyard terraces, pastel houses, obelisk, fort | Pointe des Galets | Smoking volcano, cirque peaks, sugar cane |
| Rotterdam | Erasmus Bridge, Euromast, cube houses, windmills, greenhouses, canals | Dakar | African Renaissance Monument, Mamelles lighthouse, Gorée, baobabs |
| | | Conakry | Grand Mosque, Îles de Los |
| | | Lomé | Independence Monument, palm beach |
| | | Port Louis | Pieter Both, Le Pouce, colourful waterfront, cane |
| | | Longoni | Turquoise lagoon, volcanic hills, mangroves, baobabs |
| | | Abidjan | Plateau towers, La Pyramide, St Paul's Cathedral, lagoon bridge |
| | | Jebel Ali | Burj Khalifa, Burj Al Arab, dunes |
| | | Mombasa | The Tusks, Fort Jesus, dhows, baobabs |

## One map, one road network
The whole world is one continuous map. From west to east: the inland town, the farm, the city, the port; the airport on the south coast; Bay City on its own island across the bay, reached over a big cable-stayed bridge; the forest town at the end of a road through dense forest north of the inland town; and the destination port across the sea to the east. The GFS freighter flies between the main airport and the Bay City airstrip. A main road links the town, the farm and the city, and the city streets lead to the weighbridge and the port gate. The farm is further from the port than the city, so every loaded truck drives through the city to reach the port.

Vehicles do not run on fixed loops. Each truck plans a trip on the road network (shortest route, keep-left lanes, give-way behind the vehicle ahead):
- **GFS reefer trucks** start at the transporter yard, collect an inspected empty at the depot by the port, drive out to the farm, load, come back through the city, cross the weighbridge, and either queue at the port gate and drop in the stack or, when the vessel is delayed, turn off to the plug-in depot in town. Then they return to the yard and wait for the next job.
- **Other exporters' trucks** come in from the inland packhouse in the next town and share the weighbridge, gate and stacks.
- **Local market trucks** take pallets from the cold store to the supermarket distribution centre in the city.
- **Long-distance trucks** drive from the farm along the road that links the towns: one to the client depot in the inland town, one over the bridge to the client depot in Bay City. The local truck alternates between the supermarket DC and the forest town's country store.
- **The air-freight truck** shuttles between the cold store and the air cargo terminal, and cars wander the streets.

Ships queue as well: while one vessel is alongside, the next waits at anchor. When a loaded vessel sails it crosses the sea on the map to the port of discharge.

## Audit
A full technical and visual audit with the plan that was carried out is in `docs/AUDIT.md`.

## Layout editor
Press **E** to open the layout editor. Click any building, shed, tree clump or prop to pick it, drag it to move it, press R to rotate, the arrow keys to nudge and Delete to hide it. Roads, trucks and ships are driven by the simulation and cannot be moved. Changes are saved in your browser and re-applied on every visit; **Export JSON** downloads them (and copies them to the clipboard) so they can be baked into `index.html` as `LAYOUT_OVERRIDES`, and **Import** loads such a file. The world is built with a fixed random seed so every object lands in the same place on every visit, which is what makes the saved positions stick.

Buildings placed automatically along the streets are checked against every road corridor and dropped if they would stand on the road.

## Origin side
1. Pickers fill field bins in the orchard. The farm has three blocks: citrus, apples and trellised table grapes, and containers carry what the origin grows.
2. Bin trucks take the bins to the bin cold room.
3. The packhouse tips, washes, dries, grades and packs the fruit into cartons. Pallets go to the pallet cold store.
4. The shipping line's empty depot washes and inspects (PTI) the empties.
5. Trucks from different trucking companies collect an inspected empty and load at the allocated farm.
6. Each truck crosses the weighbridge and lines up at the port gate. The reefer goes into the stack.
7. Cranes load each reefer onto its own line's vessel. Other exporters' trucks share the road, gate and stacks.
8. **Vessel delayed:** the stack closes and loaded reefers are plugged in at the separate plug-in depot in town. When the stack reopens, trucks are dispatched to collect them.

## Around the chain
- **Air freight terminal:** a GFS air-freight truck brings pallets from the pallet cold store. They are built into ULDs and a freighter takes off and lands again.
- **GFS HQ:** operations, planning and control.
- **Transporter yard:** transport partners and dispatch. Every truck starts and ends its job here.
- **The Labia Theatre:** the old independent cinema on the city's new north blocks. Lyudmila walks from her flat at Orange Court, over the zebra crossing, to the evening show and home again after the credits. Click her or the theatre.
- **Supermarket DC and inland town:** the local market and the client depot at the far end of the road.

## Network
- Lanes and carriers come from the FREIGHT 2025 Smartsheet (Cape Town, Port Elizabeth, Durban).
- Port Said, Damietta, Port-Vendres, Rotterdam, Conakry and Longoni were added by hand.
- Freight rates are deliberately not included.

## Destination side
1. The vessel discharges into the stack.
2. The reefer goes through customs inspection.
3. A client-hired truck takes it to the client's warehouse, where a forklift unloads the pallets.
4. The empty goes back to the shipping line's depot.

All trucks, containers and timings are simulated. Container numbers use valid ISO 6346 check digits.

## Credits
- Buildings, cars, streetlights, traffic lights and street props: **KayKit City Builder Bits** by Kay Lousberg (kaylousberg.com). The licence is CC0, so commercial use is allowed. The models are embedded in `index.html`.
- Trucks, containers, cranes, vessels, aircraft and all logistics equipment are modelled in code.

## Web addresses

| Address | What it needs |
|---|---|
| `https://nu-some.github.io/animate/` | Enable GitHub Pages on this repo: Settings → Pages → Source: **GitHub Actions**, then run the **Publish website** workflow once. Free. |
| `https://nu-some.github.io/` (shorter) | Create a public repo named exactly `Nu-some.github.io` and put `index.html` in it. GitHub publishes it automatically. Free. |
| `https://nusome.io` (your own domain) | 1. Buy `nusome.io` from a registrar (Cloudflare, Namecheap, GoDaddy; a `.io` costs about US$35–60 a year). 2. At the registrar add DNS records: four `A` records for `@` → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`, and a `CNAME` for `www` → `nu-some.github.io`. 3. Add a file named `CNAME` containing `nusome.io` to the site repo, and set the custom domain under Settings → Pages. 4. Tick **Enforce HTTPS** once the certificate is issued (up to a day). |
