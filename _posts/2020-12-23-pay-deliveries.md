---
title: Payment for delivery order from API
layout: default
---

Starting with API V7Preview5, it became possible to pay for a delivery order directly from the plugin, without entering the checkout screen.

A delivery order can be paid by calling the method
[`PayOrder`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_PayOrder.htm).
In this case, the cost of the delivery order must be fully covered by payments.
If the delivery is by courier and has not been sent, this method will automatically transfer the delivery to the “Waiting for dispatch” status.
If this is a pick-up, then paying for the delivery order from the plugin will automatically close such delivery.
The transfer of courier delivery from the "Delivered" status to the "Closed" status will be implemented later.
