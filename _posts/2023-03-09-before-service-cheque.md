---
title: Editing an order before printing a service receipt
layout: default
---

API V8Preview4 now allows you to edit an order immediately before printing a service receipt.

Before printing a service receipt a notification [`BeforeServiceCheque`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_INotificationService_BeforeServiceCheque.htm) is triggered. Now it allows you to change your order via [`IOperationService`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_IOperationService.htm), became available in the notification arguments.