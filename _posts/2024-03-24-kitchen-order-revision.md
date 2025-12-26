---
title: Revision for KitchenOrder  
layout: default
tags: v8
---

В Api V8 для кухонных заказов была добавлена их ревизия - число, которое является номером последнего изменения заказа. 

A revision number has been added to the Api V8 for kitchen orders. This number represents the last change made to the order.

The revision counter is shared by all kitchen orders. Thus, if the first kitchen order was changed and its revision number became 59, the next changed kitchen order will receive revision number 60.

In this regard, a new method [`GetChangedKitchenOrders`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetChangedKitchenOrders.htm) has been added. The revision number is passed to it as an input parameter, and all kitchen orders with a number higher than that must be returned as a [`ChangedEntities`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Common_ChangedEntities_1.htm) object. <[`IKitchenOrder`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Kitchen_IKitchenOrder.htm)>, which will also contain the most recent revision among all returned orders.

