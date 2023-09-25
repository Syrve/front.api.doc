---
title: Ability to work with arbitrary synchronized data
layout: default
---

Starting with API V8PreviewV7, it became possible to work with arbitrary data. This data is live and synchronized between terminals for plugins with a specific `moduleId`.

[`AddOrUpdateCustomData`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_AddOrUpdateCustomData.htm) - adding/updating data by key. A limitation has been introduced on the data size, number of keys and key length recorded by plugins. The key length is no more than 512 characters. The length of the value is no more than 32768 characters. The number of keys that the plugin can record is no more than 1024.

[`TryGetCustomData`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_TryGetCustomData.htm) - obtaining data by key. If there is no data for such a key, `null` will be returned.

[`TryRemoveCustomData`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_TryRemoveCustomData.htm) - deleting data by key. If the data was not found, `false` will be returned.

[`ClearCustomData`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_ClearCustomData.htm) - deleting all keys recorded by the plugin.

[`GetAllCustomData`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetAllCustomData.htm) - get all the data for the plugin.

There is an automatic data clearing that occurs when a cash register shift is closed. If the data has not been used for a while (3 days by default, controlled by an option in config.xml - `customDataObsolescenceDuration`), then the plugin will be notified about this by subscribing to [`BeforeCustomDataClear`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_INotificationService_BeforeCustomDataClear.htm). In the notification, the plugin receives a key-data dictionary that it has not accessed for a long time. As a response, the plugin should return the keys it wants to store. If the plugin does not subscribe to the notification, the “old” data will be deleted. The latest data access update is affected by the following calls: [`AddOrUpdateCustomData`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_AddOrUpdateCustomData.htm), [`TryGetCustomData`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_TryGetCustomData.htm), [`GetAllCustomData`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetAllCustomData.htm).