---
title: Webhooks
layout: default
---

## Webhooks

Webhooks let your application receive Partner API lifecycle updates at an HTTPS endpoint you control. Create a subscription for the events you need, verify each delivery, and use the status endpoint to investigate a missed or failed delivery.

All webhook management and discovery requests require the `Authorization` bearer token obtained from the [`/token` endpoint](authentication.html).

## Delivery model

- Deliveries use `POST` with a JSON request body and a 10-second timeout.
- Each webhook event has a stable `id`. Treat it as an idempotency key: delivery is **at least once**, so duplicate deliveries are possible.
- Ordering is best-effort per endpoint. Do not assume events arrive strictly in business-event order.
- A successful `2xx` response marks a delivery complete. Network failures, timeouts, `408`, `425`, `429`, and `5xx` responses are retried. Other `4xx` responses are not retried.
- A delivery is attempted at most eight times. Retries use exponential delays of approximately 1 minute, 5 minutes, 15 minutes, 1 hour, 3 hours, 12 hours, 24 hours, and 48 hours, with up to 15% jitter.

Your receiver should acknowledge accepted events quickly and process them asynchronously. Use the event `id` to avoid applying an event more than once.

## Events

| Event | Description |
| --- | --- |
| `order.created` | A partner order was accepted and assigned a ShopJimmy reference. |
| `order.canceled` | An order was canceled before shipment. |
| `order.shipped` | A package was created for the order. A separate event can be sent for each package. |
| `invoice.created` | An invoice was generated for a partner order. |
| `credit.issued` | A credit or refund was issued against a partner order. |
| `return.created` | A return request was created. |
| `return.credited` | A return request was approved and credited. |
| `return.denied` | A return request was denied. |

Use `GET /api/partner/v1/webhooks/events` to retrieve the current event catalog and protocol version programmatically.

## Verify webhook deliveries

Webhook deliveries include these headers:

| Header | Meaning |
| --- | --- |
| `X-SJ-Webhook-Id` | Unique, stable event identifier. |
| `X-SJ-Event` | Event name, such as `order.shipped`. |
| `X-SJ-Event-Version` | Event schema version. |
| `X-SJ-Delivery-Attempt` | One-based delivery attempt number. |
| `X-SJ-Timestamp` | Unix timestamp in seconds. |
| `X-SJ-Signature-256` | Hex-encoded HMAC-SHA256 signature. |

The signature is calculated as HMAC-SHA256 of the exact UTF-8 string:

```plaintext
{X-SJ-Timestamp}.{raw request body}
```

Use the `signing_secret` returned when the subscription is created. Verify the signature against the raw, unparsed request body before trusting the payload. Also reject stale timestamps according to your own replay-window policy.

### Node.js signature verification example

```javascript
const crypto = require('node:crypto');

function verifyWebhook(rawBody, headers, signingSecret) {
  const timestamp = String(headers['x-sj-timestamp'] || '');
  const received = String(headers['x-sj-signature-256'] || '');
  const payload = `${timestamp}.${rawBody}`;
  const expected = crypto
    .createHmac('sha256', signingSecret)
    .update(payload)
    .digest('hex');

  return received.length === expected.length
    && crypto.timingSafeEqual(Buffer.from(received), Buffer.from(expected));
}
```

## Event payload

Every delivery has this envelope. The contents of `data` depend on the event type.

```json
{
  "id": "wh_8f95d0e98f6d0f48dce58b34f86b716d2fb1dfd5",
  "type": "order.shipped",
  "version": "2026-05-12",
  "occurred_at": "2026-05-12T15:04:05.000Z",
  "created_at": "2026-05-12T15:04:06.000Z",
  "account": {
    "id": "0123456789abcdef0123456789abcdef",
    "site": "PARTNERSITE"
  },
  "resource": {
    "type": "shipment",
    "id": 778899
  },
  "data": {
    "order": {
      "id": 318968,
      "reference": "ORD4DJ363HJ",
      "po_number": "PO-1042"
    },
    "shipment": {
      "id": 778899,
      "carrier": "ups",
      "service": "UPS Ground",
      "tracking_number": "1Z999AA10123456784",
      "shipped_at": "2026-05-12T15:04:05.000Z"
    }
  }
}
```

