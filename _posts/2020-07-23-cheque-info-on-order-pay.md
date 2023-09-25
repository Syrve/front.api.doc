---
title: Added the ability to specify the method of sending a receipt when paying for an order
layout: default
---

From version V7Preview4 it will be possible to specify the method of sending a receipt for order payment methods: 
[`PayOrder`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_PayOrder.htm) and 
[`PayOrderAndPayOutOnUser`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_PayOrderAndPayOutOnUser.htm).

The following methods now accept an optional parameter 
[`ChequeAdditionalInfo`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Payments_ChequeAdditionalInfo.htm) `chequeAdditionalInfo`, 
allowing you to set check generation options:

- SMS to the specified phone number
- Email to the specified email address
- Paper check