---
title: Order History
layout: default
---

## API Authentication

To retrieve order history using the Partner API, include the `Authorization` header in your HTTP request.
The `Authorization` header must contain a Bearer token that you obtained from the [`/token` endpoint](authentication.html).

## Usage
Returns orders created for your partner account within the requested look-back window. Use the optional `days` query parameter
to control the look-back window (defaults to 30 days). Each order includes key metadata plus line items and timestamps.

### GET Request to retrieve orders
```plaintext
GET /api/partner/v1/orders?days=7 HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
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
