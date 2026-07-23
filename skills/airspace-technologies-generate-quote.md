---
name: Generate a shipping quote
description: Request an Airspace price/viability quote for a shipment before committing to an order.
api: openapi/airspace-technologies-v3-openapi.json
operations: [createQuote, getQuote]
---

# Generate a shipping quote

Authenticate with `Authorization: Bearer <API_KEY>`.

1. **Create the quote** — `POST /orders/quotes` (`createQuote`) with the same shape as an
   order (pickup/delivery addresses, pieces, service_type, pickup_time). The response
   returns a `tracking_id` and shipment viability.
2. **Retrieve the quote** — `GET /orders/quotes/{tracking_id}` (`getQuote`) to read pricing
   and `shipment_viability` once computed.

Quotes and orders share the same payload model (see
`data-model/airspace-technologies-data-model.yml`), so a validated quote body can be
reused to create an order.
