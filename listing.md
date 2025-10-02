---
title: Listing Details
layout: default
---

## API Authentication

To retrieve a listing using the Partner API, include the `Authorization` header in your HTTP request.
The `Authorization` header must contain a Bearer token that you obtained from the [`/token` endpoint](authentication.html).

## Usage
Provide the numeric `listing_id` returned by the [`/search` endpoint](search.html) to fetch pricing, images, and compatibility
details for a single listing.

### GET Request to retrieve a listing
```plaintext
GET /api/partner/v1/listing/{listing_id} HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
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
  "substitutes": [
    {
      "manufacturer": "Hisense",
      "part_number": "RUNTK0488FVZA",
      "stock": 4
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
Returned when the listing cannot be found.
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
