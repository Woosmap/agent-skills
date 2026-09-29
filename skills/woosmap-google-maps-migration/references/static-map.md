# Static Map

- Endpoint and parameters: https://developers.woosmap.com/api-reference/static-map-api/get-maps-static.md
- Parameter mapping from Google: the "Static Maps" section of
  https://developers.woosmap.com/products/map-api/guides/migration.md
- **WebP only.** Remove any `format=` parameter, and check that `<img>`
  consumers, email templates and PDF generators accept WebP.
- **Overlay parameters are JSON, not Google's pipe syntax**, including `markers`
  (the guide's "Same syntax" row is wrong). Google `path=` → `polylines` (encoded
  polyline in `enc`), filled shapes → `polygons`, `style=` → `stylers`. Take the
  exact shape from the endpoint reference.
- **One JSON object per parameter; repeat it for more.** The reference types
  `markers`, `polylines` and `polygons` as `array`, meaning a repeated query
  parameter: `markers={...}&markers={...}`. A JSON array in one value
  (`markers=[{...}]`) returns HTTP 422. URL-encode each value.
