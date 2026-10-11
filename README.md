# GFS Chainline

A live 3D operations view of the GFS export chain, built as a single HTML file with three.js. Open `index.html` in a browser.

## Origin side
1. Pickers fill field bins in the orchard.
2. Bin trucks take the bins to the bin cold room.
3. The packhouse tips, washes, dries, grades and packs the fruit into cartons. Pallets go to the pallet cold store.
4. The shipping line's empty depot washes and inspects (PTI) the empties.
5. Trucks from different trucking companies collect an inspected empty and load at the allocated farm.
6. Each truck crosses the weighbridge and lines up at the port gate. The reefer goes into the stack.
7. Cranes load each reefer onto its own line's vessel. Other exporters' trucks share the road, gate and stacks.
8. **Vessel delayed:** the stack closes and loaded reefers are plugged in at the staging depot. When the stack reopens, trucks collect them.

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
