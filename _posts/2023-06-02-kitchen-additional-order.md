---
title: Follow-up orders for kitchen orders
layout: default
---

In Api V8Preview6 for kitchen orders, the ability to create follow-up orders for kitchen orders has been added. 

The formation of a kitchen follow-up order occurs after the creation of the corresponding [`follow-up order](https://en.syrve.help/articles/#!syrve-pos-8-5/taking-orders) for a closed order at the waiter station. That is, if an follow-up order is created at the waiter station, then when it is printed, the created kitchen order will also be an follow-up order for the kitchen order, for the waiter part of which the follow-up order was made.

For this purpose to the [`IKitchenOrder`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Kitchen_IKitchenOrder.htm) a field has been added [`GroupKitchenOrderId`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Kitchen_IKitchenOrder_GroupKitchenOrderId.htm), which has the id of the kitchen order for which this order is an follow-up order.

Alternatively, you can mark an order as a follow-up order using the operation [`MarkOrderAsAdditional`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Extensions_OperationServiceExtensions_MarkOrderAsAdditional.htm). Existing kitchen orders will receive the same connection as their waiter parts.
