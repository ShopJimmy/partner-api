---
title: Invoices
layout: default
---

## API Authentication

To retrieve invoices using the Partner API, include the `Authorization` header in your HTTP request.
The `Authorization` header must contain a Bearer token that you obtained from the [`/token` endpoint](authentication.html).

## Usage
Returns invoices issued for orders placed by your partner account. Use the optional `days` query parameter to control the
look-back window (defaults to 30 days). The invoice summary includes the original order metadata and each invoiced line item.

### GET Request to retrieve invoices
```plaintext
GET /api/partner/v1/invoices?days=30 HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
```

### 200 Response
```json
[
  {
    "id": 123456,
    "sale": {
      "created_at": "2024-08-07T13:24:02.000Z",
      "reference": "PAPI3ZAABG36JWR",
      "po_number": "PO-12345",
      "billing_firstname": "Robin",
      "billing_lastname": "Receiver",
      "billing_company": "Warehouse Inc",
      "billing_address1": "123 Warehouse Way",
      "billing_address2": "Suite 100",
      "billing_city": "Burnsville",
      "billing_postcode": "55337",
      "billing_region": "MN",
      "billing_country": "US",
      "billing_phone": "8005550100"
    },
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
    "created_at": "2024-08-08T21:28:00.000Z"
  }
]
```

### 500 Response
```json
{
  "error": "There was an internal server error. Please contact administrator."
}
```
