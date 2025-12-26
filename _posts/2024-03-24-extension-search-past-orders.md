---
title: Expanding your search on PastOrders
layout: default
tags: v8
---

In Api V8, the methods [`GetPastOrdersBySum`] (https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetPastOrdersBySum.htm) and [`GetPastOrders`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetPastOrders.htm) have been modified.

For [`GetPastOrdersBySum`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetPastOrdersBySum.htm) The `paymentSum` parameter has been replaced by two parameters, `minPaymentSum` and `maxPaymentSum`, which allow you to set the price range for closed orders.

For [`GetPastOrders`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetPastOrders.htm) a new flag, `isEndOfOrderNumber`, has been added. If its status is `True`, the method will return a list of [`closed orders`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Orders_PastOrder.htm), among which there will be orders ending with the value specified in orderNumber. For example, if 400 orders have been closed in total, the request is `GetPastOrders(12, null, null, true)`, then the method will return a list of orders with numbers 12, 112, 212, 312, but not 121.

