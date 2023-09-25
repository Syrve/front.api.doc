---
title: More information has been added to plugin payment methods
layout: default
---

In V7Preview5 in methods
[`Pay`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IPaymentProcessor_Pay.htm) and
[`PaySilently`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IPaymentProcessor_PaySilently.htm)
replaced several arguments.

Now the `Pay` method receives `IPaymentItem paymentItem` instead of `Guid paymentTypeId`.
And the `PaySilently` method instead of `Guid orderId`, `Guid paymentTypeId` comes `IOrder order`, `IPaymentItem paymentItem`.
Thus, when making a plugin payment, when iikoFront transfers control to one of the specified methods in the plugin, you can determine whether payment is made when closing an order or an advance payment is made: the `IPaymentItem paymentItem` property is available
[`IsPrepay`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Payments_IPaymentItem_IsPrepay.htm).