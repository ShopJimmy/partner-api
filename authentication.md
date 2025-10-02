---
title: Authentication
layout: default
---

## API Authentication

The API requires authentication using a client ID and client secret to obtain an access token. The access token is then used to
authenticate each request until the expiration time (3 hours at the time of writing). After expiration, request a new token.

## Get a Token

Send a POST request to `/token` with the client ID and client secret in the request body.

### POST Request to Obtain Token
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
Returned when the provided credentials fail validation.
```json
{
  "error": "Client ID must be a 32-character string"
}
```

### 403 Response
Returned when the credentials are well-formed but do not match a registered partner application.
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

----

[^1]: [It can take up to 10 minutes for changes to your site to publish after you push the changes to GitHub](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll#creating-your-site).

[Just the Docs]: https://just-the-docs.github.io/just-the-docs/
