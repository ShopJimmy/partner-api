---
title: Invoice Details
layout: default
---

## Authentication

Include the `Authorization` header with a bearer token from the [`/token` endpoint](authentication.html) to retrieve individual invoices.

## Usage

Provide the numeric invoice `id` returned by the [`/invoices` endpoint](invoices.html) to fetch the complete invoice record.

### Request
```plaintext
GET /api/partner/v1/invoice/{invoice_id} HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
```

### Node.js Example Request
```javascript
// Node.js 18+ example using the built-in fetch API
async function fetchInvoice(invoiceId) {
  const response = await fetch(`https://base.shopjimmy.com/api/partner/v1/invoice/${invoiceId}`, {
    headers: {
      Authorization: 'Bearer YOUR_ACCESS_TOKEN',
      'Content-Type': 'application/json'
    }
  });

  if (!response.ok) {
    throw new Error(`Invoice request failed with status ${response.status}`);
  }

  const data = await response.json();
  console.log(data);
}

fetchInvoice(123456).catch(console.error);
```

### 200 Response
```json
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
```

### 404 Response
The specified invoice identifier does not exist.
```json
{
  "error": "Invoice not found"
}
```

### 500 Response
```json
{
  "error": "There was an internal server error. Please contact administrator."
}
```
