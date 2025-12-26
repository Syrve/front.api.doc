---
title: User data in payments with non-integrated bank cards
layout: default
tags: v9preview1 v9
---

In the V9Preview1 API, a new `nullable` field has been added to the
[`CardPaymentItemAdditionalData`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Payments_CardPaymentItemAdditionalData.htm)
has a new `nullable` field
[`CustomData`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Payments_CardPaymentItemAdditionalData_CustomData.htm),
limited to 5000 characters. The `CardPaymentItemAdditionalData` fields are now filled in via
[constructor](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Data_Payments_CardPaymentItemAdditionalData__ctor.htm)
with 2 arguments:

```
public CardPaymentItemAdditionalData([CanBeNull] string cardNumber, [CanBeNull] string customData = null)
{...}
```

A small example of filling and reading `CustomData`:

```
/// <summary>
/// Adding an external card payment.
/// </summary>
private void AddCardExternalProcessedPayment()
{
    const bool isProcessed = true;
    var order = PluginContext.Operations.GetOrders().Last(o => o.Status == OrderStatus.New);
    var paymentType = PluginContext.Operations.GetPaymentTypes().First(x => x.Kind == PaymentTypeKind.Card && x.Name.ToUpper() == “VISA”);
    var additionalData = new CardPaymentItemAdditionalData(“123456”, “0987654321”);
    var credentials = PluginContext.Operations.GetDefaultCredentials();
    var paymentItem = PluginContext.Operations.AddExternalPaymentItem(order.ResultSum / 2, isProcessed, additionalData, null, paymentType, order, credentials);

    MessageBox.Show(((CardPaymentItemAdditionalData)paymentItem.AdditionalData).CustomData);
}
```