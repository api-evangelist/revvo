---
name: Onboard a vehicle and its tire sensors
description: Add a vehicle to a Revvo fleet, then register its gateway and TPMS sensors so tires are monitored.
api: openapi/revvo-api-openapi-original.yml
operations: [authToken, vehicle, registerDevicesForVehicle, registerSensor, getVehicle]
---

# Onboard a vehicle and its tire sensors

Use this to bring a new vehicle under Revvo tire monitoring.

## Steps

1. **Authenticate** — `authToken` (`POST /auth`) with `X-API-KEY` to get a JWT;
   send `Authorization: Bearer <jwt>` on every following call.
2. **Create the vehicle** — `vehicle` (`POST /fleet/{fleetId}/vehicle`) with
   `vin`, `assetId`, and an axle `template`. Returns `201` on success.
3. **Register the gateway + sensors** — `registerDevicesForVehicle`
   (`POST /fleet/{fleetId}/devices/register`) with `assetId`,
   `gatewayMacAddress`, `gatewayInstallLocation`, and the
   `tirePositionToSensorMacAddress` map. To add sensors individually use
   `registerSensor` (`POST /fleet/{fleetId}/sensor/register`) with `sensorId`,
   `assetId`, `tirePosition`.
4. **Verify** — `getVehicle` (`GET /fleet/{fleetId}/{vin}`) and confirm tires and
   sensors are attached.

## Rules

- All operations are fleet-scoped via `{fleetId}`.
- A `400` means a malformed body (check `displayErrorMessage`/`internalErrorCode`);
  `404` means the fleet or asset does not exist.
- `tirePosition` binds a sensor to a specific tire on the axle template.
