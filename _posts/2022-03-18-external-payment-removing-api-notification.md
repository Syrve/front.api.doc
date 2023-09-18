---
title: Notifications about removing external payment from an order have been added to API V8
layout: default
---

When trying to delete an external payment, the method is called [`OnPaymentDeleting`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IPaymentProcessor_OnPaymentDeleting.htm) on the corresponding [`IPaymentProcessor`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_IPaymentProcessor.htm).

Some payment systems require payment to be made in an external system before the order is paid in Syrve, so there is a period of time when the payment has already been made in the external system, but not yet in Syrve.
Previously, the user could delete such a payment without a trace, and since Syrve considered this payment unsettled, it was deleted without cancellation, which led to an erroneous extra transaction in the external system.

Now the payment processor in the `OnPaymentDeleting` method can, at its discretion:

* do nothing - if the payment has not yet been made and does not require any action upon deletion,
* cancel payment in an external system,
* prohibit payment deletion in Syrve by generating one of the exceptions:
    * [`PaymentActionCancelledException`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Exceptions_PaymentActionCancelledException.htm) — canceling the deletion without additional messages (for example, if the plugin previously showed some question and the user chose to cancel),
    * [`PaymentActionFailedException`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Exceptions_PaymentActionFailedException.htm) — prohibition of deletion with display of an error message (for example, if it was not possible to cancel the payment in an external system).

For now, this only works if you try to remove it from the payment screen. Calling the `OnPaymentDeleting` method when deleting a payment via the API ([`DeleteExternalPaymentItem`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Editors_IEditSession_DeleteExternalPaymentItem.htm)) will be implemented later.