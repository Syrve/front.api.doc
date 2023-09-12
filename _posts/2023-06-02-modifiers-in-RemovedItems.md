---
title: Removed before printing modifiers
layout: default
---

In API V8Preview6, a new list was described in the  [`RemovedItems`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IOrder_RemovedItems.htm) in the [`news`](https://syrve.github.io/front.api.doc/2023/05/18/removed-items-without-printing.html). 

Interface [`IRemovedOrderItem`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Orders_IRemovedOrderItem.htm) has been expanded. Two fields have been added to it [`Id`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IRemovedOrderItem_Id.htm) and [`ParentId`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IRemovedOrderItem_ParentId.htm). These fields will allow you to find a connection between remote modifiers and the dishes to which they relate. 

It is worth noting a separate case when deleting [`pizz`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Orders_IOrderCompoundItem.htm): since in fact pizza consists of [`PrimaryComponent`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IOrderCompoundItem_PrimaryComponent.htm) and [`SecondaryComponent`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IOrderCompoundItem_SecondaryComponent.htm),  then, when removing, common modifiers from the list [`CommonModifiers `](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IOrderCompoundItem_CommonModifiers.htm) will be divided evenly between all pizza components.