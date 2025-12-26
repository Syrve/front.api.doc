---
title: Printing a sales receipt for PastOrder
layout: default
tags: v9preview1 v9
---

Api V9Preview1 now includes the ability to print a sales receipt for [`PastOrder`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Orders_PastOrder.htm).

To do this, the field [`CafeSessionId`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_PastOrder_CafeSessionId.htm) field was added to the [`PastOrder`](https://syrve.github.io/ front.api.sdk/v9/html/T_Resto_Front_Api_Data_Orders_PastOrderItem.htm), the fields [`VatRate`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_PastOrderItem_VatRate.htm) and [`VatSum `](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_PastOrderItem_VatSum.htm) fields were added.

To print a sales receipt, you can use the new operation [`PrintCashMemoCheque`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_PrintCashMemoCheque.htm). If you just need to get the receipt layout, taking into account the settings of the selected printing device (if specified), you can use the [`GetCashMemoMarkup`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_GetCashMemoMarkup.htm) operation.


