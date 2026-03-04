---
title: Invoices
layout: default
---

## Authentication

Include the `Authorization` header with a bearer token from the [`/token` endpoint](authentication.html) to retrieve invoice summaries.

## Usage

The endpoint returns invoices issued for orders placed by your partner account. Use the optional `days` query parameter to configure the look-back window (defaults to 30 days). Each invoice summary includes originating order details and all invoiced line items.

### Request
```plaintext
GET /api/partner/v1/invoices?days=30 HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
```

### Node.js Example Request
```javascript
// Node.js 18+ example using the built-in fetch API
async function fetchInvoices() {
  const response = await fetch('https://base.shopjimmy.com/api/partner/v1/invoices?days=30', {
    headers: {
      Authorization: 'Bearer YOUR_ACCESS_TOKEN',
      'Content-Type': 'application/json'
    }
  });

  if (!response.ok) {
    throw new Error(`Invoices request failed with status ${response.status}`);
  }

  const data = await response.json();
  console.log(data);
}

fetchInvoices().catch(console.error);
```

### 200 Response
```json
[
  {
    "id": 393076,
    "sale": {
      "created_at": "2026-03-04T00:11:22.000Z",
      "reference": "ORD562DQ9",
      "po_number": "PO-1009",
      "billing_firstname": "Casey",
      "billing_lastname": "Hoffman",
      "billing_company": "Partner Service Group",
      "billing_address1": "1850 Midway Ln",
      "billing_address2": "",
      "billing_city": "SMYRNA",
      "billing_postcode": "37167",
      "billing_region": "TN",
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
  },
  {
    "id": 393057,
    "sale": {
      "created_at": "2026-03-04T02:39:22.000Z",
      "reference": "ORD56439QA",
      "po_number": "PO-1010",
      "billing_firstname": "Casey",
      "billing_lastname": "Hoffman",
      "billing_company": "Partner Service Group",
      "billing_address1": "1850 Midway Ln",
      "billing_address2": "",
      "billing_city": "SMYRNA",
      "billing_postcode": "37167",
      "billing_region": "TN",
      "billing_country": "US",
      "billing_phone": "5550100400"
    },
    "items": [
      {
        "sku": "sj-BN96-52592A",
        "qty": 1,
        "price": "79.99",
        "tax": "0.00"
      }
    ],
    "total": "79.99",
    "tax": "0.00",
    "shipping": "0.00",
    "discount": "0.00",
    "created_at": "2026-03-04T16:57:02.000Z"
  }
]
```

### 500 Response
```json
{
  "error": "There was an internal server error. Please contact administrator."
}
```
