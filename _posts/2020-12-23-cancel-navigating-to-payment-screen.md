---
title: Cancel the transition to the cash register screen
layout: default
---

Starting with API V7Preview5, it became possible to prohibit transition to the checkout screen.

A notification is triggered before going to the checkout screen
[`NavigatingToPaymentScreen`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_NavigatingToPaymentScreen.htm).
Previously, it allowed you to change your order via
[`IOperationService`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_IOperationService.htm),
available in notification arguments, for example, add payment.
It also allowed interaction with the user via
[`IViewManager`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_UI_IViewManager.htm),
show different windows in SyrvePOS.

Now, in addition to everything else, we have added the ability to cancel the transition to the cash register screen by generating an `OperationCanceledException` exception in the corresponding subscriber.
This may be required in cases where additional conditions are checked, the failure of which may prevent navigation to the checkout.