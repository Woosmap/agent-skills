---
name: woosmap-google-maps-migration
description: Migrate Google Maps Platform code to Woosmap. Use whenever the user has existing google.maps.* JavaScript (Map, Marker, LatLngBounds, DirectionsService, Data layer, InfoWindow), Places Autocomplete, Geocoding, Distance Matrix, Directions or Static Maps code, and wants to port, migrate, switch, replace or "move off" Google Maps to Woosmap, even if they only paste Google code and mention Woosmap once. Also use for questions like "what is the Woosmap equivalent of X in Google Maps". Not for new Woosmap projects that have no Google Maps code; use the regular Woosmap docs for those.
---

# Woosmap Google Maps Migration

This skill is intentionally thin. The Woosmap docs are the source of truth, and
this file only points you to them and covers the few pitfalls the docs don't
make obvious. Read the docs before you write code: parameter names and
defaults change, and whatever you remember may be out of date.

## Step 0: load the rules and the docs index

1. Fetch `https://developers.woosmap.com/llms-rules.txt` and follow it.
2. Use `https://developers.woosmap.com/llms.txt` to find the pages you need.
   Every docs page has a Markdown twin: add `.md` to its URL. Prefer the twin.
   It is smaller and easier to parse than the HTML page.
3. For Map JS API signatures, use the reference:
   `https://developers.woosmap.com/products/map-api/reference/1.4/`
   (anchors look like `#woosmap.map.LatLngBounds`).

## Step 1: check what Woosmap doesn't cover

Before mapping any Google feature, read
[What Woosmap doesn't cover](https://developers.woosmap.com/products/map-api/guides/migration/#what-woosmap-doesnt-cover).
If the code uses one of those features, tell the user up front and suggest the
alternative that section lists. Don't make up a shim. This skill deliberately
doesn't copy that list, so it can't drift from the docs.

## Step 2: Key differences

These are the points agents get wrong most often. Details are in `references/`.

- **Namespace:** `google.maps.*` → `woosmap.map.*`. Also swap the script tag and
  key. See the migration guide's "Script Tag & Map Init" section.
- **`LatLngBounds(ne, sw)`:** Woosmap takes **north-east first**. Google takes
  `(sw, ne)`. Swap the arguments; don't just rename the class.
- **`lat()` / `lng()` stay methods:** the reference defines them as
  `() => number`, so keep the parentheses. The Map API and Localities migration
  guides currently say the opposite; follow the reference.
- **`DirectionsService.route(request, callback)`:** this is a callback
  `(result, status)`, not a Promise. Rewrite `await`/`.then()` Google code into
  a callback (or wrap it in your own Promise). Other services return Promises;
  check each one in the reference.
- **Static Map is WebP only:** Woosmap has no `format` parameter, so drop it and
  make sure anything that consumes the image accepts WebP. Overlay parameters
  (`markers`, `polylines`, `polygons`, `stylers`) take JSON, not Google's pipe
  syntax.
- **Data layer performance:** don't create one `woosmap.map.Polygon` per feature.
  Load features in bulk with `addGeoJson` on a `woosmap.map.Data` instance, and
  clear a layer with `data.setMap(null)` instead of removing features in a loop.
  Use a separate `Data` instance for each layer you toggle independently.

## Step 3: REST and web services (links only)

Don't build your own parameter mapping tables. Follow the guides:

- Overall: [Migration from Google Maps](https://developers.woosmap.com/products/migration-guides/migration-from-google-maps/)
- Places Autocomplete / Geocoding / Place Details → [Localities migration](https://developers.woosmap.com/products/localities/guides/migration/)
- Distance Matrix / Directions (REST) → [Distance API migration](https://developers.woosmap.com/products/distance-api/guides/migration/)

## Step 4: finish

- Run or build the migrated code if the project allows it, and check the
  browser console.
- Give the user a short list of what changed, and flag any unsupported feature
  from Step 1 that still needs a decision.

## Reference files (load only when the task needs that topic)

| File | Load when the code uses |
|---|---|
| `references/map-js.md` | Map init, markers, bounds, LatLng, Data layer |
| `references/services.md` | DirectionsService, autocomplete, geocoding, distance |
| `references/static-map.md` | Static Maps URLs |
