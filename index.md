---
title: Briefing
layout: home
---

## REST API Overview
The Partner API production base URL is **https://base.shopjimmy.com/api/partner/v1**.

Access is restricted to approved applications. After onboarding, you will receive a `client_id` and `client_secret`, allowing you to begin integration against the designated development environment.

---
### Shipping
Partners must authorize our carrier accounts to tender shipments under their services. Each order request must identify the requested carrier and service level.

---
### Billing
Fulfilled orders are invoiced electronically. Billing details on each order reference the API account associated with the integration.

---

### Available Endpoints
- [Authentication](authentication.html)
- [Search Listings](search.html)
- [Listing Details](listing.html)
- [Shipping Quote](shipping-quote.html)
- [Create Order](order.html)
- [Request Order Cancellation](order-request-cancel.html)
- [Order History](orders.html)
- [Order Status](order-status.html)
- [Credits](credits.html)
- [Invoices](invoices.html)
- [Invoice Details](invoice.html)

**Explore the API with Swagger.io**

[Download the swagger.json specification]

----

[Download the swagger.json specification]: https://shopjimmy.github.io/partner-api/swagger.json
