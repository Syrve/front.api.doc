---
title: Ability to edit stop list
layout: default
---

In Api V8 it became possible to add a dish in only one size or without specifying a size to the stop list [`AddProductToStopList`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_AddProductToStopList.htm).

It is now possible to see that a dish of a certain size has been added to the stop list [`IsStopListProductSellingRestricted`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_IsStopListProductSellingRestricted.htm).

To get the entire list of the stop list, you now need to use the call [`GetStopListProductsRemainingAmounts`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetStopListProductsRemainingAmounts.htm), returns a dictionary with the instance key [`ProductAndSize`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Assortment_ProductAndSize.htm), containing a specific product [`IProduct`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Assortment_IProduct.htm) and its size [`IProductSize`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Assortment_IProductSize.htm).

To remove all elements from the stop list you need to use the function [`ClearStopList`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_ClearStopList.htm).

Remove a specific dish from the stop list [`RemoveProductFromStopList`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_RemoveProductFromStopList.htm), where there must be a product/dish, but the size is optional and can be `null`.

Setting leftovers for a dish in the stop list [`SetStopListProductRemainingAmount`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_SetStopListProductRemainingAmount.htm) - It is possible to specify only values ​​from 0.001 до 999.999, size is optional and can be `null`.

Product sales limit check renamed [`CheckStopListProductsSellingRestrictions`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_CheckStopListProductsSellingRestrictions.htm) and now accepts a dictionary where the key is [`ProductAndSize`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Assortment_ProductAndSize.htm), but the value is still the same quantity.

When you try to use API calls from a user who does not have the right to clear/delete/add/set the balance on stop lists (Edit stop list and quick menu `F_EM`), an exception will be shown.

When trying to add a dish with a size to the stop list, it is always checked that the size for the dish can be applied according to its size scale, otherwise an exception will be thrown.

To track changes in stop lists, the event was renamed `ProductsRemainingAmountsChanged` -> [`StopListProductsRemainingAmountsChanged`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_INotificationService_StopListProductsRemainingAmountsChanged.htm).