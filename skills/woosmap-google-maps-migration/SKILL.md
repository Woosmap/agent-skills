---
name: woosmap-google-maps-migration
description: Migrate Google Maps Platform code to Woosmap. Use whenever the user has existing google.maps.* JavaScript (Map, Marker, LatLngBounds, DirectionsService, Data layer, InfoWindow), Places Autocomplete, Geocoding, Distance Matrix, Directions or Static Maps code, and wants to port, migrate, switch, replace or "move off" Google Maps to Woosmap, even if they only paste Google code and mention Woosmap once. Also use for questions like "what is the Woosmap equivalent of X in Google Maps". Not for new Woosmap projects that have no Google Maps code; use the regular Woosmap docs for those.
---

# Woosmap Google Maps Migration

Check the Woosmap docs before writing code, not your memory of the API. Where a
migration guide and the API reference disagree, follow the reference.

## Step 1: Read the docs, in this order (once per conversation)

Skip a page you already fetched earlier in this conversation.

1. https://developers.woosmap.com/llms-rules.txt and follow it.
2. https://developers.woosmap.com/products/map-api/guides/migration.md for the
   Map JS API and Static Maps.
3. The guide for each other Google product in the code:
   - Places Autocomplete, Geocoding, Place Details →
     https://developers.woosmap.com/products/localities/guides/migration.md
   - Directions, Distance Matrix (JS SDK or REST) →
     https://developers.woosmap.com/products/distance-api/guides/migration.md
   - Android, iOS, Flutter, React Native → the "Mobile SDKs" table in
     https://developers.woosmap.com/products/migration-guides/migration-from-google-maps.md
4. https://developers.woosmap.com/products/map-api/reference/1.4.md for any
   class, method or event signature (search it for the class name).

Don't build your own parameter mapping tables. The guides hold them.

## Step 2: Check what Woosmap doesn't cover

Before mapping any Google feature, read the "What Woosmap Doesn't Cover" section
of the Map API migration guide. If the code uses one of those features, tell the
user up front and suggest the alternative that section lists. Don't make up a
shim. Do this once per conversation, unless the user brings in a new Google
feature.

## Step 3: What the guides don't say

- **Data layer:** don't create one `woosmap.map.Polygon` per feature. Load
  features in bulk with `addGeoJson` on a `woosmap.map.Data` instance, style
  with `setStyle(fn)`, and clear a layer with `data.setMap(null)` instead of
  removing features in a loop. Use a separate `Data` instance for each layer
  you toggle independently. Put per-feature data in the GeoJSON `properties`
  before loading: the return value of `addGeoJson` is unreliable (typed
  `void`), and `Data.forEach` visits every feature in the layer, not only the
  ones just added.
- **Static Map `stylers`:** takes JSON, like the `markers`, `polylines` and
  `polygons` parameters the guide describes.
- **`DirectionStatus` has one member, `OK`** (it sits near the end of the 1.4
  reference). Treat any other status as a failure. Drop Google's
  `ZERO_RESULTS` branch instead of porting it.

## Step 4: Finish

- Run or build the migrated code if the project allows it, and check the
  browser console.
- End every answer with a short "What changed" list, even when you only return
  a snippet: one line per Woosmap difference you applied, plus any unsupported
  feature from Step 2 that still needs a decision.
