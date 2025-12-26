---
title: Kitchen order status
layout: default
tags: v9preview3 v9
---

In V9Preview3, the [ProcessingStatus](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Kitchen_IKitchenOrder_ProcessingStatus.htm) field was added to the [kitchen order](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Kitchen_IKitchenOrder.htm), and the [KitchenOrderProcessingStatus](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Kitchen_KitchenOrderProcessingStatus.htm) enumeration was introduced for this purpose. This field describes the current status of the kitchen order.

In standard (automatic) mode, this field will be equal to the status of the least prepared dish in the order. Alternatively, this status can be set manually. For this, the API method [SetKitchenOrderProcessingStatus](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_SetKitchenOrderProcessingStatus.htm) was created. The mode is set separately for each kitchen order. To return the order to automatic mode, pass `null` as the status value to [SetKitchenOrderProcessingStatus](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_SetKitchenOrderProcessingStatus.htm).

The kitchen order status currently does not affect dish statuses or the [appearance of the kitchen screen](https://ru.syrve.help/articles/#!syrvefront-8-9/topic-33), but it can be useful when working with delivery orders. Delivery status directly depends on the status of the corresponding kitchen order, and also do not forget about the [order packing process](https://ru.syrve.help/articles/syrvefront-8-9/syrvesouschef1/a/h2_204041478).

**Correspondence table of kitchen order and delivery statuses**

| Kitchen order status | Delivery status | Delivery status with packing enabled|
| ----------- | ----------- | ----------- |
| KitchenOrderProcessingStatus.Idle | CookingStarted | CookingStarted |
| KitchenOrderProcessingStatus.Processing1 | CookingStarted | CookingStarted |
| KitchenOrderProcessingStatus.Processing2 | CookingStarted | CookingStarted |
| KitchenOrderProcessingStatus.Processing3 | CookingStarted | CookingStarted |
| KitchenOrderProcessingStatus.Processing4 | CookingStarted | CookingCompleted | 
| KitchenOrderProcessingStatus.Processed | CookingCompleted | Packed |
| KitchenOrderProcessingStatus.Served | CookingCompleted | Packed |

Note: On the UI, the *CookingCompleted* and *Packed* statuses are displayed as *Prepared* and *Packed* respectively.

This innovation can be useful for cases where a delivery order can be closed using the stock of prepared dishes, and then the kitchen order related to the current delivery can be calmly prepared.
