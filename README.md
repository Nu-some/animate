# GFS Chainline

A live 3D operations view of the GFS export chain, built as a single HTML file with three.js. Open `index.html` in a browser.

Press **How GFS works** for a guided, plain-language tour through every step of the job.

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

## Origin side
1. Pickers fill field bins in the orchard.
2. Bin trucks take the bins to the bin cold room.
3. The packhouse tips, washes, dries, grades and packs the fruit into cartons. Pallets go to the pallet cold store.
4. The shipping line's empty depot washes and inspects (PTI) the empties.
5. Trucks from different trucking companies collect an inspected empty and load at the allocated farm.
6. Each truck crosses the weighbridge and lines up at the port gate. The reefer goes into the stack.
7. Cranes load each reefer onto its own line's vessel. Other exporters' trucks share the road, gate and stacks.
8. **Vessel delayed:** the stack closes and loaded reefers are plugged in at the staging depot. When the stack reopens, trucks collect them.

## Around the chain
- **Air freight terminal:** a GFS air-freight truck brings pallets from the pallet cold store. They are built into ULDs and a freighter takes off and lands again.
- **GFS HQ:** operations, planning and control.
- **Transporter office:** transport partners and dispatch.

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
