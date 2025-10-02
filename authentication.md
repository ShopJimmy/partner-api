---
title: Authentication
layout: default
---

## API Authentication

Authenticate with the Partner API by exchanging your `client_id` and `client_secret` for a short-lived bearer token. Tokens expire three hours after issuance; request a new token when the current token approaches expiration.

## Requesting a Token

Submit a `POST` request to `/token` that includes the client credentials in the request body.

### Token Request
```plaintext
POST /api/partner/v1/token HTTP/1.1
Host: base.shopjimmy.com
Content-Type: application/json
```

### Request Body
```json
{
  "client_id": "your_client_id",
  "client_secret": "your_client_secret"
}
```

### Node.js Example Request
```javascript
// Node.js 18+ example using the built-in fetch API
async function requestToken() {
  const response = await fetch('https://base.shopjimmy.com/api/partner/v1/token', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      client_id: 'your_client_id',
      client_secret: 'your_client_secret'
    })
  });

  if (!response.ok) {
    throw new Error(`Token request failed with status ${response.status}`);
  }

  const data = await response.json();
  console.log(data);
}

requestToken().catch(console.error);
```

### 200 Response
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...",
  "expires": "2024-08-08T12:34:56.000Z"
}
```

### 400 Response
The submitted credentials were malformed or failed validation.
```json
{
  "error": "Client ID must be a 32-character string"
}
```

### 403 Response
The credentials were structurally valid but did not match a registered partner application.
```json
{
  "error": "Authentication failed. Client Not found."
}
```

### 500 Response
```json
{
  "error": "Unexpected error message"
}
```
