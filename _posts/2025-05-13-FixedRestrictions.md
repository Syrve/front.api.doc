---
title: Ability to prevent the front-end from changing delivery duration and zone values passed from API
layout: default
tags: v9preview1 v9
---

Starting from V9Preview1, it is possible to prohibit the front-end from changing delivery duration and zone values calculated by an external OHAC and passed from the API.

A new field [`FixedRestrictions`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_FixedRestrictions.htm) has been added to the delivery order [`IDeliveryOrder`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Orders_IDeliveryOrder.htm). This field allows you to either enable the front-end to replace the delivery duration [`Duration`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_Duration.htm) and zone [`Zone`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_Zone.htm) values with those the front-end receives from its "own" OHAC, or, conversely, prevent editing of the `Duration` and `Zone` fields, keeping the data received from the API.

To utilize this new functionality when creating a delivery via API, you must pass a non-empty delivery duration value in the `TimeSpan? duration` parameter and pass `true` in the `bool fixedRestrictions` parameter of the [`IEditSession.CreateDeliveryOrder`](https://syrve.github.io/front.api.sdk/v9/html/Overload_Resto_Front_Api_Editors_IEditSession_CreateDeliveryOrder.htm) method arguments. If you also need to set and fix the delivery zone, the `string zone` parameter must also contain a non-empty value. When `IDeliveryOrder.FixedRestrictions = true`, the front-end does not trigger a OHAC check for this delivery and, thus, keeps the delivery duration and zone values unchanged.

The `IDeliveryOrder.FixedRestrictions` value is automatically reset to `false` by the front-end only when the service mode [`IOrderType`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Organization_IOrderType.htm) is changed.

The value of the `IDeliveryOrder.FixedRestrictions` field can be modified using the new method [`IEditSession.ChangeDeliveryFixedRestrictions`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Editors_IEditSession_ChangeDeliveryFixedRestrictions.htm).

When calling delivery order editing methods — such as changing the delivery duration [`IEditSession.ChangeDeliveryDuration`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Editors_IEditSession_ChangeDeliveryDuration.htm) or the zone [`IEditSession.ChangeDeliveryZone`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Editors_IEditSession_ChangeDeliveryZone.htm) — the front-end will apply the new parameters while leaving `IDeliveryOrder.FixedRestrictions` unchanged.

If a delivery with `IDeliveryOrder.FixedRestrictions = true` reaches the (legacy) Call Center and is modified in a way that triggers a OHAC check, the `IDeliveryOrder.FixedRestrictions` flag is reset to `false`, and the fields [`IDeliveryOrder.Duration`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_Duration.htm) and [`IDeliveryOrder.Zone`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_Zone.htm) will be edited — they will store the values returned by the OHAC.