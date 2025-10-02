---
title: Order History
layout: default
---

## Authentication

Include the `Authorization` header with a bearer token from the [`/token` endpoint](authentication.html) to retrieve order history.

## Usage

Fetch orders created under your partner account within a configurable look-back window. Use the optional `days` query parameter to adjust the timeframe (defaults to 30 days). Each record includes order metadata, line items, and key timestamps.

### Request
```plaintext
GET /api/partner/v1/orders?days=7 HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
```

### Node.js Example Request
```javascript
// Node.js 18+ example using the built-in fetch API
async function fetchOrders() {
  const response = await fetch('https://base.shopjimmy.com/api/partner/v1/orders?days=7', {
    headers: {
      Authorization: 'Bearer YOUR_ACCESS_TOKEN',
      'Content-Type': 'application/json'
    }
  });

  if (!response.ok) {
    throw new Error(`Orders request failed with status ${response.status}`);
  }

  const data = await response.json();
  console.log(data);
}

fetchOrders().catch(console.error);
```

### 200 Response
```json
[
  {
    "reference": "PAPI3ZAABG36JWR",
    "po_number": "PO-12345",
    "customer_email": "receiving@example.com",
    "customer_name": "Robin Receiver",
    "total": 199.95,
    "note": "Purchase Order: PO-12345",
    "items": [
      {
        "listing_id": 438195,
        "sku": "sj-62661-2",
        "paid": 99.98,
        "qty": 1
      }
    ],
    "created_at": "2024-08-07T13:24:02.000Z",
    "shipped_at": "2024-08-08T21:28:00.000Z",
    "canceled_at": null
  }
]
```

### 500 Response
```json
{
  "error": "There was an internal server error. Please contact administrator."
}
```
