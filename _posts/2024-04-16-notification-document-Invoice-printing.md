---
title: Added notification about printing a report with the ability to extend the content of the printed document.
layout: default
tags: v9preview1 v9

---

The V9Preview1 API adds functionality to insert XML markup into a report before printing to allow plugins to extend the content of the printed document.


A notification has been added to [`INotificationService`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_INotificationService.htm) [`DocumentInvoicePrinting`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_INotificationService_DocumentInvoicePrinting.htm) notification with the following parameters: 

[`IDocument`](https://syrve.github.io/front.api.sdk/v9/html/Properties_T_Resto_Front_Api_Data_Documents_IDocument.htm) - Passes the type and number of the document being printed.

[`ChequeExtensions`](ChequeExtensions) - Used to insert xml markup into the appropriate sections of the document (BeforeHeader, AfterHeader, BeforeFooter, AfterFooter).