---
title: Options for expanding the cash register receipt
layout: default
tags: v9 v9preview2

---

The V9Preview2 API now includes the ability to change, set, and use [`ChequeSale.FiscalTags`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Device_Tasks_ChequeSale_FiscalTags.htm) for fiscal receipt items.

- Tags can be set within [`IChequeTaskProcessor`](https://syrve.github.io/front.api.doc/v6/ru/ChequeTaskProcessor.html) in the operation [`BeforeDoChequeAction()`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Devices_IChequeTaskProcessor_BeforeDoChequeAction.htm)
  [external fiscal registrar operations](https://syrve.github.io/front.api.doc/v6/ru/CashRegisters.html):
- Tags can be read as part of the operation [`ICashRegister.DoCheque()`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Devices_ICashRegister_DoCheque.htm)

In addition, you can obtain the properties of an already printed check using the [`GetDocumentFiscalTags()`] method (https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_GetDocumentFiscalTags.htm), knowing the [`FdNumber`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Device_Tasks_GetFiscalTagsTask_FdNumber.htm) of the document you are interested in.