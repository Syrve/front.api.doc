---
title: Tracking which delivery the current delivery was formed from or which it moved to when transferred to a new point.
layout: default
---

Starting from V8Preview7, new properties have been added to the delivery order [`IDeliveryOrder`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Orders_IDeliveryOrder.htm) which are filled in when transferring a delivery from point to point: the identifier of the new delivery created to transfer the current one, and the identifier of the group of the new delivery. The identifier of the old delivery from which the current delivery was formed, and the identifier of the group of the old delivery.

When transferring a delivery from point to point, the old delivery changes to the “Canceled” status and a new delivery is created at the new point by cloning the data. Field [`MovedToDeliveryId`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_MovedToDeliveryId.htm) old delivery is filled with the identifier of the new delivery. Field [`MovedToTerminalGroupId`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_MovedToTerminalGroupId.htm) is filled with the identifier of the group to which the current delivery was transferred.

For a new delivery, the ID of the old delivery is stored in the field [`MovedFromDeliveryId`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_MovedFromDeliveryId.htm) ,and the ID of the group from which the current delivery came is stored in the field [`MovedFromTerminalGroupId`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_MovedFromTerminalGroupId.htm) - If the delivery has not been moved to another point, all listed properties are `null`.

The `MovedToDeliveryId` property existed in the delivery order [`IDeliveryOrder`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Orders_IDeliveryOrder.htm) and up to V8Preview7, but under a different name - [`MovedDeliveryId`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_MovedDeliveryId.htm). In V8Preview7 it was renamed to `MovedToDeliveryId`.

To change any of the four delivery fields listed above, a method has been added to the API [`ChangeDeliveryMoveIds`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Editors_IEditSession_ChangeDeliveryMoveIds.htm).