# Map init, markers, bounds, LatLng, Data layer

Source of truth: https://developers.woosmap.com/products/map-api/guides/migration.md
and https://developers.woosmap.com/products/map-api/reference/1.4.md
Take the script tag, key and map init from the guide's "Script Tag & Map Init"
section. Don't write them from memory.

## LatLngBounds: argument order

Woosmap `LatLngBounds(northEast, southWest)`, Google `LatLngBounds(sw, ne)`.

```js
// Google
new google.maps.LatLngBounds(sw, ne);
// Woosmap
new woosmap.map.LatLngBounds(ne, sw);
```

Also check helpers that build bounds from arrays or positional arguments.

## LatLng: keep the parentheses

Reference `#woosmap.map.LatLng`: `lat: () => number`, `lng: () => number`.
`LatLngLiteral` (`{ lat, lng }`) is still a plain object.

## Data layer: bulk-load, don't build per-feature objects

- Load features in bulk with `addGeoJson(geojson)` on `map.data` or a `new woosmap.map.Data()`.
  Don't create one `woosmap.map.Polygon` per feature.
- Style with `setStyle(fn)`; it accepts a style object or a per-feature function.
- Clear a layer with `data.setMap(null)`. Don't loop over features or overlays.
- Use a separate `Data` instance for each layer you toggle independently.
- `addGeoJson` returns nothing (Google's returns the added features). Use `forEach` to reach them.

## Markers

`AdvancedMarkerElement` doesn't exist. Use `Marker` with an image `icon` and optional `label`.
For custom HTML content, extend `woosmap.map.OverlayView` (`onAdd`, `draw`, `onRemove`).
Sample: https://developers.woosmap.com/js-samples/marker-html-css.md
