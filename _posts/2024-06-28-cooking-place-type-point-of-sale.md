---
title: Retrieving a point of sale by cooking place type
layout: default
tags: v9preview2 v9
---

In API V9Preview2, a method for retrieving a point of sale by cooking place type has been added: [`TryGetPointOfSaleByCookingPlaceType`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_TryGetPointOfSaleByCookingPlaceType.htm).
The method accepts a cooking place type [`ICookingPlaceType`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Orders_ICookingPlaceType.htm) and returns a point of sale [`IPointOfSale`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Organization_IPointOfSale.htm).

If you need to retrieve the fiscal register associated with the cooking place type, you can use the [`CashRegister`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Organization_IPointOfSale_CashRegister.htm) property.


