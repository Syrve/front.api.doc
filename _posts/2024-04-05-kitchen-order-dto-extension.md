---
title: KitchenOrderDto extension
layout: default
tags: v9preview1 v9
---

In API V9Preview1, the [`KitchenOrderDto`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Kitchen_KitchenOrderDto.htm) class has been extended to achieve the most accurate configuration [kitchen order created via the API](https://syrve.github.io/front.api.doc/2023/08/23/creating-kitchen-orders-from-api.html). 

The following fields have been added:

- [`ExternalNumber`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Kitchen_KitchenOrderDto_ExternalNumber.htm) - external order number, equivalent to [`ExternalNumber`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_IOrder_ExternalNumber.htm) waiter order. 
- [`TabName`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Kitchen_KitchenOrderDto_TabName.htm) - tab name, equivalent to [`TabName`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_IOrder_TabName.htm) waiter order.
- [`OriginName`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Kitchen_KitchenOrderDto_OriginName.htm) - name of the aggregator, equivalent to [`OriginName`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_IOrder_OriginName.htm) waiter order.
- [`CookingPriority`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Kitchen_KitchenOrderDto_CookingPriority.htm) - responsible for the order in which orders are displayed on the kitchen screen. The higher the value, the higher the order priority and its position in the list.
- [`IsTopCookingPriority`] (https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Kitchen_KitchenOrderDto_IsTopCookingPriority.htm) - a special value for priority - such orders always have the highest positions in the order list and are also highlighted in a special color. 

All added fields are displayed or affect the display of the order on the kitchen screen. You can see what it looks like at the following [link](https://ru.syrve.help/articles/#!syrvefront-8-8/topic-33).



