---
title: Credits
layout: default
---

## Authentication

Include the `Authorization` header with a bearer token from the [`/token` endpoint](authentication.html) to retrieve credit memos.

## Usage

The endpoint returns credits issued for orders placed by your partner account. Use the optional `days` query parameter to set the look-back window (default is 30 days).

### Request
```plaintext
GET /api/partner/v1/credits?days=60 HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
```

### Node.js Example Request
```javascript
// Node.js 18+ example using the built-in fetch API
async function fetchCredits() {
  const response = await fetch('https://base.shopjimmy.com/api/partner/v1/credits?days=60', {
    headers: {
      Authorization: 'Bearer YOUR_ACCESS_TOKEN',
      'Content-Type': 'application/json'
    }
  });

  if (!response.ok) {
    throw new Error(`Credits request failed with status ${response.status}`);
  }

  const data = await response.json();
  console.log(data);
}

fetchCredits().catch(console.error);
```

### 200 Response
```json
[
  {
    "reference": "PAPI3ZAABG36JWR",
    "amount": 25.0,
    "reason": "Missing accessories",
    "created_at": "2024-08-10T10:12:00.000Z"
  }
]
```

### 500 Response
```json
{
  "error": "There was an internal server error. Please contact administrator."
}
```
