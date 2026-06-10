---
title: Create Return
layout: default
---

## Authentication

Include the `Authorization` header with a bearer token from the [`/token` endpoint](authentication.html) to create returns.

## Usage

Submit a return request for an existing order reference. Schema validation requires `reference`, `reason_class`, and at least one return item.

The payload rejects unknown fields at both the top level and each `items[]` object. You may optionally request a return label as part of the same call. When a label is requested, ShopJimmy derives the package `weight` and `dimensions` from the original outbound shipment, preferring a package that contained one of the returned listings. Carrier, service, and any optional return shipping account are taken from your partner configuration and are not supplied in the request body.

### Request
```plaintext
POST /api/partner/v1/return HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
```

### Validation Rules
- `reference`: string, max 45, required
- `inbound_tracking`: string, max 60, optional, allows `""` and `null`
- `reason_class`: string, required
- `reason_class`: allowed values
  - `damaged_in_transit`
  - `defective_or_failed`
  - `wrong_item_shipped`
  - `ordered_in_error`
  - `missing_parts`
  - `other`
- `reason_description`: string, optional, allows `""` and `null`
- `items`: array, required
- `items`: minimum 1 item
- `items`: unique by `listing_id`
- `items[].listing_id`: integer, required
- `items[].qty`: integer, required, minimum 1
- `return_label`: object, optional
- `return_label.delivery_method`: string, required when `return_label` is provided, allowed values `download`, `email`
- `return_label.label_format`: string, required when `delivery_method` is `download`, allowed values `GIF`, `ZPL`
- `return_label.email_address`: string, required when `delivery_method` is `email`
- Unknown fields: not allowed (`unknown(false)` at top level and item level)

### Return Reason Options
Use the `reason_class` key values below in your request payload:

- `damaged_in_transit`: Damaged in Transit
- `defective_or_failed`: Defective / Failed Under Warranty
- `wrong_item_shipped`: Wrong Item Shipped
- `ordered_in_error`: Ordered in Error
- `missing_parts`: Missing Parts / Accessories
- `other`: Other (Explain Below)

`reason_description` is especially useful for `damaged_in_transit`, `defective_or_failed`, `missing_parts`, and `other`.

### Request Body
```json
{
  "reference": "ORD4MQ43R",
  "inbound_tracking": "1Z999AA10123457012",
  "reason_class": "other",
  "reason_description": "Defective Part",
  "items": [
    {
      "listing_id": 328398,
      "qty": 1
    }
  ],
  "return_label": {
    "delivery_method": "download",
    "label_format": "GIF"
  }
}
```

### Email Delivery Example
```json
{
  "reference": "ORD4MQ43R",
  "reason_class": "other",
  "items": [
    {
      "listing_id": 328398,
      "qty": 1
    }
  ],
  "return_label": {
    "delivery_method": "email",
    "email_address": "returns@example.com"
  }
}
```

### Node.js Example Request
```javascript
// Node.js 18+ example using the built-in fetch API
async function createReturn() {
  const response = await fetch('https://base.shopjimmy.com/api/partner/v1/return', {
    method: 'POST',
    headers: {
      Authorization: 'Bearer YOUR_ACCESS_TOKEN',
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      reference: 'ORD4MQ43R',
      inbound_tracking: '1Z999AA10123457012',
      reason_class: 'other',
      reason_description: 'Defective Part',
      items: [
        {
          listing_id: 328398,
          qty: 1
        }
      ],
      return_label: {
        delivery_method: 'download',
        label_format: 'GIF'
      }
    })
  });

  if (!response.ok) {
    throw new Error(`Return request failed with status ${response.status}`);
  }

  const data = await response.json();
  console.log(data);
}

createReturn().catch(console.error);
```

### 200 Response
```json
{
  "reference": "ORD4MQ43R-0",
  "label": {
    "shipment_id": "1a2b3c4d5e6f",
    "tracking_number": "1Z999AA10123456784",
    "charges": "12.34",
    "label_id": 123,
    "reference": "ORD4MQ43R-0",
    "delivery_method": "download",
    "label_format": "GIF",
    "label_payload": "R0lGODlh...",
    "package": {
      "tracking_number": "1Z999AA10123456784",
      "label": "R0lGODlh..."
    },
    "errors": []
  }
}
```

If `return_label` is omitted from the request, the response returns the created return reference and `label` will be `null`.

### Return Label Configuration
Return labels use the partner-level defaults configured by ShopJimmy for your account:

- return carrier
- return service
- optional return shipping account id
- origin/from address from the partner app billing/contact address on file
- destination/return-to address from ShopJimmy's warehouse configuration

The request only controls whether to generate a label and how that label should be delivered back to you.

### 400 Response
Returned when payload validation fails.
```json
{
  "error": "Validation failed",
  "details": [
    "\"return_label.label_format\" must be one of [GIF, ZPL]"
  ]
}
```

### 500 Response
```json
{
  "error": "There was an error",
  "message": "Could not find an original shipped package for this return."
}
```
