---
title: New order property ExternalNumber
layout: default
---

Starting with API V8Preview4, a new property has been added to the order [`IOrder.ExternalNumber`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IOrder_ExternalNumber.htm), which stores the order number in the external system. 
You can change the value of this property using the new method [`IEditSession.ChangeOrderExternalNumber`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Editors_IEditSession_ChangeOrderExternalNumber.htm).

The ExternalNumber property can now be used to build OLAP sales reports for orders that came to the SyrvePOS from external systems. The ExternalNumber value is displayed in the Sales Report in the "External Order Number" column.