Use `GET /api/partner/v1/webhooks/protocol` for the current protocol definition, including an example envelope, delivery policy, and management paths.

## Create or update a subscription

`POST /api/partner/v1/webhooks/subscribe` creates a new subscription. If a subscription for the same `target_url` already exists for your application, this request reactivates and updates it.

The target must be a valid `https://` URL. `events` must contain at least one supported event. `max_attempts` is optional and may be from 1 through 8.

```json
{
  "target_url": "https://partner.example.com/webhooks/shopjimmy",
  "description": "Production order notifications",
  "max_attempts": 8,
  "events": [
    "order.created",
    "order.shipped",
    "credit.issued"
  ]
}
```

### Node.js example request

```javascript
const response = await fetch('https://base.shopjimmy.com/api/partner/v1/webhooks/subscribe', {
  method: 'POST',
  headers: {
    Authorization: 'Bearer YOUR_ACCESS_TOKEN',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    target_url: 'https://partner.example.com/webhooks/shopjimmy',
    description: 'Production order notifications',
    events: ['order.created', 'order.shipped', 'credit.issued']
  })
});

const subscription = await response.json();
```

### 200 Response

Store the returned `signing_secret` securely. It is needed to verify deliveries.

```json
{
  "id": 42,
  "app_id": "0123456789abcdef0123456789abcdef",
  "target_url": "https://partner.example.com/webhooks/shopjimmy",
  "description": "Production order notifications",
  "status": "active",
  "events": ["order.created", "order.shipped", "credit.issued"],
  "signing_secret": "a1b2c3d4e5f6...",
  "created_at": "2026-05-12T15:00:00.000Z",
  "updated_at": "2026-05-12T15:00:00.000Z",
  "unsubscribed_at": null,
  "last_delivery_at": null,
  "last_failure_at": null,
  "retry_policy": {
    "strategy": "exponential_backoff_with_jitter",
    "max_attempts": 8,
    "max_age_hours": 168,
    "jitter_ratio": 0.15,
    "base_delay_seconds": 60,
    "schedule_seconds": [60, 300, 900, 3600, 10800, 43200, 86400, 172800]
  }
}
```

## Manage subscriptions

### List subscriptions

`GET /api/partner/v1/webhooks/subscriptions` returns subscriptions for the authenticated application:

```json
{
  "subscriptions": [
    {
      "id": 42,
      "target_url": "https://partner.example.com/webhooks/shopjimmy",
      "status": "active",
      "events": ["order.created", "order.shipped"]
    }
  ]
}
```

### Unsubscribe

`POST /api/partner/v1/webhooks/unsubscribe` stops future deliveries but retains the subscription record. Supply either `subscription_id` or `target_url`.

```json
{ "subscription_id": 42 }
```

### Destroy

`POST /api/partner/v1/webhooks/destroy` permanently removes a subscription. Supply either `subscription_id` or `target_url`.

```json
{ "subscription_id": 42 }
```

The response confirms the deleted endpoint:

```json
{
  "id": 42,
  "target_url": "https://partner.example.com/webhooks/shopjimmy",
  "destroyed": true
}
```

## Monitor delivery status

Use `GET /api/partner/v1/webhooks/status` to inspect delivery attempts for the authenticated application. Results are returned newest first.

| Query parameter | Description |
| --- | --- |
| `page` | Page number; defaults to `1`. |
| `per` | Results per page; defaults to `50`, maximum `200`. |
| `subscription_id` | Filter by subscription. |
| `status` | Filter by delivery status, such as `pending`, `retry`, `delivered`, or `failed`. |
| `event_name` | Filter by event name. |
| `entity_type` | Filter by resource type. |
| `entity_id` | Filter by resource ID. |

For example:

```plaintext
GET /api/partner/v1/webhooks/status?subscription_id=42&status=failed&page=1&per=50
```

The response includes the delivery `status`, `attempts`, `response_code`, `last_error_message`, `next_attempt_at`, and `delivered_at` so you can diagnose failures and correlate them with your receiver logs.

To inspect the configured retry policy directly, use `GET /api/partner/v1/webhooks/retry-policy`.
