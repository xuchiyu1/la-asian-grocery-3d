# Where to Find Asian Groceries in Los Angeles (3D)

**Live map:** https://xuchiyu1.github.io/la-asian-grocery-3d/
SSCI 591 Web and Mobile GIS, Project 3 (Fall 2026) by Chiyu (Leo) Xu

## a. What this map shows

This 3D web map shows the major Asian supermarket chains in Los Angeles County, such as 99 Ranch, 168 Market, H Mart, Galleria Market, Tokyo Central, Seafood City, and Island Pacific. Each store is a colored pin grouped by cuisine (Chinese/Taiwanese, Korean, Japanese, Filipino, Southeast Asian, South Asian). LA Metro rail stations are shown as small gray squares so you can see which stores are reachable by train. Clicking a pin opens a popup with the store's address, city, ZIP code, and USDA store type. This is a first step toward my larger question: how far you have to travel from any point in LA to reach a real Asian grocery store.

## b. Starting sample

[Using callout lines with labels](https://developers.arcgis.com/javascript/latest/sample-code/visualization-label-callout/), ArcGIS Maps SDK for JavaScript 5.1. I picked it because it already uses the `<arcgis-scene>` component, a 3D FeatureLayer, icon symbols, and callout lines, which are exactly the pieces I needed for points in 3D.

## c. What I changed

- Replaced the sample's web scene item with `basemap="dark-gray-vector"` and `ground="world-elevation"`.
- Replaced the Swiss mountain peaks layer with live LA data (see section d).
- Changed the camera so the map opens over the LA basin in a bird's-eye view, and pointed the Home button at that camera.
- Replaced the single black circle with a unique-value renderer: one color per cuisine, with each pin lifted off the ground on a white callout line. The cuisine group is computed from the store name with an Arcade expression, which the renderer, legend, and popup all reuse.
- Added a popup with five fields, labels that only appear when zoomed in, a page title, a header with an "About this map" paragraph, a live store-count chip, and a footer with data credits. I also moved the navigation tools to the right so the legend does not cover them.

## d. Data source

- **USDA SNAP Retailer Locations** (USDA Food and Nutrition Administration), `https://services1.arcgis.com/RLQu0rK7h4kbsBq5/arcgis/rest/services/snap_retailer_location_data/FeatureServer/0`. Loaded with a **FeatureLayer** (Option 1). A `definitionExpression` (SQL `where`) keeps only stores in Los Angeles County whose names match the Asian chains, so the server sends about 60 stores instead of 250,000. The service is updated every two weeks, so new or closed stores appear without changing my code.
- **LA Metro rail stations** (LA GeoHub), `https://services5.arcgis.com/7nsPwEMP38bSkCjy/arcgis/rest/services/RAIL_BRT/FeatureServer/0`, also a FeatureLayer, used as context.

## e. Component I added

A **Legend** (`<arcgis-legend>`), which reads the cuisine colors directly from the renderer, plus a **Home** button (`<arcgis-home>`) that returns to the starting view.

## f. Problems and what I'd improve next

I first tried to pull stores from OpenStreetMap through the Overpass API, but the public server was overloaded and timed out, which would be risky for a page that has to load reliably, so I switched to the USDA service. The camera initially did not move to LA because my camera position had no spatial reference; adding `wkid: 4326` fixed it. The data has limits: only SNAP-authorized stores appear, so chains like Mitsuwa and Nijiya are missing, and the cuisine groups come from store names, not an official category. Next I would add drive-time areas around each store to show which neighborhoods are far from any Asian grocery.

## g. Credits

Data: USDA FNA SNAP Retailer Locator (public domain) and LA Metro via LA GeoHub. Code based on Esri's sample above. AI tools: I used Claude (Anthropic) to help find the USDA data service, write and debug the JavaScript, and draft this README. I reviewed and tested every part and can explain each line of the code.
