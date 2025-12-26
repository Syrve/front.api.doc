---
title: Printing a fiscal receipt before order payment
layout: default
tags: v9preview4 v9
---

Starting from V9Preview4, it is now possible to print a fiscal receipt before order payment:
[`PrintFiscalChequeBeforePaymentOrder`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_PrintFiscalChequeBeforePaymentOrder.htm).

To print a fiscal receipt before payment, the following conditions must be met:
- the amount of payments entered must be sufficient to pay for the order
- the order must contain at least one fiscalized payment for which the fiscal receipt will be printed
- the order must belong to a section where the option *"Separate printing of the fiscal receipt before payment"* is enabled:
[`FiscalChequeBeforePaymentEnabled`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Organization_Sections_IRestaurantSection_FiscalChequeBeforePaymentEnabled.htm).
- the order must not yet be fiscalized:
[`IsFiscalizedBeforePayment`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_IOrder_IsFiscalizedBeforePayment.htm)` = false`.

Also, printing a fiscal receipt before payment is not available for deliveries, except for self-pickup mode.

Before paying for the order [`PayOrder`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_PayOrder.htm) or before printing a fiscal receipt before payment [`PrintFiscalChequeBeforePaymentOrder`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_PrintFiscalChequeBeforePaymentOrder.htm), the notification [`BeforeProceedOrderPayment`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_INotificationService_BeforeProceedOrderPayment.htm) will be triggered. If one of the subscribed plugins throws an `OperationCanceledException`, the addition of the payment will be cancelled.

When printing a fiscal receipt before payment, the order will switch to a non-editable state (status [`Bill`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Orders_OrderStatus.htm)), but the payments will not be processed.

In case of success, the order is marked as *fiscalized*: [`IsFiscalizedBeforePayment`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_IOrder_IsFiscalizedBeforePayment.htm).
All order payments are marked as *fiscalized*: [`IsFiscalizedLocally`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Payments_IPaymentItem_IsFiscalizedLocally.htm).

In case of an error, the front-end will issue an exception with an error description: 
[`PrintFiscalChequeBeforePaymentOrderFailed`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Exceptions_PaymentActionFailedExceptionReason.htm).

#### Closing a fiscalized order

A fiscal receipt can be printed *only once* before payment for an order.
Subsequently, the fiscalized order can be closed: [`PayOrder`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_PayOrder.htm).
At this stage, all unprocessed payments will be completed. 
If the fiscalized order has been modified, then when executing `IOperationService.PayOrder`, the following will be printed:
- a correction receipt for return of income and a correction receipt for income (for FFD 1.1 and higher)
- a return of income receipt and an income receipt according to the current state of the order (FFD 1.05).

#### Correction receipt
A fiscalized order is considered modified if at least one of the following conditions is met:
- a pre-cheque of a fiscalized order was cancelled. I.e., a fiscal receipt was printed before payment, and then the pre-cheque was cancelled. 
- a change in one or more amounts on the receipt: cash / electronic (non-cash) / prepayment / post-payment (on credit) / counter provision. For example, if one electronic payment is replaced by another with the same amount, a correction receipt will not be printed, as in this case the details of the fiscal receipt remain unchanged. 

To track the "correction receipt" fiscal operation (for FFD 1.1 and higher), which occurs when paying for a modified fiscalized order, use:
- [`BeforeDoFfd11CorrectionOnPaymentOrderAction`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Devices_IChequeTaskProcessor_BeforeDoFfd11CorrectionOnPaymentOrderAction.htm) — a command executed before the "correction receipt" operation. Here you can check the possibility of performing the operation and add additional information to the receipt.
- [`AfterDoFfd11CorrectionOnPaymentOrderAction`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Devices_IChequeTaskProcessor_AfterDoFfd11CorrectionOnPaymentOrderAction.htm) — a command executed after the "correction receipt" operation. Its main purpose is to perform final actions after printing the correction receipt, where the [`result`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Device_Results_PostResult.htm) argument describes the result of the operation on the fiscal register.

#### Deleting a fiscalized order
When deleting a fiscalized order, a fiscal return receipt will be printed.

Examples of [`PrintFiscalChequeBeforePaymentOrder`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_PrintFiscalChequeBeforePaymentOrder.htm) calls can be found in the SDK SamplePaymentPlugin project.