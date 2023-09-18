---
title: Sign of discount payment and list of payment discounts
layout: default
---

A new property has been added to the V8Preview1 API for payment type
[`IPaymentType.IsDiscount`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Payments_IPaymentType_IsDiscount.htm),
which shows whether the payment type is discounted, i.e. carried out as a discount.

A list of discounts has also become available in the order
[`IOrder.PaymentDiscounts`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IOrder_PaymentDiscounts.htm),
which is the result of applying order payments
[`IOrder.Payments`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IOrder_Payments.htm),
that are discounted, i.e. carried out as a discount on the dishes ordered.
Such payments are non-fiscal and are fiscalized on the FR side not as payments, but as discounts by reducing the cost of dishes.