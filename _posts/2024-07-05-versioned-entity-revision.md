---
title: Revisions for IVersionedEntity
layout: default
tags: v9preview3 v9
---

In API V9Preview3, the behavior of revisions for [`IVersionedEntity`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Common_IVersionedEntity.htm) has been changed.

Previously, the [`IVersionedEntity.Revision`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Common_IVersionedEntity_Revision.htm) value was shared across all terminals within the same group. The revision was assigned on the main cash register and then distributed to the subordinate terminals. Because of this, for proper synchronization of data access, it was recommended to install plugins that use data editing functions on the main cash register. When installed on other terminals, the plugin could occasionally encounter failures in applying changes. 

Now, each Syrve POS terminal maintains its own object revision. This means that unjustified change application failures caused by Syrve POS “architectural specifics” have been eliminated.
The revision can no longer be used as a criterion for comparing objects across different terminals.

Each time an object is modified, a new revision is assigned. The revision increases strictly and monotonically. However, this monotonic growth is guaranteed only within the same Syrve POS terminal database.
Objects may receive new revision values if the Syrve POS database is recreated - for example, when the main terminal is changed. To handle this, a new method has been introduced [`GetHostDatabaseId`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_GetHostDatabaseId.htm), which returns the identifier of the Syrve POS database. When the Syrve POS database is recreated, it is assigned a new identifier. Since the database identifier cannot change during the Syrve POS operation, it is sufficient to check it once at startup. If, at the next startup, the database identifier differs from the previous one, you should reload data starting from revision zero using [`GetChangedOrders`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetChangedOrders.htm), [`GetChangedDeliveryOrders`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetChangedDeliveryOrders.htm), [`GetChangedKitchenOrders`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetChangedKitchenOrders.htm), [`GetChangedReserves`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetChangedReserves.htm).

An [`example`](https://github.com/syrve/front.api.sdk/blob/master/sample/v9preview3/Resto.Front.Api.SamplePlugin/SamplePlugin.VersionedEntityRevisionHelper.cs) of tracking revision resets has been added to the [`SamplePlugin`](https://github.com/syrve/front.api.sdk/tree/master/sample/v9preview3/Resto.Front.Api.SamplePlugin).