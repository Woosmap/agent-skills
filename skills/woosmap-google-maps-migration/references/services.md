# DirectionsService, autocomplete, geocoding, distance

## DirectionsService

Reference: `#woosmap.map.DirectionsService__route`
Signature: `route(request, callback)`, where the callback is `(result, status) => void`.

```js
// Google (Promise)
const res = await directionsService.route(req);
// Woosmap (callback); draw the result with DirectionsRenderer
const renderer = new woosmap.map.DirectionsRenderer({ map });
directionsService.route(req, (result, status) => {
  if (status !== "OK") { /* handle */ return; }
  renderer.setDirections(result);
});
```

If the calling code needs `await`, wrap the call in a Promise:

```js
const routeAsync = (req) =>
  new Promise((resolve, reject) =>
    directionsService.route(req, (result, status) =>
      status === "OK" ? resolve(result) : reject(new Error(status))));
```

`DirectionStatus` only documents `OK` (`woosmap.map.DirectionStatus.OK`); treat any other status as a failure.
Don't match on Google's `ZERO_RESULTS`: Woosmap failures arrive as other status strings
(for example `MAX_ROUTE_LENGTH_EXCEEDED`).

## Other services

`DistanceService`, `LocalitiesService`, `StoresService` and `DatasetsService` return Promises.
Note the reverse of Directions: Google's Places `AutocompleteService` / `PlacesService` use
callbacks, while Woosmap's `LocalitiesService` returns a Promise.

## Autocomplete / geocoding / place details

→ Localities API: https://developers.woosmap.com/products/localities/guides/migration.md

## Distance Matrix / Directions REST

→ Distance API: https://developers.woosmap.com/products/distance-api/guides/migration.md
