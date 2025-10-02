---
title: Search
layout: default
---

## API Authentication

To perform search operations using the Partner API, include the `Authorization` header in your HTTP request.
The `Authorization` header must contain a Bearer token that you obtained from the [`/token` endpoint](authentication.html).

## Usage
Search queries are supplied in the `q` query parameter. Results are limited to the first 100 matches for the provided term, so
refine your query to narrow the response set. The `id` that is returned for each listing is used when creating an order.

The characters `/`, `.`, and `-` are stripped from both the query and the search index and all matches are case insensitive.

Each result contains summary pricing, availability, and compatibility information in addition to part and image metadata.

### GET Request to search parts
```plaintext
GET /api/partner/v1/search?q=PaRtNum83r HTTP/1.1
Host: base.shopjimmy.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IlE...
Content-Type: application/json
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
