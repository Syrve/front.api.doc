---
title: More information in orders of closed cash register shifts
layout: default
---

The V8Preview7 API has added information about whether the current order was deleted or reversed, information about which order the current order was reversed from, as well as a list of orders that are included in the same group as the current order.

- [`PastOrder.Deleted`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_PastOrder_Deleted.htm)
— was the order deleted;
- [`PastOrder.Storned`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_PastOrder_Storned.htm)
— whether the order was reversed in the cash register shift in which it was paid, or the current order is an order returning an order from a closed cash register shift (note: if the original order was not reversed in the cash register shift in which it was paid, and was returned only from a closed cash register shift, this property will be equal to `false`);
- [`PastOrder.SourceOrderInfo`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_PastOrder_SourceOrderInfo.htm)
— information about the order from which the current order was copied when canceling or returning from a closed cash register shift;
- [`PastOrder.GroupOrderInfo`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_PastOrder_GroupOrderInfo.htm)
— information about the order, the group of which includes the current order
(here there will be information either about the order from which the current order was copied during cancellation,
or about an order, the additional order of which is the current order,
or about the order that was the first as a result of division by 2 cash registers,
if the listed original orders were themselves the first in the group;
or about the current order, if it is the first (or only) in the group,
and if the original order was itself part of a group, then the group of the current order will be the group of the original order);
- [`PastOrder.GroupPastOrders`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_PastOrder_GroupPastOrders.htm)
— list of orders included in the same group as the current order
(orders having the same property [`GroupOrderInfo`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_PastOrder_GroupOrderInfo.htm), as the current order).

Thus, to understand whether the current order has been canceled at least once, you need to check its property
[`Storned`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_PastOrder_Storned.htm),
if it is equal to `false`, then you additionally need to check whether there are any among the grouped orders that refer to the current one through the property
[`SourceOrderInfo`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_PastOrder_SourceOrderInfo.htm):

```
if (pastOrder.Storned || pastOrder.GroupPastOrders.Any(o => o.SourceOrderInfo?.OrderId == pastOrder.OrderId)
    // This order has already been cancelled.
```

When trying to return again
([`StornoPastOrder`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_StornoPastOrder.htm))
already canceled order Syrve POS will check the new right `CAN_STORN_CLOSED_ORDERS_AGAIN ("F_STRNA", "Repeated return of an order from a closed cash register shift")`.