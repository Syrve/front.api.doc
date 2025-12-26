---
title: Comprehensive change to ExternalData for kitchen order
layout: default
tags: v9preview1 v9
---

Two new operations have been added to Api V9Preview1 - [`ComplexAddOrUpdateKitchenOrderAndItemsExternalData`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_ComplexAddOrUpdateKitchenOrderAndItemsExternalData.htm) and [`ComplexDeleteKitchenOrderAndItemsExternalData`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_ComplexDeleteKitchenOrderAndItemsExternalData.htm).

[`ComplexAddOrUpdateKitchenOrderAndItemsExternalData`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_ComplexAddOrUpdateKitchenOrderAndItemsExternalData.htm) - allows you to add/update `ExternalData` for a kitchen order and all its items in bulk.

[`ComplexDeleteKitchenOrderAndItemsExternalData`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_ComplexDeleteKitchenOrderAndItemsExternalData.htm) - allows you to delete all unnecessary `ExternalData` for a kitchen order and all its items at once.

These operations will be particularly useful in cases of active use of external data: operations generate a notification [`KitchenOrderChanged`] only once (https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_INotificationService_KitchenOrderChanged.htm), and are also executed faster than multiple uses of [`AddOrUpdateKitchenOrderExternalData`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_AddOrUpdateKitchenOrderExternalData.htm) / [`AddOrUpdateKitchenOrderItemExternalData`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_AddOrUpdateKitchenOrderItemExternalData.htm) / [`DeleteKitchenOrderExternalData`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_DeleteKitchenOrderExternalData.htm) / [`DeleteKitchenOrderItemExternalData`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_DeleteKitchenOrderItemExternalData.htm).
