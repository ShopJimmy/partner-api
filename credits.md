---
title: Credits
layout: default
---

## API Authentication

To retrieve credits/refunds using the Partner API, include the `Authorization` header in your HTTP request.
The `Authorization` header must contain a Bearer token that you obtained from the [`/token` endpoint](authentication.html).

## Usage
Returns credit memos created for orders placed by your partner account. Use the optional `days` query parameter to control the
look-back window (defaults to 30 days).

### GET Request to retrieve credits
```plaintext
GET /api/partner/v1/credits?days=60 HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
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
