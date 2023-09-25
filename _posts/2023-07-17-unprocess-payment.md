---
title: Refund of payments made
layout: default
---

Starting with API V8PreviewV7, it became possible to return completed payments and prepayments using the [`IOperationService.UnprocessPayment`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_UnprocessPayment.htm). 
This may be required for scenarios where payment has been initiated but the fiscal receipt has not been printed. Or when payment was made using the method [`IOperationService.ProcessPrepay`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_ProcessPrepay.htm) earlier, but now it needs to be returned.