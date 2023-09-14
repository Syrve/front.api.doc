---
title: Tips & Charitable Contributions
layout: default
order: 16
---
## Adding Tips
The following method is used to add tips:

[IOperationService.AddDonation](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_AddDonation.htm)

```cs
IPaymentItem AddDonation([NotNull] ICredentials credentials, [NotNull] IOrder order, [NotNull] IDonationType donationType, [NotNull] IPaymentType paymentType, [CanBeNull] IPaymentItemAdditionalData additionalData, bool isProcessed, decimal donationSum);
```

- The [IDonationType](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Payments_IDonationType.htm) tips type is sent as a parameter. A list of available tips types can be obtained by invoking the [IOperationService.GetDonationTypesCompatibleWith](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_GetDonationTypesCompatibleWith.htm) method and sending an order to it as a parameter.
- The [IPaymentType](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Payments_IPaymentType.htm)payment method used to process tips is also one of the parameters. You can learn which payment methods are available for the selected type from the earlier selected type of tips — [IDonationType.PaymentTypes](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Payments_IDonationType_PaymentTypes.htm).
- The `isProcessed` parameter tells whether tips should be posted in Syrve POS or they are already posted from outside.

##### Examples

- Adding cash tips to an open order.

```cs
const bool isProcessed = true;
var order = PluginContext.Operations.GetOrders().Last(o => o.Status == OrderStatus.New);
var donationType = PluginContext.Operations.GetDonationTypesCompatibleWith(order).Last(dt => dt.PaymentTypes.Any(pt => pt.Kind == PaymentTypeKind.Cash));
var paymentType = donationType.PaymentTypes.First(x => x.Kind == PaymentTypeKind.Cash);
var credentials = PluginContext.Operations.GetCredentials();
var paymentItem = PluginContext.Operations.AddDonation(credentials, order, donationType, paymentType, null, isProcessed, order.ResultSum / 10);
order = PluginContext.Operations.GetOrderById(order.Id);
Debug.Assert(order.Donations.Contains(paymentItem));
```

- Adding bank card tips to a closed order.

```cs
const bool isProcessed = true;
var order = PluginContext.Operations.GetOrders().Last(o => o.Status == OrderStatus.Closed);
var donationType = PluginContext.Operations.GetDonationTypesCompatibleWith(order).First(dt => dt.PaymentTypes.Any(pt => pt.Kind == PaymentTypeKind.Card));
var paymentType = donationType.PaymentTypes.First(x => x.Kind == PaymentTypeKind.Card && x.Name.ToUpper() == "VISA");
var additionalData = new CardPaymentItemAdditionalData { CardNumber = "123456" };
var credentials = PluginContext.Operations.GetCredentials();
var paymentItem = PluginContext.Operations.AddDonation(credentials, order, donationType, paymentType, additionalData, isProcessed, order.ResultSum / 4);
order = PluginContext.Operations.GetOrderById(order.Id);
Debug.Assert(order.Donations.Contains(paymentItem));
```

- Adding plugin tips to an open order.

```cs
const bool isProcessed = false;
var order = PluginContext.Operations.GetOrders().Last(o => o.Status == OrderStatus.New);
var donationType = PluginContext.Operations.GetDonationTypesCompatibleWith(order).Last(dt => dt.PaymentTypes.Any(pt => pt.Kind == PaymentTypeKind.Cash));
var paymentType = donationType.PaymentTypes.First(x => x.Kind == PaymentTypeKind.Cash);
var credentials = PluginContext.Operations.GetCredentials();
var paymentItem = PluginContext.Operations.AddDonation(credentials, order, donationType, paymentType, null, isProcessed, order.ResultSum / 10);
order = PluginContext.Operations.GetOrderById(order.Id);
Debug.Assert(order.Donations.Contains(paymentItem));
```

- Adding cash tips to an open order to be processed in Syrve POS.

```cs
const bool isProcessed = false;
var order = PluginContext.Operations.GetOrders().Last(o => o.Status == OrderStatus.New);
var donationType = PluginContext.Operations.GetDonationTypesCompatibleWith(order).Last(dt => dt.PaymentTypes.Any(pt => pt.Kind == PaymentTypeKind.Cash));
var paymentType = donationType.PaymentTypes.First(x => x.Kind == PaymentTypeKind.Cash);
var credentials = PluginContext.Operations.GetCredentials();
var paymentItem = PluginContext.Operations.AddDonation(credentials, order, donationType, paymentType, null, isProcessed, order.ResultSum / 10);
order = PluginContext.Operations.GetOrderById(order.Id);
Debug.Assert(order.Donations.Contains(paymentItem));
```

- Adding bank card tips to a closed order to be processed in Syrve POS.

