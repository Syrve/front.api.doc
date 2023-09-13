---
title: Added the ability to combine orders into a follow-up order via API
layout: default
---

Method added to API V8 [`MarkOrderAsAdditional`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Editors_IEditSession_MarkOrderAsAdditional.htm) , which allows you to assign
order [`current`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Editors_Stubs_IOrderStub.htm) follow-up order to order [`parent`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Editors_Stubs_IOrderStub.htm).
Property [`GroupOrderId`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IOrder_GroupOrderId.htm)
order current is set equal to the property [`Id`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Common_IEntity_Id.htm) order parent,
или равным null если parent == null.

Order current cannot be null, parent can be null if you need to cancel the merging of orders into an additional order.
The current order must be open, parent can be an open or closed order.
Both current and parent orders cannot be delivery orders.