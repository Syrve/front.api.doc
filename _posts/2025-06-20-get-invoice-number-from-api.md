---
title: Obtaining an invoice number from API
layout: default
tags: v9preview6 v9
---
In API V9Preview6, the ability to obtain an invoice number from API for Bulgaria has been added.

Two new notifications were added:
- [`GetEInvoiceNumber`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_INotificationService_GetEInvoiceNumber.htm) is used to obtain the invoice number. It is sent when an organization is selected and the cashier proceeds to payment.
- [`ConfirmEInvoiceNumberProcessed`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_INotificationService_ConfirmEInvoiceNumberProcessed.htm) is used to notify the plugin whether the invoice number issued by GetEInvoiceNumberHandler was successfully used. This happens after the order is closed.

To pay for an order with an invoice, there must be exactly one subscription to both notifications: GetEInvoiceNumber and ConfirmEInvoiceNumberProcessed.
Otherwise, it will be impossible to obtain an invoice number and generate an invoice — the user will see a corresponding message.

If an error occurs while obtaining the invoice number, the payment will terminate with an error — the user will see a corresponding message.
