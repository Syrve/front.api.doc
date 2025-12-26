---
title: Canceling plugin payment so that it is not removed from the order
layout: default
tags: v8
---

Currently, it is possible to interrupt a plugin payment by throwing an exception in the
[`Pay`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IPaymentProcessor_Pay.htm) method and
[`PaySilently`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IPaymentProcessor_PaySilently.htm).

- Throwing a [`PaymentActionCancelledException`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Exceptions_PaymentActionCancelledException.htm)
will cancel the order payment silently without any windows.
- Throwing [`PaymentActionFailedException`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Exceptions_PaymentActionFailedException.htm)
(or any other) will display an error window on the UI.

After that, the plugin payment will be marked with the status `FAILED` and deleted.

In API V8, the specified exceptions have overloads
([#1](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Exceptions_PaymentActionCancelledException__ctor_2.htm),
[#2](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Exceptions_PaymentActionFailedException__ctor_2.htm)),
allowing you to create an exception with the `bool keepInOrder` flag set to `true`, which will leave the payment that threw the exception in the order, if possible (applies only to payments; prepayments and tips will behave as before).