---
title: Renaming BillCheque to ChequeExtensions.
layout: default
tags: v9preview1 v9

---

In the V9Preview1 API, the `BillCheque` class has been renamed to [`ChequeExtensions`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Cheques_ChequeExtensions.htm) to eliminate semantic inconsistencies. This class is used to extend document content and is used in callback notifications [`BillChequePrinting`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_INotificationService_BillChequePrinting.htm) and [`DocumentInvoicePrinting`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_INotificationService_DocumentInvoicePrinting.htm).