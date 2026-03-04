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

fetchInvoice(393076).catch(console.error);
```

### 200 Response
```json
{
  "id": 393076,
  "sale": {
    "created_at": "2026-03-04T00:11:22.000Z",
    "reference": "ORD562DQ9",
    "po_number": "PO-1009",
    "billing_firstname": "Casey",
    "billing_lastname": "Hoffman",
    "billing_company": "Partner Service Group",
    "billing_address1": "100 Example Ave",
    "billing_address2": "",
    "billing_city": "TESTVILLE",
    "billing_postcode": "00000",
    "billing_region": "ZZ",
    "billing_country": "US",
    "billing_phone": "5550100400"
  },
  "items": [
    {
      "sku": "sj-KIT-UN65MU6500FXZA-K3",
      "qty": 1,
      "price": "129.99",
      "tax": "0.00"
    }
  ],
  "total": "129.99",
  "tax": "0.00",
  "shipping": "0.00",
  "discount": "0.00",
  "created_at": "2026-03-04T17:03:02.000Z"
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

