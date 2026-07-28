---
name: OpenTravel 2020A facility resource
description: >-
  Add a facility to a facilities collection and retrieve one by identifier against a system
  that implements the OpenTravel 2020A global FacilityResource contract.
api: openapi/opentravel-2020a-facility-resource-openapi.json
operations: [Post, Get]
generated: '2026-07-28'
method: generated
source: openapi/opentravel-2020a-facility-resource-openapi.json
---

# OpenTravel 2020A facility resource

"RESTful service to access all types of facilities." A facility is a hotel, conference or golf
facility and **must have a single address and a single geolocation**.

## Before you start — read this

OpenTravel hosts nothing. This contract declares `host: example.com`,
`basePath: /resource/v1_0`, `schemes: [http]` — placeholders. Get the real base URL and the
auth scheme from the implementer; the contract declares no `securityDefinitions`.

Backing library `FacilityResource 1.0.0`, `LibraryStatus: DRAFT`, compiled 2020-05-15. This is
the **global** (cross-sector) facility resource in the 2020A Object Suite — distinct from the
2018A hospitality `FacilityResource`, which has different operations. See
`skills/opentravel-alliance-hospitality-facility-availability.md`.

## Operations

### 1. `Post` — POST /Facilities

Add a facility to the collection. Body is a `FacilityID` object
(`Organization_4_1_0_Trim.schema.json#/definitions/FacilityID`).

Success is `200` returning `FacilityList` — note the collection is returned, not the created
resource, and no `Location` header is specified.

**No idempotency contract exists.** There is no `Idempotency-Key` header or equivalent
anywhere in the OpenTravel corpus. A retried POST will create a second facility. If your agent
must retry, first `Get` by identifier and check for the existing record.

### 2. `Get` — GET /Facilities/{Identifier}

| Parameter | In | Type | Notes |
|---|---|---|---|
| `Identifier` | path | string(128), required | "a unique identifier defined by an external authority for this object" |

Success is `200` returning a `FacilityID`. The identifier is **implementer- or
authority-assigned** — OpenTravel mints no identifiers for commercial entities. Carry the
external code system (IATA, chain code, TTI) with the value so the counterparty can resolve it.

## Facility specializations

`Organization_4_1_0` defines `Facility` plus the substitutable views `FacilityHotel`,
`FacilityGolf`, `FacilityMeeting`, and the query views `FacilityQueryHotel`,
`FacilityQueryHotelAvail`, `FacilityQueryHotelAvailRetrieve`. Extension points are the OTM way
to add implementer-specific fields — there is no free-form metadata bag.

## Error handling

`400, 401, 402, 403, 404, 500` on both operations, all returning
`Common_5_0_0_Trim.schema.json#/definitions/BaseResponse` with an empty description. Not
RFC 9457. See `errors/opentravel-alliance-problem-types.yml`.

## Schemas

- `json-schema/opentravel-2020a-facility-resource-trim.schema.json` — `FacilityList`
- `json-schema/opentravel-2020a-organization-4-1-0-trim.schema.json` — `FacilityID`,
  `Facility`, `FacilityHotel`, `FacilityGolf`, `FacilityMeeting` and query views
- `json-schema/opentravel-2020a-common-5-0-0-trim.schema.json` — `BaseResponse`
