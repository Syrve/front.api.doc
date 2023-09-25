---
title: It has become possible to compare returned and new orders
layout: default
---

In V7Preview4 we renamed the order return event to 
[`OrderStorned`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_OrderStorned.htm), 
adding a new argument `Guid newOrderId` to it -
identifier of the new order, which is a copy of the returned one, and with which further work will take place.
A new field was added to the order itself
[`StornedOrderId`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Orders_IOrder_StornedOrderId.htm) — 
identifier of the same returned order from which the current order was copied during the return (reversal) operation.
This field is not filled in if the order has not been returned.