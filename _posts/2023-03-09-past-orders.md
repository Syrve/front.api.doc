---
title: Receiving closed orders
layout: default
---

The V8Preview4 API has expanded the ability to obtain information about closed orders.

 
Method [`GetPastOrder`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetPastOrder.htm) has been changed to only require the Order Id.

Two new methods have also been added:
[`GetPastOrders`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetPastOrders.htm) and
[`GetPastOrdersBySum`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetPastOrdersBySum.htm).

[`GetPastOrders`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetPastOrders.htm)
allows you to search for an order by its number.
If the order number is missing, information about all closed orders for the specified time interval will be returned..

[`GetPastOrdersBySum`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetPastOrdersBySum.htm)
searches for orders based on the total payment amount (after all discounts and surcharges).

In the above methods, setting the time interval is optional.
If it is absent, the interval will be set to the time period for the last 3 months. 
