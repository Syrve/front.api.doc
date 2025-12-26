---
title: ExternalData for kitchen order items
layout: default
tags: v8
---

In Api V8, the capabilities of specifying external information (`ExternalData`) for kitchen orders have been expanded. Now it can also be specified for order items.

The following operations have been added for this purpose:

- [`AddOrUpdateKitchenOrderItemExternalData`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_AddOrUpdateKitchenOrderItemExternalData.htm) - adding or updating ExternalData for an order item
- [`DeleteKitchenOrderItemExternalData`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_DeleteKitchenOrderItemExternalData.htm) - deleting ExternalData for the order item by the specified key
- [`TryGetKitchenOrderItemExternalDataByKey`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_TryGetKitchenOrderItemExternalDataByKey.htm) - get ExternalData at the order position by the specified key
- [`GetKitchenOrderItemAllExternalData`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetKitchenOrderItemAllExternalData.htm) - get all ExternalData at the order position

It is worth noting that for [`OLAP Sales Report`](https://ru.syrve.help/articles/syrveoffice-8-7/topic-109) for kitchen order items in ExternalData (field `Public plugin data about preparation`), ExternalData will also record the order. In case of key collisions, ExternalData items will take precedence.
