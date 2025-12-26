---
title: Renaming CashCheque to ShortenedChequeExtensions.
layout: default
tags: v9preview1 v9

---

In the V9Preview1 API, the `CashCheque` class has been renamed to [`ShortenedChequeExtensions`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Cheques_ShortenedChequeExtensions.htm) to eliminate semantic inconsistencies. This class is used to extend the content of a document and is used in the [`CashChequePrinting`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_INotificationService_CashChequePrinting.htm) callback notification. 