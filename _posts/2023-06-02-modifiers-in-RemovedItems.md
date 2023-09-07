---
title: Removed before printing modifiers
layout: default
---

В API V8Preview6 в [`новости`](https://syrve.github.io/front.api.doc/2023/05/18/removed-items-without-printing.html) был описан новый список [`RemovedItems`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IOrder_RemovedItems.htm). 

Интерфейс [`IRemovedOrderItem`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Orders_IRemovedOrderItem.htm) был расширен. В него были добавлены два поля [`Id`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IRemovedOrderItem_Id.htm) и [`ParentId`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IRemovedOrderItem_ParentId.htm). Данные поля позволят находить связь между удаленными модификаторами и блюдами, к которым они имеют отношение. 

Стоит отметить отдельный случай при удалении [`пицц`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Orders_IOrderCompoundItem.htm): так как фактически пицца состоит из [`PrimaryComponent`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IOrderCompoundItem_PrimaryComponent.htm) и [`SecondaryComponent`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IOrderCompoundItem_SecondaryComponent.htm),  то, при удалении, общие модификаторы из списка [`CommonModifiers `](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IOrderCompoundItem_CommonModifiers.htm) будут разделяться равномерно между всеми компонентами пиццы.