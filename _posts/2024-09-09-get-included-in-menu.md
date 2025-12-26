---
title: Ability to check if a dish in a specific size is available for sale
layout: default
tags: v9preview3 v9
---

Starting from V9Preview3, it is possible to check if a dish in a specified size is included in the menu:
[`GetIncludedInMenu`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_GetIncludedInMenu.htm).
However, before calling it, you must ensure that the dish is not deleted:
[`IProduct.IsActive`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Assortment_IProduct_IsActive.htm) must be `true`.

Previously, the API already allowed getting a list of departments where a product is available for sale in at least one size:
[`GetIncludedInMenuSectionsByProduct`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_GetIncludedInMenuSectionsByProduct.htm),
as well as the ability to trigger price retrieval from an order:
[`GetPrice`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_GetPrice.htm).
