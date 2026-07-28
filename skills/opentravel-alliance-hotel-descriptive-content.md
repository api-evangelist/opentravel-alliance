---
name: OpenTravel 2020A hotel descriptive content
description: >-
  Search, query and receive push notifications of hotel descriptive content against a system
  that implements the OpenTravel 2020A HotelDescriptiveContentResource contract.
api: openapi/opentravel-2020a-hotel-descriptive-content-resource-openapi.json
operations: [Get, Query, Notification]
generated: '2026-07-28'
method: generated
source: openapi/opentravel-2020a-hotel-descriptive-content-resource-openapi.json
---

# OpenTravel 2020A hotel descriptive content

## Before you start — read this

OpenTravel does not host this API. The contract declares `host: 127.0.0.1`, `basePath: /v1_0`
and `schemes: [http]` — all OTM compiler placeholders. You are calling **an implementer's**
server (a hotel chain, channel manager, CRS or GDS that adopted OpenTravel 2.0). Get the real
base URL and the authentication scheme from that implementer: the contract declares **no**
`securityDefinitions` even though every operation returns 401 and 403.

Content negotiation: every operation accepts and returns **both** `application/json` and
`application/xml`. Send an explicit `Accept` header; there is no documented default.

The backing library is `HospitalityContent 1.0.0` with `LibraryStatus: DRAFT` — the contract
is not final.

## Operations

### 1. `Get` — GET /HotelDescriptiveContents

Broad search using flat, all-optional query parameters:

| Parameter | Type | Notes |
|---|---|---|
| `code` | string(32) | uniquely identifies a single property |
| `chainCode` | string(32) | identifies the chain (example in spec: `MC`) |
| `brandCode` | string(32) | brand within a chain (example: `FS`) |
| `cityCode` | string(32) | three character IATA city code (example: `WAS`) |
| `areaID` | string(512) | reservation-system-defined area (suburb, region) |
| `hotelCode_TTI` | int32 ≥ 0 | TTI hotel reference code |
| `displayCurrency` | string, pattern `[a-zA-Z]{3}` | ISO 4217 currency code |
| `maxResponses` | int32 | maximum number of results |
| `summaryResultsInd` | boolean | return summary information only |

Returns `200` with `HotelDescriptiveContentListResponse`.

**There is no pagination.** `maxResponses` is a hard cap with no cursor, offset, next link or
total count — if your filter matches more than the cap you cannot reach the rest. Narrow the
filter (chain + city, or an explicit property `code`) instead of raising the cap.

### 2. `Query` — POST /HotelDescriptiveContents/Query

For anything the flat parameters cannot express. Body is a
`HotelDescriptiveContentQueryInfo` object. Success is **`201`**, not 200 — do not treat 201
as "created"; the OTM compiler uses it for query responses. Response payload is the same
`HotelDescriptiveContentListResponse`.

Note this is a **POST for a read**. It is therefore not safe to retry blindly: there is no
idempotency key anywhere in the OpenTravel contract. If you retry, expect duplicate work on
the server side.

### 3. `Notification` — POST /HotelDescriptiveContents/Notifications

The push half of the contract. Body is a `HotelDescriptiveContentID`. Success is `201` with
an empty body.

If you are **consuming** notifications, you implement this endpoint; the contract specifies no
subscription mechanism, no signing, no retry policy and no replay endpoint — agree all of that
bilaterally with the sender. See `asyncapi/opentravel-alliance-notifications-webhooks.yml`.

## Error handling

Every operation declares `400, 401, 402, 403, 404, 500`, all carrying
`Common_5_0_0_Trim.schema.json#/definitions/BaseResponse`, all with an **empty description**.
There is no error code registry and no `application/problem+json`. `402 Payment Required` is
compiler boilerplate — no documented condition produces it.

Treat 401/403 as "the implementer's auth scheme rejected you", not as anything the
specification defines. See `errors/opentravel-alliance-problem-types.yml`.

## Schemas

- `json-schema/opentravel-2020a-hospitality-content-hospitality-resources-trim.schema.json` —
  `HotelDescriptiveContentListResponse`, `HotelDescriptiveContentQueryInfo`,
  `HotelDescriptiveContentQueryGet`
- `json-schema/opentravel-2020a-organization-hospitality-4-0-0-trim.schema.json` —
  `HotelDescriptiveContent`, `HotelDescriptiveContentID`, `GuestRoomInfo`, `Recreation`,
  `Service` (54 definitions)
- `json-schema/opentravel-2020a-common-5-0-0-trim.schema.json` — `BaseResponse` and shared
  primitives (89 definitions)
