---
title: Credit Details
layout: default
---

## Authentication

Include the `Authorization` header with a bearer token from the [`/token` endpoint](authentication.html) to retrieve an individual credit record.

## Usage

Provide the order `reference` returned by the [`/credits` endpoint](credits.html) to fetch the full credited order record, including order details, invoices, returns, and package data.

### Request
```plaintext
GET /api/partner/v1/credit/{order_reference} HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
```

### Node.js Example Request
```javascript
// Node.js 18+ example using the built-in fetch API
async function fetchCreditDetails(orderReference) {
  const response = await fetch(`https://base.shopjimmy.com/api/partner/v1/credit/${orderReference}`, {
    headers: {
      Authorization: 'Bearer YOUR_ACCESS_TOKEN',
      'Content-Type': 'application/json'
    }
  });

  if (!response.ok) {
    throw new Error(`Credit details request failed with status ${response.status}`);
  }

  const data = await response.json();
  console.log(data);
}

fetchCreditDetails('ORD4MQ43R').catch(console.error);
```

### 200 Response
```json
{
  "id": 324477,
  "reference": "ORD4MQ43R",
  "po_number": "PO-1024",
  "customer_email": "ops+returns@example.com",
  "customer_name": "Harbor Technical Services",
  "shipping_address_1": "18884 Grace St",
  "shipping_address_2": "",
  "shipping_city": "LINDEN",
  "shipping_state": "CA",
  "shipping_postcode": "95236",
  "shipping_country": "US",
  "shipping_telephone": "5550100300",
  "shipping_business": null,
  "total": "44.99",
  "note": "",
  "items": [
    {
      "id": 286004,
      "listing_id": 328398,
      "sku": "sj-LB65065",
      "qty": 1,
      "paid": 44.99,
      "tax": 0
    }
  ],
  "credits": [
    {
      "id": 41341,
      "reason": "Partner API Return #270",
      "amount": "44.99",
      "created_at": "2026-03-04T14:53:47.000Z"
    }
  ],
  "invoices": [
    {
      "id": 285955,
      "items": [
        {
          "listing_id": 328398,
          "sku": "sj-LB65065",
          "qty": 1,
          "price": "44.99",
          "tax": "0.00"
        }
      ],
      "total": "44.99",
      "tax": "0.00",
      "shipping": "0.00",
      "discount": "0.00",
      "created_at": "2026-02-19T23:15:02.000Z"
    }
  ],
  "returns": [
    {
      "id": 270,
      "created_at": "2026-02-24T17:29:50.000Z",
      "credited_at": "2026-03-04T14:53:47.000Z",
      "denied_at": null,
      "reference": "ORD4MQ43R--1",
      "reason_class": "other",
      "reason_description": "Defective Part",
      "comment": null,
      "items": [
        {
          "listing_id": 328398,
          "sku": "sj-LB65065",
          "qty": 1
        }
      ]
    }
  ],
  "created_at": "2026-02-19T22:02:01.000Z",
  "shipped_at": "2026-02-19T23:15:00.000Z",
  "canceled_at": null,
  "packages": [
    {
      "id": 257380,
      "created_at": "2026-02-19T23:15:00.000Z",
      "dimensions": "40x4x4",
      "weight_lbs": 1.45,
      "carrier": "ups",
      "service": "02",
      "tracking_number": "1Z999AA10123456901",
      "cost_usd": 0,
      "customer_email": "ops+returns@example.com",
      "customer_name": "Harbor Technical Services",
      "destination_name": "Harbor Technical Services",
      "destination_address_1": "18884 Grace St",
      "destination_address_2": "",
      "destination_city": "LINDEN",
      "destination_state": "CA",
      "destination_postcode": "95236",
      "destination_country": "US",
      "destination_telephone": "5550100300",
      "destination_business": null,
      "destination_email": "ops+returns@example.com",
      "items": [
        {
          "listing_id": 328398,
          "sku": "sj-LB65065",
          "qty": 1,
          "serial": "",
          "part_number": "LB65065",
          "substituted": false
        }
      ]
    }
  ]
}
```

### 404 Response
The credit reference was not found for your account.
```json
{
  "error": "Credit not found"
}
```

### 500 Response
```json
{
  "error": "There was an internal server error. Please contact administrator."
}
```
