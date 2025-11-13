---
title: Retrieving detailed information about discounts
layout: default
tags: v9preview1 v9
---

In API V9Preview1, a discount now has a flag that identifies whether the discount is a loyalty discount 
[`IDiscountType.IsCardLoyalty`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_IDiscountType_IsCardLoyalty.htm).
You can now also get detailed information on loyalty discounts via [`IOperationService.GetCardLoyaltyDiscounts`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_GetCardLoyaltyDiscounts.htm). Information can only be retrieved for open orders. After payment, the method will always return an empty collection.

In addition, the [`IEditSession.AddIikoCardDiscounts`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Editors_IEditSession_AddIikoCardDiscounts.htm) method has been renamed to [`IEditSession.AddCardLoyaltyDiscounts`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Editors_IEditSession_AddCardLoyaltyDiscounts.htm).