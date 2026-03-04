---
title: Listing Details
layout: default
---

## Authentication

Include the `Authorization` header with a bearer token obtained from the [`/token` endpoint](authentication.html) to retrieve listing details.

## Usage

Provide the numeric `listing_id` returned from the [`/search` endpoint](search.html) to access pricing, imagery, and compatibility metadata for a single listing.

`image` is the primary image URL when available, otherwise `null`. `images` always returns an array of image objects (`url`, `default`, `label`) sourced from listing-level images and, when needed, item-level fallback images.

### Request
```plaintext
GET /api/partner/v1/listing/{listing_id} HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
```

### Node.js Example Request
```javascript
// Node.js 18+ example using the built-in fetch API
async function fetchListing(listingId) {
  const response = await fetch(`https://base.shopjimmy.com/api/partner/v1/listing/${listingId}`, {
    headers: {
      Authorization: 'Bearer YOUR_ACCESS_TOKEN',
      'Content-Type': 'application/json'
    }
  });

  if (!response.ok) {
    throw new Error(`Listing request failed with status ${response.status}`);
  }

  const data = await response.json();
  console.log(data);
}

fetchListing(336076).catch(console.error);
```

### 200 Response
```json
{
  "id": 336076,
  "title": "Hisense RUNTK0488FVZZ T-Con Board",
  "sku": "sj-RUNTK0488FVZZ",
  "price": 19.99,
  "important": "RUNTK0488FVZZ can be found on barcode sticker on board..",
  "qty": 4,
  "parts_included": [
    {
      "manufacturer": "Hisense",
      "part_number": "RUNTK0488FVZZ",
      "qty": 1,
      "actual_stock": 4,
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
  ]
}
```

### 404 Response
The requested listing identifier was not found.
```json
{
  "error": "Listing not found"
}
```

### 500 Response
```json
{
  "error": "There was an internal server error. Please contact administrator."
}
```
