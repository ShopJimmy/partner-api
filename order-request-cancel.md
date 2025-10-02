---
title: Request Order Cancellation
layout: default
---

## API Authentication

To request an order cancellation using the Partner API, include the `Authorization` header in your HTTP request.
The `Authorization` header must contain a Bearer token that you obtained from the [`/token` endpoint](authentication.html).

## Usage
Submit a cancellation request for an order that has not yet shipped. The request posts a message to our shipping team for
manual review.

### POST Request to request cancellation
```plaintext
POST /api/partner/v1/order/{order_reference}/request-cancel HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
```

No body payload is required.

### Node.js Example Request
```javascript
// Node.js 18+ example using the built-in fetch API
async function requestOrderCancellation(orderReference) {
  const response = await fetch(`https://base.shopjimmy.com/api/partner/v1/order/${orderReference}/request-cancel`, {
    method: 'POST',
    headers: {
      Authorization: 'Bearer YOUR_ACCESS_TOKEN',
      'Content-Type': 'application/json'
    }
  });

  if (!response.ok) {
    throw new Error(`Cancellation request failed with status ${response.status}`);
  }

  const data = await response.json();
  console.log(data);
}

requestOrderCancellation('PAPI3ZAABG36JWR').catch(console.error);
```

### 200 Response
```json
{
  "message": "Your request has been received."
}
```

### 500 Response
```json
{
  "error": "There was an internal server error. Please contact administrator."
}
```
