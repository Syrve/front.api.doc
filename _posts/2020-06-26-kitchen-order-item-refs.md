---
title: Added the ability to compare dishes from regular and kitchen orders
layout: default
---

In V7Preview4, for dishes and modifiers in a kitchen order, links to the corresponding dishes and modifiers in the original order have been added.

* [`IKitchenOrderCookingItem.BaseOrderItemId`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Kitchen_IKitchenOrderCookingItem_BaseOrderItemId.htm) — corresponds [`IOrderCookingItem.Id`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Orders_IOrderCookingItem.htm),
* [`IKitchenOrderModifierItem.BaseOrderModifierId`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Kitchen_IKitchenOrderModifierItem_BaseOrderModifierId.htm) — corresponds [`IOrderModifierItem.Id`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Orders_IOrderModifierItem.htm),
* [`IKitchenOrder.BaseOrderId`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Kitchen_IKitchenOrder_BaseOrderId.htm) — corresponds [`IOrder.Id`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Orders_IOrder.htm). 