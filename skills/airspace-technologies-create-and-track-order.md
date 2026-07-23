---
name: Create and track a shipment order
description: Create a time-critical Airspace order, activate it for fulfillment, and follow its progress to delivery.
api: openapi/airspace-technologies-v3-openapi.json
operations: [createOrder, activateOrder, getOrderById, getEventsForOrderById]
---

# Create and track a shipment order

Authenticate every request with `Authorization: Bearer <API_KEY>` (see
`authentication/airspace-technologies-authentication.yml`). Work against the test host
`https://apitest.airspace.com/api/public/v3` until certified for production.

1. **Create the order** — `POST /orders` (`createOrder`) with pickup/delivery addresses,
   pieces, service_type, and pickup_time. The API is asynchronous: expect `202 Accepted`
   with a `request_id`. Store the returned `tracking_id` and `request_id`.
2. **Activate for fulfillment** — `PUT /orders/{tracking_id}/activate` (`activateOrder`)
   when the order is ready to be routed.
3. **Poll status** — `GET /orders/{tracking_id}` (`getOrderById`) to read the current
   `status` and `current_segment`.
4. **Follow events** — `GET /orders/{tracking_id}/events` (`getEventsForOrderById`) for the
   milestone/delay/cancellation stream, or subscribe to webhooks
   (`asyncapi/airspace-technologies-webhooks.yml`) and correlate by `request_id`.

Errors are a flat `{ "error": "..." }` envelope; `422` uses `{ "errors": "..." }`
(see `errors/airspace-technologies-problem-types.yml`). There is no client idempotency
key — use `request_id` to correlate async webhook events with your request.
