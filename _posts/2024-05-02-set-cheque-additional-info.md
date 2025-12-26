---
title: Method for sending an order check
layout: default
tags: v8
---
Starting with version V8, you can set the method for sending an order check - [`ChequeAdditionalInfo`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Payments_ChequeAdditionalInfo.htm), which is passed as an argument - [`SetChequeAdditionalInfo`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_SetChequeAdditionalInfo.htm).

- [`ChequeAdditionalInfo`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Payments_ChequeAdditionalInfo.htm) - information about the check.
- [`IOrder`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Orders_IOrder.htm) - order for which check information must be set.
- [`ICredentials`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Security_ICredentials.htm) - authentication data.

Example:
```cs
Operations.AddButtonToPaymentScreen(“Add Cheque”, false, true, x =>
{
    var isChecked = !x.state.isChecked;
    var caption = isChecked ? “Added Cheque” : “Add Cheque”;

    var chequeAdditionalInfo = new ChequeAdditionalInfo(false, “+79998887766”, “mail@mail.com”, “”);

    x.os.SetChequeAdditionalInfo(chequeAdditionalInfo, x.order, x.os.GetDefaultCredentials());
    x.os.UpdatePaymentScreenButtonState(x.state.buttonId, caption, isChecked);
}, syrveIcon);
```