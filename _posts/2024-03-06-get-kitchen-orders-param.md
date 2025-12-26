---
title: Optional export of external plugin data when receiving all kitchen orders
layout: default
tags: v9preview1 v9
---

In API V9Preview1, an optional parameter _includeExternalData_ has been added to the method [`GetKitchenOrders`] (https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_GetKitchenOrders.htm), allowing external plugin data to be loaded when receiving kitchen orders.

If the parameter _includeExternalData_ is set to `true`, all kitchen orders with the [`IKitchenOrder.ExternalData`] field filled in will be returned. (https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Kitchen_IKitchenOrder_ExternalData.htm) — all external data from all plugins.

If external plugin data was not requested when receiving all kitchen orders, it can always be obtained for a specific kitchen order using the method [`TryGetKitchenOrderExternalDataByKey`] (https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_TryGetKitchenOrderExternalDataByKey.htm) .