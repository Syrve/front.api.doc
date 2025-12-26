---
title: Receiving a kitchen order by a regular order
layout: default
tags: v9preview1 v9
---

New method added in API V9Preview1: [`TryGetKitchenOrderByOrder`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_TryGetKitchenOrderByOrder.htm) allows you to return the kitchen order for a regular order. If no kitchen order was created for this order in this group, the value will be returned as `null`.