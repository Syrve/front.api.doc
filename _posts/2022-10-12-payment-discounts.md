---
title: Payments carried out or fiscalized as a discount
layout: default
---

In API V8 it became possible to distinguish between discount payments.

Earlier we wrote
[note]({{ site.baseurl }}/2022/03/30/payment-discounts.html)
about payments carried out as a discount.
Now there are payments that are fiscalized as a discount.
Such payments are also non-fiscal and are fiscalized on the cash register side not as payments, but as discounts by reducing the cost of dishes.
But in the server OLAP reports they are still displayed not as discounts, but as regular non-fiscal payments
(whereas payments posted as a discount pretend to be discounts in OLAP reports too).

Thus, our API has undergone such changes:

- The payment type property `IPaymentType.IsDiscount` has been removed. Instead, 2 properties were added:
	- [`IPaymentType.ProcessAsDiscount`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Payments_IPaymentType_ProcessAsDiscount.htm)
— is the payment type posted as a discount.
	- [`IPaymentType.FiscalizeAsDiscount`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Payments_IPaymentType_FiscalizeAsDiscount.htm)
— is the payment type fiscalizable as a discount.
- 2 properties have been added to the order payment element `IPaymentItem`:
	- [`IPaymentItem.IsProcessedAsDiscount`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Payments_IPaymentItem_IsProcessedAsDiscount.htm)
— whether this payment was made as a discount. This property makes sense only for prepayments:
[`IPaymentItem.IsPrepay`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Payments_IPaymentItem_IsPrepay.htm).
For regular payments, you must use the appropriate property of the payment type: 
[`IPaymentType.ProcessAsDiscount`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Payments_IPaymentType_ProcessAsDiscount.htm).
	- [`IPaymentItem.IsFiscalizedAsDiscount`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Payments_IPaymentItem_IsFiscalizedAsDiscount.htm)
— whether this payment was fiscalized as a discount. This property makes sense only for prepayments:
[`IPaymentItem.IsPrepay`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Payments_IPaymentItem_IsPrepay.htm).
For regular payments, you must use the appropriate property of the payment type: 
[`IPaymentType.FiscalizeAsDiscount`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Payments_IPaymentType_FiscalizeAsDiscount.htm).
- A new property has been added to the order discount element `IDiscountItem`
[`IDiscountItem.DiscountPaymentItem`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IDiscountItem_DiscountPaymentItem.htm)
— payment item from the list
[`IOrder.Payments`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IOrder_Payments.htm), 
carried out or fiscalized as a discount.
If the discount is not paid, this property is equal to `null`.
- The list of payment discounts is still in
[`IOrder.PaymentDiscounts`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IOrder_PaymentDiscounts.htm).