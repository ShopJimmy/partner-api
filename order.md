---
title: Place Order
layout: default
---

## Authentication

Include the `Authorization` header with a bearer token from the [`/token` endpoint](authentication.html) to submit orders.

## Usage

Billing information is derived from your partner profile. Provide shipment details, the desired carrier service, and at least one line item sourced from search results.

Schema validation requires `ship_method`, `destination_telephone`, and `items` (minimum of 1 item). Other top-level fields allow empty string (`""`) or `null`.

Use the [`/shippingQuote` endpoint](shipping-quote.html) to confirm which carrier services are enabled for your account.

### Request
```plaintext
POST /api/partner/v1/order HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
```

### Validation Rules
- `po_number`: string, max 45, optional, allows `""` and `null`
- `ship_method`: string, max 45, required
- `delivery_instructions`: string, max 45, optional, allows `""` and `null`
- `destination_email`: string, optional, allows `""` and `null`
- `destination_name`: string, max 45, optional, allows `""` and `null`
- `destination_address_1`: string, max 50, optional, allows `""` and `null`
- `destination_address_2`: string, max 35, optional, allows `""` and `null`
- `destination_city`: string, max 45, optional, allows `""` and `null`
- `destination_state`: string, max 2, optional, allows `""` and `null`
- `destination_postcode`: string, max 15, optional, allows `""` and `null`
- `destination_country`: string, max 2, optional, allows `""` and `null`
- `destination_telephone`: string, max 25, required
- `destination_business`: string, max 45, optional, allows `""` and `null`
- `items`: array, required, minimum 1 item
- `items[].listing_id`: integer, required when item is present
- `items[].qty`: integer, required when item is present

### Request Body
```json
{
  "po_number": "PO-1001",
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

### Node.js Example Request
```javascript
// Node.js 18+ example using the built-in fetch API
async function createOrder() {
  const response = await fetch('https://base.shopjimmy.com/api/partner/v1/order', {
    method: 'POST',
    headers: {
      Authorization: 'Bearer YOUR_ACCESS_TOKEN',
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      po_number: 'PO-1001',
      ship_method: 'FedEx FedEx Next Day Air',
      delivery_instructions: 'Leave at receiving desk',
      destination_email: 'receiving@example.com',
      destination_name: 'Robin Receiver',
      destination_address_1: '123 Warehouse Way',
      destination_address_2: 'Suite 100',
      destination_city: 'Burnsville',
      destination_state: 'MN',
      destination_postcode: '55337',
      destination_country: 'US',
      destination_telephone: '8005550100',
      destination_business: 'Warehouse Inc',
      items: [
        {
          listing_id: 336076,
          qty: 2
        }
      ]
    })
  });

  if (!response.ok) {
    throw new Error(`Order request failed with status ${response.status}`);
  }

  const data = await response.json();
  console.log(data);
}

createOrder().catch(console.error);
```

### 200 Response
```json
{
  "reference": "ORD7XK33D9"
}
```

### 400 Response
Returned when payload validation fails.
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
