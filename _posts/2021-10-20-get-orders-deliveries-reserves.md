---
title: Update in methods for obtaining a list of orders, deliveries, reserves and banquets
layout: default
---

In API V7, we have made changes to the methods for obtaining a list of orders, deliveries, reservations and banquets. Including methods for obtaining entity data from a known revision.


Getting a complete list of all entities:

- The method for receiving reservations/banquets has not changed —
[`GetReserves`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetReserves.htm),
which returns them in any
[status](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Brd_ReserveStatus.htm);
- The method for obtaining a general list of orders (regular and delivery) has changed —
[`GetOrders`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetOrders.htm),
which now has optional parameters:
	- `includeDeleted` — include orders in the result
[status](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Orders_OrderStatus.htm)
`Deleted`, which were not included before;
	- `excludeDeliveryOrders` — exclude delivery orders from the results.
- The method for obtaining a list of delivery orders has changed (separate from regular ones) —
[`GetDeliveryOrders`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetDeliveryOrders.htm),
which has an optional parameter:
	- `includeDeleted` — include uncanceled deliveries in the results
([delivery status](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_DeliveryStatus.htm) != `Cancelled`) and their orders in
[status](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Orders_OrderStatus.htm)
`Deleted`, which were previously enabled by default.

The principle of obtaining entities by revision has changed.
Previously, you could only get delivery orders and only a list of objects.
The fact is that in addition to deleted orders in the `Deleted` status, there are also irrevocably deleted orders, of which only `id` remains and only at the Main Terminal.
Such permanently deleted orders are obtained by dividing the order by 2FR.
Now, when retrieving entities from a known revision, an object
[`ChangedEntities<T>`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Common_ChangedEntities_1.htm)  is returned,
which includes [list of changed entities](https://syrve.github.io/front.api.sdk/v7/html/F_Resto_Front_Api_Data_Common_ChangedEntities_1_Entities.htm)
and [maximum revision](https://syrve.github.io/front.api.sdk/v7/html/F_Resto_Front_Api_Data_Common_ChangedEntities_1_RevisionTo.htm) entities from the list.
The changed entity itself
[`ChangedEntity<T>`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Common_ChangedEntity_1.htm)
— this is the `id` and the object if this object has not been permanently deleted.

Retrieving changed entities by known revision:

- [`GetChangedOrders`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetChangedOrders.htm)
— receiving modified orders;
- [`GetChangedDeliveryOrders`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetChangedDeliveryOrders.htm)
— receiving modified deliveries;
- [`GetChangedReserves`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetChangedReserves.htm)
— receiving modified reservations/banquets.