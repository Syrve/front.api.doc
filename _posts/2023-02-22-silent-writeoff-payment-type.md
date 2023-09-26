---
title: The "No Revenue" payment type now supports silent payment
layout: default
---

In SyrvePOS version 8.4.4 and higher, it became possible to close an order containing payment type "Without revenue" remotely using the method
[`PayOrder`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_PayOrder.htm).

Also in API V8
[`WriteoffPaymentItemAdditionalData`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Payments_WriteoffPaymentItemAdditionalData.htm)
a new field has been added
[`AuthorizationUser`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Payments_WriteoffPaymentItemAdditionalData_AuthorizationUser.htm) —
"Employee or guest to whom the debit is made".
This field must be filled in if authorization by an employee or guest is selected in the “Without revenue” payment type settings.
The transferred user must have a checkmark next to “Guest” and/or “Employee” in their personal card, depending on the settings in the payment type.
If authorization is not required in the payment type, `AuthorizationUser` can be omitted.

The employee whose
[`ICredentials`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Security_ICredentials.htm)
we pass to the method of adding payment to the order, there must be the right F_COTH (Close orders at the expense of the establishment).

Usage example:

```
// An employee whose right to F_COTH (Close orders at the expense of the establishment) will be checked.
var credentials = PluginContext.Operations.AuthenticateByPin("777");
var order = PluginContext.Operations.GetOrders().Last(o => o.Status == OrderStatus.New);
var paymentType = PluginContext.Operations.GetPaymentTypes().First(x => x.Kind == PaymentTypeKind.Writeoff);
var additionalData = new WriteoffPaymentItemAdditionalData
{
    Ratio = 1,
    Reason = "Debit",
    // Employee or guest to whom the debit is made.
    AuthorizationUser = PluginContext.Operations.GetUsers().SingleOrDefault(user => user.Name == "Guest Gregory")
};
// Adding an external unsettled payment without revenue.
PluginContext.Operations.AddExternalPaymentItem(order.ResultSum, false, additionalData, null, paymentType, order, credentials);
// Or adding a regular payment without revenue.
// PluginContext.Operations.AddPaymentItem(order.ResultSum, additionalData, paymentType, order, credentials);

order = PluginContext.Operations.GetOrderById(order.Id);
// Remote payment of an order using local payments existing in the order.
PluginContext.Operations.PayOrder(credentials, order, true);
```

Documentation that may be useful:

- [Payment actions]({{ site.baseurl }}/v6/en/Payments).