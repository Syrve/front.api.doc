---
title: Canceling the payment process
layout: default
---

Starting with API V8, it became possible to interrupt payment/cancel an order, make/return an advance payment.

After making payments in the payment system (if one is supported by the payment type), before the start of fiscalization, a notification is triggered on the fiscal registrar in the API
[`BeforeDoCheque`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_INotificationService_BeforeDoCheque.htm).
Previously, it allowed you to change your order via
[`IOperationService`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_IOperationService.htm),
available in notification arguments, for example, add some external data
([`AddOrderExternalData`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Editors_IEditSession_AddOrderExternalData.htm).
It also allowed interaction with the user through
[`IViewManager`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_UI_IViewManager.htm),
show different windows in iikoFront if the payment was not performed in the background.

Now, in addition to everything else, we have added the ability to cancel the payment process by generating an `OperationCanceledException` exception in the corresponding subscriber.
This may be required in cases where additional conditions are checked,
failure to comply with which may prevent the closure/reversal of the order or the payment/refund of the prepayment.

Also, in the notification arguments, a list of payments has appeared on which an operation is currently taking place and for which a receipt should be printed.

The notification is generated for both fiscal and non-fiscal types of payments.
