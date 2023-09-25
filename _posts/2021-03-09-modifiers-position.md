---
title: Order of dish modifiers
layout: default
---

Starting with API version V7Preview6, it will be possible to arrange simple and group dish modifiers in the same order as is done in Syrve Office.

MenuIndex field added for types [`ISimpleModifierBase`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Assortment_ISimpleModifierBase_MenuIndex.htm) and [`IGroupModifier`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Assortment_IGroupModifier_MenuIndex.htm).

You can get dish modifiers using the following methods:
- [`IOperationService.GetSimpleModifier`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetSimpleModifiers.htm)
- [`IOperationService.GetGroupModifiers`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetGroupModifiers.htm)

If dish modifiers are configured using the modifier scheme (field [`IProduct.Template`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Assortment_IProduct_Template.htm) is not `null`), then the following methods should be used:
- [`IOperationService.GetCommonSimpleModifiers`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetCommonSimpleModifiers.htm)
- [`IOperationService.GetCommonGroupModifiers`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetCommonGroupModifiers.htm)
- [`IOperationService.GetSplittableSimpleModifiers`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetSplittableSimpleModifiers.htm)
- [`IOperationService.GetSplittableGroupModifiers`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetSplittableGroupModifiers.htm)
