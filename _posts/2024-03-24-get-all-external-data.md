---
title: Receiving complete ExternalData for a regular/kitchen order
layout: default
tags: v8
---

Two new operations have been added to Api V8: [`GetOrderAllExternalData`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetOrderAllExternalData.htm) and [`GetKitchenOrderAllExternalData`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetKitchenOrderAllExternalData.htm). These operations allow you to obtain all ExternalData for a regular order and a kitchen order, respectively.

The parameter in the operations `isPublicOnly` determines which list we want to obtain: `false` - the full list of ExternalData, `true` - the list of public ExternalData (`isPublic = true`).