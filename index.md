---
title: Briefing
layout: home
---

## REST API Concept
The production endpoint for the Partner API is currently **https://base.shopjimmy.com/api/partner/v1**

To access the API, your application must be approved, and an account must be set up on our server. 
Once approved, you will receive a client_id and client_secret, enabling you to begin development using our designated development endpoint.

---
### Shipping 
You are required to provide access for our carrier accounts to ship using your services. 
Each order request must include the corresponding shipping service name and carrier details.

---
### Billing
All fulfilled orders will be invoiced electronically at a later time.
The billing address for each order will reference the API account information associated with your account.

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

**Interact with our API using Swagger.io**

[download our swagger.json]

----

[download our swagger.json]: https://shopjimmy.github.io/partner-api/swagger.json