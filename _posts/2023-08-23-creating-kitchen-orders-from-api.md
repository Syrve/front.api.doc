---
title: Creating kitchen orders from the API
layout: default
---

A method has been added to the V8Preview7 API [`CreateKitchenOrder`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_CreateKitchenOrder.htm), that allows you to create a kitchen order via the API.

This kitchen order will not contain a "waiter part", so all fiscal transactions must be carried out through external systems.

[`CreateKitchenOrder`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_CreateKitchenOrder.htm) allows you to create an empty kitchen order. This can be useful for cases where you need to transfer external data between points ([`AddOrUpdateKitchenOrderExternalData`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_AddOrUpdateKitchenOrderExternalData.htm), [`TryGetKitchenOrderExternalDataByKey`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_TryGetKitchenOrderExternalDataByKey.htm)).

You cannot add dishes to kitchen orders created via the API. However, such a kitchen order can be cleared of all dishes. A method has been added for this purpose [`DeleteKitchenOrderItems`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_DeleteKitchenOrderItems.htm). This method only works for orders created via the API.

Also, to make it easier to distinguish whether this is a standard kitchen order or created through [`CreateKitchenOrder`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_CreateKitchenOrder.htm) in [`IKitchenOrder`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Kitchen_IKitchenOrder.htm) field has been added [`IsExternal`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Kitchen_IKitchenOrder_IsExternal.htm).

