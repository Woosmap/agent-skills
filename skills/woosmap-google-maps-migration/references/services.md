# DirectionsService, autocomplete, geocoding, distance

## DirectionsService

Reference: `#woosmap.map.DirectionsService__route`
Signature: `route(request, callback)`, where the callback is `(result, status) => void`.
It has no Promise form. Take the callback example and the Promise wrapper for `await` code
from the "JS SDK" section of the
[Distance API migration](https://developers.woosmap.com/products/distance-api/guides/migration.md).
Draw the result with `DirectionsRenderer.setDirections(result)`.

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
