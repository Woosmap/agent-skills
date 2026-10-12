---
name: woosmap-store-locator
description: Build a store locator with Woosmap. Load stores, agencies, dealers or pickup points into Woosmap, show them on a map, find the ones nearest to an address or to the user's position, and list them, with the Store Locator Widget or the Map JS API. Use whenever the user wants to display or search their own locations with Woosmap (store locator, store finder, dealer locator, "find a store near me", nearest branch), including the script that imports the locations. Not for migrating existing Google Maps code; use woosmap-google-maps-migration for that.
---

# Woosmap Store Locator

Check the Woosmap docs before writing code, not your memory of the API. Every
docs page has a Markdown twin: add `.md` to its URL.

## Step 1: Choose the widget or the Map JS API

- **Store Locator Widget** for a store finder page: search box, results list,
  store details and directions come built in and are configured rather than
  coded. It is the fastest route, and the docs recommend it for this case.
- **Map JS API** when the user describes their own layout or interactions,
  the locator lives inside an existing app or framework, or the page mixes
  stores with other Woosmap services.

When the request only asks for a store locator page, use the widget. Say in
one line which one you chose and why.

## Step 2: Read the docs, in this order (once per conversation)

Skip a page you already fetched earlier in this conversation.

1. https://developers.woosmap.com/llms-rules.txt and follow it.
2. Loading the stores:
   https://developers.woosmap.com/products/stores-api/guides/importing-store-data.md,
   then the asset schema in
   https://developers.woosmap.com/api-reference/data-api/post-stores-replace.md
3. Widget:
   https://developers.woosmap.com/products/widgets/store-locator-widget/quick-start.md,
   https://developers.woosmap.com/products/widgets/store-locator-widget/full-configuration.md,
   and https://developers.woosmap.com/products/widgets/store-locator-widget/store-locator-widget-reference.md
   for every option.
4. Map JS:
   - Drawing the stores: https://developers.woosmap.com/products/map-api/guides/display-stores-overlay.md
   - From an address to the nearest stores: https://developers.woosmap.com/js-samples/stores-search.md
   - Address suggestions: https://developers.woosmap.com/api-reference/localities-api/get-localities-autocomplete.md.
     Street addresses only come back when `types` includes `address`.
   - Signatures of `StoresOverlay`, `StoresService` and `LocalitiesService`:
     https://developers.woosmap.com/products/map-api/reference/1.4.md

## Step 3: What the docs don't say

Either way, the import runs server-side with the private key, and the page
uses only the public `woos-` key, restricted to the site's domains.

With the widget:

- **It runs on Google unless told otherwise.** Set `maps.provider: "woosmap"`,
  `datasource.distanceMatrixProvider: "woosmap"` and
  `datasource.routeProvider: "woosmap"`. Without them the widget loads Google
  Maps and calls Google's Distance Matrix and Directions, which need a Google
  key.
- **Set `datasource.maxResponses` explicitly.** The quick start, the
  configuration guide and the reference disagree on its default and range.
- Address search is `maps.localities`: include `address` in `types`. The
  widget filters countries with `componentRestrictions`, unlike
  `LocalitiesService`.
- `new WebApp(elementId, publicKey)`, then `setConf(config)`, then `render()`.

With the Map JS API:

- **Draw the stores with one `StoresOverlay`.** It renders every store in the
  project from Woosmap's tiles, whatever the size of the network. Don't load
  the stores into the page to draw them, and don't create a `Marker` per
  store. Keep `Marker` for the searched address or the user's position.
- **Let the search rank the stores.** For the N nearest, call
  `StoresService.search({ latLng, storesByPage: N })` (REST: `lat`, `lng`,
  `stores_by_page`). Results come sorted by distance, with
  `properties.distance` in metres. Don't page through every store and sort
  them in the browser. The `stores-list-sync-map` sample loads every store
  because its list shows the whole network; follow it only when the page
  must list them all.
- **Highlight with the overlay.** When the user picks a store in the list,
  pass its feature to `storesOverlay.setSelection(feature)`. Feature
  coordinates are `[lng, lat]`.

## Step 4: Finish

End with a short "Still to check" list: the public key's allowed domains, the
private key's write permission, and any column of the source data that could
not be mapped to the asset schema.
