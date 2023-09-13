---
title: Custom properties (ProductTags) have been set for the product (IProduct)
layout: default
---

In API V8, custom properties were exposed for products [`IProduct.ProductTags`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Assortment_IProduct_ProductTags.htm), and also it became possible to get all user properties and their groups using [`GetProductTags`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetProductTags.htm) and [`GetProductTagGroups`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetProductTagGroups.htm) calls accordingly.

Retrieving specific custom properties by ID [`GetProductTagById`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetProductTagById.htm), [`TryGetProductTagById`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_TryGetProductTagById.htm).

Retrieving specific groups of custom properties by ID [`GetProductTagGroupById`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetProductTagGroupById.htm), [`TryGetProductTagGroupById`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_TryGetProductTagGroupById.htm).

Getting the group for a specific custom property is available by property [`IProductTag.Group`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Assortment_IProductTag_Group.htm).