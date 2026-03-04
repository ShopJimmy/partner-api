---
title: Order History
layout: default
---

## Authentication

Include the `Authorization` header with a bearer token from the [`/token` endpoint](authentication.html) to retrieve order history.

## Usage

Fetch orders created under your partner account within a configurable look-back window. Use the optional `days` query parameter to adjust the timeframe (defaults to 30 days). Each record includes order metadata, shipping destination fields, line items, package/tracking details, and lifecycle timestamps.

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
    "id": 333091,
    "reference": "ORD7XK33D9",
    "po_number": "PO-1001",
    "customer_email": "receiving+alpha@example.com",
    "customer_name": "Acme Service Center",
    "shipping_address_1": "1250 Market St Suite 210",
    "shipping_address_2": "",
    "shipping_city": "DENVER",
    "shipping_state": "CO",
    "shipping_postcode": "80202",
    "shipping_country": "US",
    "shipping_telephone": "5550100100",
    "shipping_business": null,
    "total": "59.99",
    "note": "",
    "items": [
      {
        "listing_id": 337660,
        "sku": "sj-BN96-52594A",
        "paid": 59.99,
        "qty": 1
      }
    ],
    "packages": [],
    "created_at": "2026-03-04T17:33:11.000Z",
    "shipped_at": null,
    "canceled_at": null
  },
  {
    "id": 332817,
    "reference": "ORD76439QA",
    "po_number": "PO-1002",
    "customer_email": "warehouse+beta@example.com",
    "customer_name": "Northside Repair Depot",
    "shipping_address_1": "8800 Lakeview Dr",
    "shipping_address_2": "Dock B",
    "shipping_city": "GLENVIEW",
    "shipping_state": "IL",
    "shipping_postcode": "60025",
    "shipping_country": "US",
    "shipping_telephone": "5550100200",
    "shipping_business": null,
    "total": "79.99",
    "note": "",
    "items": [
      {
        "listing_id": 337661,
        "sku": "sj-BN96-52592A",
        "paid": 79.99,
        "qty": 1
      }
    ],
    "packages": [
      {
        "created_at": "2026-03-04T16:56:00.000Z",
        "dimensions": "27x4x4",
        "weight_lbs": 0.5,
        "carrier": "ups",
        "service": "02",
        "tracking_number": "1Z999AA10123456784",
        "cost_usd": 0,
        "destination_name": "Northside Repair Depot",
        "destination_address_1": "8800 Lakeview Dr",
        "destination_address_2": "Dock B",
        "destination_city": "GLENVIEW",
        "destination_state": "IL",
        "destination_postcode": "60025",
        "destination_country": "US",
        "destination_telephone": "5550100200",
        "destination_business": null,
        "destination_email": "warehouse+beta@example.com",
        "items": [
          {
            "listing_id": 337661,
            "sku": "sj-BN96-52592A",
            "qty": 1,
            "serial": "",
            "part_number": "BN96-52592A",
            "substituted": false
          }
        ]
      }
    ],
    "created_at": "2026-03-04T02:39:22.000Z",
    "shipped_at": "2026-03-04T16:56:00.000Z",
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