```cs
const bool isProcessed = false;
var order = PluginContext.Operations.GetOrders().Last(o => o.Status == OrderStatus.Closed);
var donationType = PluginContext.Operations.GetDonationTypesCompatibleWith(order).First(dt => dt.PaymentTypes.Any(pt => pt.Kind == PaymentTypeKind.Card));
var paymentType = donationType.PaymentTypes.First(x => x.Kind == PaymentTypeKind.Card && x.Name.ToUpper() == "VISA");
var additionalData = new CardPaymentItemAdditionalData { CardNumber = "123456" };
var credentials = PluginContext.Operations.GetCredentials();
var paymentItem = PluginContext.Operations.AddDonation(credentials, order, donationType, paymentType, additionalData, isProcessed, order.ResultSum / 4);
order = PluginContext.Operations.GetOrderById(order.Id);
Debug.Assert(order.Donations.Contains(paymentItem));
```

- Adding unprocessed plugin tips to an open order. It should be noted that to handle unprocessed payment methods, the plugin should support the Silent payment.

```cs
const bool isProcessed = false;
var order = PluginContext.Operations.GetOrders().Last(o => o.Status == OrderStatus.New);
var donationType = PluginContext.Operations.GetDonationTypesCompatibleWith(order).First(dt => dt.PaymentTypes.Any(pt => pt.Kind == PaymentTypeKind.External));
var paymentType = donationType.PaymentTypes.First(x => x.Kind == PaymentTypeKind.External && x.Name == "SampleApiPayment");
var additionalData = new ExternalPaymentItemAdditionalData { CustomData = Serializer.Serialize(new PaymentAdditionalData { SilentPay = true }) };
var credentials = PluginContext.Operations.GetCredentials();
var paymentItem = PluginContext.Operations.AddDonation(credentials, order, donationType, paymentType, additionalData, isProcessed, order.ResultSum / 2);
order = PluginContext.Operations.GetOrderById(order.Id);
Debug.Assert(order.Donations.Contains(paymentItem));
```

- Adding tips when processing external plugin payments. In this example, the implementation of the [IExternalPaymentProcessor.Pay](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IExternalPaymentProcessor_Pay.htm) method should be changed.

```cs
public void Pay(decimal sum, [NotNull] IOrder order, Guid paymentTypeId, Guid transactionId, [NotNull] IPointOfSale pointOfSale, [NotNull] IUser cashier,
  [NotNull] IOperationService operations, IReceiptPrinter printer, IViewManager viewManager, IPaymentDataContext context)
{
  var donationType = operations.GetDonationTypesCompatibleWith(order).FirstOrDefault(dt => dt.PaymentTypes.Any(pt => pt.Kind == PaymentTypeKind.External));
  if (donationType != null)
  {
      var paymentType = donationType.PaymentTypes.First(x => x.Kind == PaymentTypeKind.External && x.Name == "SampleApiPayment");
      var additionalData = new ExternalPaymentItemAdditionalData { CustomData = Serializer.Serialize(new PaymentAdditionalData { SilentPay = true }) };
      var credentials = operations.GetCredentials();

      operations.AddDonation(credentials, order, donationType, paymentType, additionalData, false, order.ResultSum / 2);
  }
}
```

Comments:

- The `PluginContext.Operations.GetOrders().Last(...)` expression is used in the examples, which means getting one of the most recent orders from the list. Similar expressions are used to get the types of tips and payments. To solve business tasks, a corresponding selection criterion should be used.
- `PluginContext.Operations.GetCredentials()` — hereinafter, examples show an extension method [made available](https://github.com/syrve/front.api.sdk/blob/main/sample/v6/Resto.Front.Api.SamplePlugin/OperationServiceExtensions.cs) mere demonstration of the fact that recent tips should be on the list of order tips (unless someone has removed them during the time between adding and checking).

**Note:**

- For details on the Silent payment, check the [External Payment Types](PaymentProcessor.html) article.
- For details on payments, advance payments, and prepayments, see the [Payment Actions](Payments.html) article.

## Removing Tips
The following method is used to remove tips from the order:

[IOperationService.DeleteDonation](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_DeleteDonation.htm)

```cs
void DeleteDonation([NotNull] ICredentials credentials, [NotNull] IOrder order, [NotNull] Data.Payments.IPaymentItem paymentItem);
```

- [IPaymentItem](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Payments_IPaymentItem.htm) (tips) is one of the parameters that needs to be removed.
- A list of given tips can be obtained from [IOrder.Donations](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrder_Donations.htm). 

##### Examples

- Attempt to remove tips from the order.

```cs
var order = PluginContext.Operations.GetOrders().Last(o => o.Status == OrderStatus.New || o.Status == OrderStatus.Bill || o.Status == OrderStatus.Closed);
var donationItem = order.Donations.LastOrDefault();
if (donationItem != null)
{
  PluginContext.Operations.DeleteDonation(PluginContext.Operations.GetCredentials(), order, donationItem);
}
```
