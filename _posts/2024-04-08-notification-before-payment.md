---
title: Added notification about adding payment to order
layout: default
tags: v8

---

A notification [`BeforePaymentAdded`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_INotificationService_BeforePaymentAdded.htm) has been added to [`INotificationService`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_INotificationService.htm)
The notification is called before adding a payment to an order. If one of the subscribed plugins throws an `OperationCanceledException` 
exception, the payment addition will be canceled.