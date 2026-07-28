---
name: OpenTravel 2018A hospitality offers
description: >-
  Find single-property and multi-property hotel offers against a system that implements the
  OpenTravel 2018A HospitalityOffersResource contract.
api: openapi/opentravel-2018a-hospitality-offers-resource-openapi.json
operations: [FindSinglePropertyOffers, FindMultiPropertyOffers]
generated: '2026-07-28'
method: generated
source: openapi/opentravel-2018a-hospitality-offers-resource-openapi.json
---

# OpenTravel 2018A hospitality offers

The shop step: given a query, return the offers a property (or a set of properties) can sell.
Backing library `HospitalityOffers 1.0.0`, `LibraryStatus: FINAL`, compiled 2018-04-05 — this
is one of the two FINAL contracts in this repository.

## Before you start — read this

OpenTravel operates no endpoint. `host: 127.0.0.1`, `basePath: /`, `schemes: [http]` are
placeholders. Auth is the implementer's; no `securityDefinitions` are declared.

Both operations take the **same** request body type,
`HospitalityOffers_1_0_0_Trim.schema.json#/definitions/HospitalityOffersQueryFindHospitalityOffers`.
The two operations differ only in the shape of what comes back.

## Operations

### 1. `FindSinglePropertyOffers` — POST /HospitalityPropertyOffers

Returns `201` with `SinglePropertyHospitalityOffers`. Use when the query names one property.

### 2. `FindMultiPropertyOffers` — POST /HospitalityOffers

Returns `201` with `MultiPropertyHospitalityOffers`, which carries a collection of the
single-property shape. Use for a city, area or chain-wide shop.

## Rules an agent must respect

- **Success is `201`, not `200`.** The OTM compiler uses 201 for these query responses. Do not
  interpret it as resource creation.
- **These are POSTs that read.** There is no idempotency key and no pagination in the
  OpenTravel contract, so a retry is a second shop request and there is no way to page a
  multi-property result. Constrain the query instead.
- **No rate-limit signalling** is specified. Agree throughput with the implementer.
- **Offers are perishable.** OpenTravel's own Offer–Order work
  (https://opentravel.org/offer-order/) frames an offer as a basket that becomes an order on
  payment; the 2018A contract carries no TTL or booking step, so never cache an offer beyond
  the implementer's stated validity.

## Error handling

`400, 401, 402, 403, 404, 500`. Note a **spec defect**: in the 2018A suite the compiler reused
the operation's own success payload as the error payload, so a 4xx returns
`SinglePropertyHospitalityOffers` / `MultiPropertyHospitalityOffers` rather than a distinct
error type, and every description is empty. Do not parse an error body as offers — check the
status code first. See `errors/opentravel-alliance-problem-types.yml`.

## Schemas

- `json-schema/opentravel-2018a-hospitality-offers-trim.schema.json` —
  `HospitalityOffersQueryFindHospitalityOffers`, `SinglePropertyHospitalityOffers`,
  `MultiPropertyHospitalityOffers`
- The `-defs-` sibling of the OpenAPI file inlines all 372 model definitions.
