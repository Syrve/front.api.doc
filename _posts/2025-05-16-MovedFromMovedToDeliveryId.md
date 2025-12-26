---
title: Extension of information about a moved delivery order
layout: default
tags: v8preview7 v8
---

Starting from V8Preview7, when moving a delivery order to a new point, it is possible to track which group the current delivery order was moved from and to. Also, the order at the old point that became the source of the current one is now known.

In addition to the existing property [`MovedToDeliveryId`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_MovedToDeliveryId.htm), new fields have appeared in the delivery order [`IDeliveryOrder`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Orders_IDeliveryOrder.htm):
- [`MovedFromDeliveryId`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_MovedFromDeliveryId.htm) — the previous ID of the delivery order before moving to a new point
- [`MovedFromTerminalGroupId`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_MovedFromTerminalGroupId.htm) — the ID of the group to which the previous delivery point belonged
- [`MovedToTerminalGroupId`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_MovedToTerminalGroupId.htm) — the ID of the group to which the current delivery point belongs

Based on this data, one can easily determine the chain of movement of a delivery order when it is moved to a new point. 

To change the values of fields containing information about the delivery order transfer, a new method [`IEditSession.ChangeDeliveryMoveIds`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Editors_IEditSession_ChangeDeliveryMoveIds.htm) has been added instead of [`IEditSession.ChangeDeliveryMovedId`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Editors_IEditSession_ChangeDeliveryMovedId.htm). This method allows editing any of the properties: `MovedFromDeliveryId`, `MovedFromTerminalGroupId`, `MovedToDeliveryId`, `MovedToTerminalGroupId`.
The previously existing method `IEditSession.ChangeDeliveryMovedId` has been removed.