---
title: Place Order
layout: default
---

## API Authentication

To place an order using the Partner API, include the `Authorization` header in your HTTP request.
The `Authorization` header must contain a Bearer token that you obtained from the [`/token` endpoint](authentication.html).

## Usage
Most billing information is taken from your partner account profile. The payload supplies destination details, the requested
carrier/service, and the line items from your search results. Provide a valid shipping phone number and at least one line item.

Use the [`/shippingQuote` endpoint](shipping-quote.html) to see which methods are enabled for your account.

### POST Request to place order
```plaintext
POST /api/partner/v1/order HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
```

### Body JSON definition
```json
{
  "po_number": "Optional PO number shown on packing slips",
  "ship_method": "FedEx FedEx Next Day Air",
  "delivery_instructions": "Leave at receiving desk",
  "destination_email": "receiving@example.com",
  "destination_name": "Robin Receiver",
  "destination_address_1": "123 Warehouse Way",
  "destination_address_2": "Suite 100",
  "destination_city": "Burnsville",
  "destination_state": "MN",
  "destination_postcode": "55337",
  "destination_country": "US",
  "destination_telephone": "8005550100",
  "destination_business": "Warehouse Inc",
  "items": [
    {
      "listing_id": 336076,
      "qty": 2
    }
  ]
}
```

### 200 Response
```json
{
  "reference": "PAPI3ZAABG36JWR"
}
```

### 400 Response
Returned when the payload does not satisfy validation.
```json
{
  "error": "Validation failed {\"context\":{\"label\":\"ship_method\",\"value\":null}}"
}
```

### 500 Response
```json
{
  "error": "There was an internal server error. Please contact administrator.",
  "message": "Detailed failure reason"
}
```
