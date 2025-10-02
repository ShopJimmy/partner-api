---
title: Order Status
layout: default
---

## API Authentication

To get an order's status using the Partner API, include the `Authorization` header in your HTTP request.
The `Authorization` header must contain a Bearer token that you obtained from the [`/token` endpoint](authentication.html).

## Usage
This action returns timestamps for order lifecycle events, a snapshot of the order items, invoices, credits, returns, and every
package that has shipped. You can only retrieve orders that were placed by your partner account.

### GET Request to retrieve order
```plaintext
GET /api/partner/v1/order/{order_reference} HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
```

### 200 Response
```json
{
  "reference": "PAPI3ZAABG36JWR",
  "po_number": "PO-12345",
  "total": 199.95,
  "note": "Purchase Order: PO-12345",
  "items": [
    {
      "listing_id": 438195,
      "sku": "sj-62661-2",
      "qty": 1,
      "paid": 99.98,
      "tax": 0
    }
  ],
  "credits": [
    {
      "reason": "Damaged in transit",
      "amount": 10.0,
      "created_at": "2024-08-09T14:20:00.000Z"
    }
  ],
  "invoices": [
    {
      "id": 555432,
      "items": [
        {
          "listing_id": 438195,
          "sku": "sj-62661-2",
          "qty": 1,
          "price": 99.98,
          "tax": 0
        }
      ],
      "total": 99.98,
      "tax": 0,
      "shipping": 0,
      "discount": 0,
      "created_at": "2024-08-07T13:24:02.000Z"
    }
  ],
  "returns": [
    {
      "id": 321,
      "created_at": "2024-08-10T10:12:00.000Z",
      "credited_at": null,
      "denied_at": null,
      "items": [
        {
          "listing_id": 438195,
          "sku": "sj-62661-2",
          "amount": 99.98,
          "qty": 1
        }
      ]
    }
  ],
  "created_at": "2024-08-07T13:24:02.000Z",
  "shipped_at": "2024-08-08T21:28:00.000Z",
  "canceled_at": null,
  "packages": [
    {
      "created_at": "2024-08-08T21:28:00.000Z",
      "dimensions": "17x7x1",
      "weight_lbs": 0.85,
      "carrier": "usps",
      "service": "US-PM",
      "tracking_number": "9405511105501325283592",
      "cost_usd": 8.72,
      "destination_name": "Annie Mall",
      "destination_address_1": "2300 West Highway 13",
      "destination_address_2": "",
      "destination_city": "Burnsville",
      "destination_state": "MN",
      "destination_postcode": "55337",
      "destination_country": "US",
      "destination_telephone": "877-881-6492",
      "destination_business": "ShopJimmy.com",
      "destination_email": "annie@example.com",
      "items": [
        {
          "listing_id": 438195,
          "sku": "sj-62661-2",
          "qty": 1,
          "serial": null,
          "part_number": "62661-2",
          "substituted": false
        }
      ]
    }
  ]
}
```

### 404 Response
Returned when the order is not found for your account.
```json
{
  "error": "Order not found."
}
```

### 500 Response
```json
{
  "error": "There was an internal server error. Please contact administrator."
}
```
