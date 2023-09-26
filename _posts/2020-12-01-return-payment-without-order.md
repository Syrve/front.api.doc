---
title: When returning orders from closed cash register shifts, the order ID became available
layout: default
---

In V7Preview5 in the method
[`ReturnPaymentWithoutOrder`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IPaymentProcessor_ReturnPaymentWithoutOrder.htm)
a new argument `Nullable<Guid> orderId` has been added.

This method is implemented in the plugin code and called from SyrvePOS when an attempt is made to return an order that was paid by the plugin type in a cash register shift that is currently closed.
When returning goods separately from orders, the specified argument is `null`.
You can read more about returning orders from closed cash register shifts in the article ["Purchase returns"](https://en.syrve.help/articles/#!syrve-pos-8-5/product-return).
You can read more about the `ReturnPaymentWithoutOrder` method in the article ["External payment types"](https://syrve.github.io/front.api.doc/v6/en/PaymentProcessor.html), in the "Refund Methods" section.