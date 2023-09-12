---
title: ExternalData in kitchen order
layout: default
---

In Api V8, the ability to record ExternalData for kitchen orders has been added. This will allow you to store and transmit the necessary additional information related to the order 

For this purpose, 3 methods were created in the API:

- [`AddOrUpdateKitchenOrderExternalData(IKitchenOrder order, string key, string value)`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_AddOrUpdateKitchenOrderExternalData.htm) - Adding __value__ information with a key __key__ to the kitchen order __order__;
- [`TryGetKitchenOrderExternalDataByKey(IKitchenOrder order, string key`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_TryGetKitchenOrderExternalDataByKey.htm) - Trying to get ExternalData information from order __order__ with key __key__;
- [`DeleteKitchenOrderExternalData(IKitchenOrder order, string key)`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_DeleteKitchenOrderExternalData.htm) - Deleting a record from order __order__ with key __key__.