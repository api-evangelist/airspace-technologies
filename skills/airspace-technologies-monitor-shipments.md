---
name: Monitor active shipments
description: Track live status across Airspace orders via the events polling endpoints (webhook alternative).
api: openapi/airspace-technologies-v3-openapi.json
operations: [getOrders, getOrdersEvents, getEventsForOrderById]
---

# Monitor active shipments

Authenticate with `Authorization: Bearer <API_KEY>`.

1. **List orders** — `GET /orders` (`getOrders`), paginating with `page` and `page_limit`.
2. **Poll the fleet event stream** — `GET /orders/events` (`getOrdersEvents`) for events
   across all your orders.
3. **Poll a single order** — `GET /orders/{tracking_id}/events` (`getEventsForOrderById`).

Each event carries a `milestone`, or a `problem` object (with a `code` such as `2-001`
"Changed shipment parameters" or `2-007`) for delay/cancellation events. This polling path
mirrors the webhook surface (`asyncapi/airspace-technologies-webhooks.yml`); prefer webhooks
for push delivery and use polling as a reconciliation fallback.
