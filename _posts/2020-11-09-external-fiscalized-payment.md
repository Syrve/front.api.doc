---
title: It is now possible to add and delete payments fiscalized at an external cash register
layout: default
---

In V7Preview4 it became possible to work with payments fiscalized at an external cash register. They are relevant when prepayment is accepted on the site, and the fiscal receipt is printed on the site’s cloud printer. When making such a payment, all transactions corresponding to the external fiscal payment will be created in Syrve POS, but no fiscal receipt will be printed, because the receipt had already been printed previously at the external cash register.

#### Methods added:

- [`AddExternalFiscalizedPaymentItem`](https://syrve.github.io/front.api.sdk/v7/html/Overload_Resto_Front_Api_Editors_IEditSession_AddExternalFiscalizedPaymentItem.htm) — adds payment fiscalized at the external checkout to the order
- [`DeleteExternalFiscalizedPaymentItem`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Editors_IEditSession_DeleteExternalFiscalizedPaymentItem.htm) — removes such payment from the order

#### Example
```cs
var order = PluginContext.Operations.GetOrders().Last(o => (o.Status == OrderStatus.New));
var paymentType = PluginContext.Operations.GetPaymentTypes().Last(x => x.Kind == PaymentTypeKind.Card && x.Name.ToUpper() == "DINERS");
var additionalData = new CardPaymentItemAdditionalData { CardNumber = "123456" };
var credentials = PluginContext.Operations.GetCredentials();
var paymentItem = PluginContext.Operations.AddExternalFiscalizedPaymentItem(50, additionalData, paymentType, order, credentials);
// To turn such a payment into an advance payment with immediate creation of the corresponding transactions on Syrve POS,
// you can call the method IOperationService.ProcessPrepay
order = PluginContext.Operations.GetOrderById(order.Id);
PluginContext.Operations.ProcessPrepay(credentials, order, paymentItem);
```