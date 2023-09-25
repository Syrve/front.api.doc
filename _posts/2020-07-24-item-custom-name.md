---
title: Added the ability to set a custom name for order items
layout: default
---

From version V7Preview4 it will be possible to specify and receive a custom name for an order item that contains a link to `IProduct`.

Available properties for reading custom names:

- [`IOrderProductItem.ProductCustomName`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Orders_IOrderProductItem_ProductCustomName.htm) - 
ordinary dish
- [`IOrderCompoundItemComponent.ProductCustomName`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Orders_IOrderCompoundItemComponent_ProductCustomName.htm) - 
component of a compound dish
- [`IOrderServiceItem.ServiceCustomName`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Orders_IOrderServiceItem_ServiceCustomName.htm) - 
service
- [`IOrderModifierItem.ProductCustomName`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Orders_IOrderModifierItem_ProductCustomName.htm) - 
modifier for a regular dish, a compound dish, or a component of a compound dish
- [`IOrderServiceItemPeriod.ServiceCustomName`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Orders_IOrderServiceItemPeriod_ServiceCustomName.htm) - 
time service tariff

Available methods for writing custom names:

- For a regular dish
	- [`IEditSession.SetProductItemCustomName`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Editors_IEditSession_SetProductItemCustomName.htm)
	- [`OperationServiceExtensions.SetProductItemCustomName`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Extensions_OperationServiceExtensions_SetProductItemCustomName.htm)
- For a component of a compound dish
	- [`IEditSession.SetCompoundItemComponentCustomName`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Editors_IEditSession_SetCompoundItemComponentCustomName.htm)
	- [`OperationServiceExtensions.SetCompoundItemComponentCustomName`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Extensions_OperationServiceExtensions_SetCompoundItemComponentCustomName.htm)
- For service
	- [`IEditSession.SetServiceItemCustomName`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Editors_IEditSession_SetServiceItemCustomName.htm)
	- [`OperationServiceExtensions.SetServiceItemCustomName`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Extensions_OperationServiceExtensions_SetServiceItemCustomName.htm)
- For a modifier of a regular dish, a compound dish, or a component of a compound dish
	- [`IEditSession.SetOrderModifierItemCustomName`](https://syrve.github.io/front.api.sdk/v7/html/Overload_Resto_Front_Api_Editors_IEditSession_SetOrderModifierItemCustomName.htm)
	- [`OperationServiceExtensions.SetOrderModifierItemCustomName`](https://syrve.github.io/front.api.sdk/v7/html/Overload_Resto_Front_Api_Extensions_OperationServiceExtensions_SetOrderModifierItemCustomName.htm)
- For a time service tariff
	- [`IEditSession.SetServiceItemPeriodCustomName`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Editors_IEditSession_SetServiceItemPeriodCustomName.htm)
	- [`OperationServiceExtensions.SetServiceItemPeriodCustomName`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Extensions_OperationServiceExtensions_SetServiceItemPeriodCustomName.htm)

Custom names are displayed as part of the order and in the event log, and are also printed on receipts.