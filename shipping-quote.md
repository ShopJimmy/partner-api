---
title: Shipping Quote
layout: default
---

## Authentication

Include the `Authorization` header with a bearer token from the [`/token` endpoint](authentication.html) when requesting shipping quotes.

## Usage

The endpoint returns the carrier services available to your partner account based on the shipping credentials on file. Provide the destination address in the request payload so the service list can be validated.

### Request
```plaintext
POST /api/partner/v1/shippingQuote HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
```

### Request Body
```json
{
  "destination_name": "Emery House",
  "destination_address_1": "123 Warehouse Way",
  "destination_address_2": "Suite 100",
  "destination_city": "Burnsville",
  "destination_state": "MN",
  "destination_postcode": "55337",
  "destination_country": "US",
  "destination_telephone": "6515551234",
  "destination_business": "ShopJimmy",
  "items": [
    {
      "listing_id": 314205,
      "qty": 1
    }
  ]
}
```

### 200 Response
```json
[
  {
    "id": "FedEx Ground",
    "label": "FedEx Ground",
    "description": "5-7 business day arrival",
    "amount": 0
  },
  {
    "id": "UPS UPS Ground",
    "label": "UPS Ground",
    "description": "5-7 business day arrival",
    "amount": 0
  }
]
```

### 500 Response
```json
{
  "error": "There was an internal server error. Please contact administrator."
}
```
