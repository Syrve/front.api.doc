---
title: Ability to work with orders during DoCheque operations in ICashRegister
layout: default
tags: v9preview1 v9
---

The V9Preview1 API now allows you to work with orders during the [`DoCheque`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Devices_ICashRegister_DoCheque.htm) operation using [`IOperationService`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_IOperationService.htm).

To change any order data, for example, to add external data using [`AddOrderExternalData`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Editors_IEditSession_AddOrderExternalData.htm), you must use [`IOperationService`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_IOperationService.htm), which is an input argument [`DoCheque`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Devices_ICashRegister_DoCheque.htm).

Since the order is already in [`status`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Orders_OrderStatus.htm) `Bill` during the execution of [`DoCheque`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Devices_ICashRegister_DoCheque.htm), most operations are unavailable.