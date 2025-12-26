---
title: Nutritional value considering dish size
layout: default
tags: v9preview2 v9
---

In API V9Preview2, it became possible to retrieve the nutritional value ([`FoodValue`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_DataTransferObjects_FoodValue.htm)) of a specific dish considering its size.

The new field is available for regular order items:

- [`IOrderProductItem.FoodValue`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_IOrderProductItem_FoodValue.htm)
- [`IOrderCompoundItemComponent.FoodValue`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_IOrderCompoundItemComponent_FoodValue.htm)
- [`IOrderModifierItem.FoodValue`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_IOrderModifierItem_FoodValue.htm)

For kitchen order items:

- [`IKitchenOrderProductItem.FoodValue`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Kitchen_IKitchenOrderProductItem_FoodValue.htm)
- [`IKitchenOrderCompoundItemComponent.FoodValue`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Kitchen_IKitchenOrderCompoundItemComponent_FoodValue.htm)
- [`IKitchenOrderModifierItem.FoodValue`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Kitchen_IKitchenOrderModifierItem_FoodValue.htm)

And for order items from closed cash shifts:

- [`PastOrderItem.FoodValue`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_PastOrderItem_FoodValue.htm)

The product contains corresponding decimal fields:
([`IProduct.FoodValueCaloricity`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Assortment_IProduct_FoodValueCaloricity.htm),
[`IProduct.FoodValueCarbohydrate`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Assortment_IProduct_FoodValueCarbohydrate.htm),
[`IProduct.FoodValueFat`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Assortment_IProduct_FoodValueFat.htm),
[`IProduct.FoodValueProtein`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Assortment_IProduct_FoodValueProtein.htm)).
