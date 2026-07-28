---
name: OpenTravel 2018A hospitality facility availability
description: >-
  Search hospitality facilities and find or retrieve their availability against a system that
  implements the OpenTravel 2018A hospitality FacilityResource contract.
api: openapi/opentravel-2018a-facility-resource-openapi.json
operations: [HospitalityFacilitySearch, FindHospitalityAvailability, RetrieveHospitalityAvailability]
generated: '2026-07-28'
method: generated
source: openapi/opentravel-2018a-facility-resource-openapi.json
---

# OpenTravel 2018A hospitality facility availability

Backing library `HospitalityFacility 1.0.0`, `LibraryStatus: FINAL`, compiled 2018-04-05.
A facility is "a hotel facility, conference facility or golf facility" with a single address
and a single geolocation.

**Do not confuse this with the 2020A global FacilityResource** — same title, different
release, different operations, different schema module. See
`skills/opentravel-alliance-facility-resource.md`.

## Before you start — read this

`host: 127.0.0.1`, `basePath: /`, `schemes: [http]` are OTM placeholders; OpenTravel serves
nothing. No `securityDefinitions` are declared. Both JSON and XML are accepted and produced —
note this contract lists `application/xml` **first** on `consumes` and `application/json`
first on `produces`, so always send an explicit `Content-Type` and `Accept`.

## Operations — a three-step flow

### 1. `HospitalityFacilitySearch` — POST /Facilities

Body: `FacilityQueryFacilitySearch`. Returns `201` with `HospitalityFacilities`.
Find the facilities in scope.

### 2. `FindHospitalityAvailability` — POST /HospitalityFindHospitalityAvailability

Body: `FacilityQueryFindHospitalityAvailability`. Returns `201` with `HospitalityFacilities`.
Ask for availability across the facilities you found.

### 3. `RetrieveHospitalityAvailability` — POST /HospitalityRetrieveFacilityAvailability

Body: `FacilityQueryRetrieveHospitalityAvailability`. Returns `201` with
`HospitalityFacilities`. Retrieve a previously-found availability result.

## Rules an agent must respect

- All three are POSTs with `201` success. `201` here means "query answered", not "created".
- All three return the **same** `HospitalityFacilities` type — branch on which operation you
  called, not on the payload shape.
- No idempotency key, no pagination, no request-id/correlation header exists in the contract.
  Retries are unguarded; results are uncapped and unpageable.
- The find → retrieve split implies server-side state between calls, but the contract
  specifies **no** handle, token or TTL for it. Confirm the correlation mechanism with the
  implementer before building a two-step flow.

## Error handling

`400, 401, 402, 403, 404, 500` on all three operations, every one returning
`HospitalityFacility_1_0_0_Trim.schema.json#/definitions/HospitalityFacilities` — the same
type as success, with an empty description. Check the status code before parsing.
See `errors/opentravel-alliance-problem-types.yml`.

## Schemas

- `json-schema/opentravel-2018a-hospitality-facility-trim.schema.json` —
  `HospitalityFacilities`, `HospitalityRetrieveFacilityAvailability`,
  `FacilityQueryFacilitySearch`, `FacilityQueryFindHospitalityAvailability`,
  `FacilityQueryRetrieveHospitalityAvailability`
- The `-defs-` sibling of the OpenAPI file inlines all 187 model definitions. Note the
  upstream file is Latin-1 encoded, not UTF-8.
