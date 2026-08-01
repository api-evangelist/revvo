---
name: Pull tire events and alerts for a fleet
description: Retrieve Revvo tire events (pressure, temperature, puncture, vehicle-off) for a fleet over a time window.
api: openapi/revvo-api-openapi-original.yml
operations: [authToken, getEvents]
---

# Pull tire events and alerts for a fleet

Use this to poll Revvo tire alerts into another system.

## Steps

1. **Authenticate** — `authToken` (`POST /auth`) with `X-API-KEY` to get a JWT.
2. **List events** — `getEvents` (`GET /fleet/{fleetId}/events`) with
   `Authorization: Bearer <jwt>` and `fromTimestamp` / `toTimestamp` query
   params bounding the window. Each event carries `vin`, `tirePosition`,
   `eventTime`, `type`, and `resolutionTime`.

## Rules

- There is no cursor pagination — page by advancing the time window; keep windows
  small enough to avoid large payloads.
- Track the last `fromTimestamp` you polled so windows do not overlap or gap.
- A `type` with no `resolutionTime` is an open alert; a set `resolutionTime`
  means it has cleared.
