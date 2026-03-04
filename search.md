---
title: Search
layout: default
---

## Authentication

Provide the `Authorization` header with a bearer token from the [`/token` endpoint](authentication.html) to execute search requests.

## Usage

Pass the search term in the `q` query parameter. Results include up to the first 100 matches; refine the term to narrow the response set. The returned `id` for each listing is required when placing orders.

During indexing, the characters `/`, `.`, and `-` are removed and matching is case-insensitive. Each result provides summary pricing, availability, compatibility details, and related imagery.

`image` is the primary image URL when available, otherwise `null`. `images` always returns an array of image objects (`url`, `default`, `label`) sourced from listing-level images and, when needed, item-level fallback images.

### Request
```plaintext
GET /api/partner/v1/search?q=PaRtNum83r HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
```

### Node.js Example Request
```javascript
// Node.js 18+ example using the built-in fetch API
async function searchListings(query) {
  const url = new URL('https://base.shopjimmy.com/api/partner/v1/search');
  url.searchParams.set('q', query);

  const response = await fetch(url, {
    headers: {
      Authorization: 'Bearer YOUR_ACCESS_TOKEN',
      'Content-Type': 'application/json'
    }
  });

  if (!response.ok) {
    throw new Error(`Search request failed with status ${response.status}`);
  }

  const data = await response.json();
  console.log(data);
}

searchListings('PaRtNum83r').catch(console.error);
```

### 200 Response
```json
[
  {
    "id": 336076,
    "title": "Hisense RUNTK0488FVZZ T-Con Board",
    "sku": "sj-RUNTK0488FVZZ",
    "price": 19.99,
    "important": "RUNTK0488FVZZ can be found on barcode sticker on board..\r\n\r\nIMPORTANT: Horizontal lines on the screen are virtually NEVER caused by a bad T-con board. Horizontal lines indicate a defective LCD panel (screen).",
    "qty": 1,
    "parts_included": [
      {
        "manufacturer": "Hisense",
        "part_number": "RUNTK0488FVZZ",
        "qty": 1,
        "actual_stock": 1,
        "image": "https://cdn.example.com/2021-10-25-20-16-52-ShopJimmy-RUNTK0488FVZZ-TOP.jpg",
        "substitutes": [
          {
            "manufacturer": "Hisense",
            "part_number": "RUNTK0488FVZA",
            "stock": 4
          }
        ]
      }
    ],
    "image": "https://cdn.example.com/2021-10-25-20-16-52-ShopJimmy-RUNTK0488FVZZ-TOP.jpg",
    "images": [
      {
        "url": "https://cdn.example.com/2021-10-25-20-16-52-ShopJimmy-RUNTK0488FVZZ-TOP.jpg",
        "default": true,
        "label": "Front"
      }
    ],
    "compatibility": [
      {
        "brand": "Hisense",
        "model": "58H6550E",
        "title": "Hisense 58H6550E",
        "versions": "Version 1, Version 2"
      }
    ],
    "substitutes": [
      {
        "manufacturer": "Hisense",
        "part_number": "RUNTK0488FVZA",
        "stock": 4
      }
    ],
    "score": 985
  }
]
```

### 400 Response
```json
{
  "error": "Missing search term."
}
```

### 500 Response
```json
{
  "error": "There was an internal server error. Please contact administrator."
}
```
