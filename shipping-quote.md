---
title: Shipping Quote
layout: default
---

## API Authentication

To request shipping methods using the Partner API, include the `Authorization` header in your HTTP request.
The `Authorization` header must contain a Bearer token that you obtained from the [`/token` endpoint](authentication.html).

## Usage
Returns the carrier services that are enabled for your partner account based on the shipping accounts on file. The payload
should include the destination address so the request can be validated.

### POST Request to retrieve shipping methods
```plaintext
POST /api/partner/v1/shippingQuote HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
```

### Body JSON definition
```json
{
  "destination_address_1": "123 Warehouse Way",
  "destination_address_2": "Suite 100",
  "destination_city": "Burnsville",
  "destination_state": "MN",
  "destination_postcode": "55337",
  "destination_country": "US"
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
