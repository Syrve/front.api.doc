---
title: Ability to create delivery without a client
layout: default
---
Starting from V7, the ability to create a delivery without a client has been added.

From V7 in delivery [`DeliveryOrder`](http://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Orders_IDeliveryOrder.htm) client has become an optional field, and in the method [`CreateDeliveryOrder`](http://syrve.github.io/front.api.sdk/v7/html/Overload_Resto_Front_Api_Editors_IEditSession_CreateDeliveryOrder.htm) it becomes possible not to specify a client when creating a delivery.
The phone number of the anonymous client corresponds to the phone number from the delivery [`Phone`](http://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_Phone.htm).
In [`DeliveryOrder`](http://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Orders_IDeliveryOrder.htm) new property added [`ClientName`](http://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_ClientName.htm) - client name. 
This field is optional and is only used if no client is specified.
For change [`ClientName`](http://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Orders_IDeliveryOrder_ClientName.htm) new method added [`ChangeDeliveryClientName`](http://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_ChangeDeliveryClientName.htm).
