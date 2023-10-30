---
title: Ability to change the hasFixedIikoCardDiscounts parameter
layout: default
---

Starting from API V8, we renamed the parameter hasIikoCardDiscounts methods [`CreateOrder`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Editors_IEditSession_CreateOrder.htm),
[`CreateDeliveryOrder`](https://syrve.github.io/front.api.sdk/v8/html/Overload_Resto_Front_Api_Editors_IEditSession_CreateDeliveryOrder.htm) in hasFixedIikoCardDiscounts, and also added the ability to change it.

A new method has been added to the API for this purpose [`SetOrderHasFixedIikoCardDiscounts`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Editors_IEditSession_SetOrderHasFixedIikoCardDiscounts.htm), 
with which you can change the value [`HasFixedDiscounts`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IIikoCard51OrderInfo_HasFixedDiscounts.htm) of both a delivery and a regular order.
