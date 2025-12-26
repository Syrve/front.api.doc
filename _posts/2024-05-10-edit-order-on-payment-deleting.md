---
title: Added the ability to edit an order when deleting a payment via a plugin
layout: default
tags: v8

---

The V8 API now allows you to edit an order when deleting a payment via a plugin by calling the [`OnPaymentDeleting`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IPaymentProcessor_OnPaymentDeleting.htm) method.

Editing the current order is now possible via the `IOperationService operationService` argument of this method.