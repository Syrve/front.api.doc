---
title: Editing an order before checking out
layout: default
---

Starting with API V7Preview5, it became possible to edit an order immediately before the bill.

Before the order is transferred to the bill, a notification is triggered
[`BeforeOrderBill`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_BeforeOrderBill.htm).
Previously, it allowed you to cancel the transfer of an order to status
[`Bill`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Orders_OrderStatus.htm)
by throwing an `OperationCanceledException` exception at the appropriate subscriber.
It also allowed user interaction (when possible) through
[`IViewManager`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_UI_IViewManager.htm),
show different windows in SyrvePOS.

Now, in addition to everything else, we have added the ability to change the order via
[`IOperationService`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_IOperationService.htm),
made available in the notification arguments.
Thus, before fixing the cost, you can edit the composition of dishes or discounts of the order.