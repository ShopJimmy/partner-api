---
title: Create Return
layout: default
---

## Authentication

Include the `Authorization` header with a bearer token from the [`/token` endpoint](authentication.html) to create returns.

## Usage

Submit a return request for an existing order reference. Schema validation requires `reference` and `reason_class`.

The payload rejects unknown fields at both the top level and each `items[]` object.

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
- `reason_description`: string, optional, allows `""` and `null`
- `items`: array, optional
- `items` (when provided): minimum 1 item
- `items`: unique by `listing_id`
- `items[].listing_id`: integer, required
- `items[].qty`: integer, required, minimum 1
- Unknown fields: not allowed (`unknown(false)` at top level and item level)

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
  ]
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
      ]
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
  "success": true
}
```

### 400 Response
Returned when payload validation fails.
```json
{
  "error": "Validation failed {\"context\":{\"label\":\"reference\",\"value\":null}}"
}
```

### 500 Response
```json
{
  "error": "There was an internal server error. Please contact administrator."
}
```
