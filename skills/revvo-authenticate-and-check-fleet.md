---
name: Authenticate and check fleet tire status
description: Exchange a Revvo fleet API key for a JWT and read the current tire status of a fleet.
api: openapi/revvo-api-openapi-original.yml
operations: [authToken, getFleetStatus]
---

# Authenticate and check fleet tire status

Use this to obtain a Revvo session token and read a fleet's current tire status.

## Steps

1. **Get a JWT** — call `authToken` (`POST /auth`) with the fleet API key in the
   `X-API-KEY` header. The response body (`text/plain`) is the JWT. A `401` means
   the API key is missing or invalid.
2. **Read fleet status** — call `getFleetStatus` (`GET /fleet/{fleetId}`) with
   `Authorization: Bearer <jwt>`. Returns the fleet's vehicles with per-tire
   pressure, temperature, tread class and open/resolved events.

## Rules

- The JWT is short-lived; re-run step 1 on a `401` rather than reusing a stale token.
- Every operation is scoped to `{fleetId}` — you can only read fleets your key covers.
- On error, read the JSON envelope: `success: false`, `displayErrorMessage`,
  `internalErrorCode`.
