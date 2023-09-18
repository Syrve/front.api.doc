---
title: Closing a paid delivered delivery order from the APII
layout: default
---

Starting with API V8Preview1, it became possible to close delivery directly from the plugin.

Courier delivery can be closed by calling the method
[`SetDeliveryCloseTime`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Editors_IEditSession_SetDeliveryCloseTime.htm).
In this case, the delivery order must be in the “Closed” status.
([`IDeliveryOrder.Status`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IOrder_Status.htm) ==
[`OrderStatus.Closed`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Orders_OrderStatus.htm),
that is, paid. Payment for delivery orders was made earlier, as we wrote about in
[note](https://syrve.github.io/front.api.doc/2020/12/23/pay-deliveries.html).
The delivery itself must be in the "Delivered" status.
([`IDeliveryOrder.DeliveryStatus`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_DeliveryStatus.htm) ==
[`DeliveryStatus.Delivered`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Brd_DeliveryStatus.htm)).
You can mark a delivery as delivered by sequentially calling the methods
[`SetDeliveryDelivered`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Editors_IEditSession_SetDeliveryDelivered.htm) and
[`ChangeDeliveryActualDeliverTime`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Editors_IEditSession_ChangeDeliveryActualDeliverTime.htm).

The method takes as a parameter the delivery closing time `DateTime? closeTime`.
If you set this parameter to `null`, the method will change the delivery status from "Closed" back to "Delivered".

This method does not work for self-pickup deliveries, since to close such a delivery you simply need to pay for it, as mentioned in
[note](https://syrve.github.io/front.api.doc/2020/12/23/pay-deliveries.html),
and the return of such delivery implies order reversal (i.e., refund of payments), which is not yet supported from the API.
