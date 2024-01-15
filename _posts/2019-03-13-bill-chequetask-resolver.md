---
title: Added the ability to select a device for printing a receipt “Invoice” 
layout: default
---
Starting with version Syrve 6.4, API V6 added the ability to explicitly specify where a receipt of the “Invoice‎” type will be printed. 

The “Invoice‎” type check is used in some countries, for example in Latvia.
Not all models of fiscal recorders support printing receipts of the “Invoice‎” type. For devices written in [API Equipment]({{ site.baseurl }}/v6/ru/Devices.html) support for printing checks of the “Invoice‎” type is enabled via [`CashRegisterDriverParameters`](https://syrve.github.io/front.api.sdk/v6/html/Properties_T_Resto_Front_Api_Data_Device_Settings_CashRegisterDriverParameters.htm)  by specifying `IsBillTaskSupported = true`.
A check like “Invoice‎” is a command [ICashRegister.DoBillCheque()](http://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_Devices_ICashRegister_DoBillCheque.htm).
The `IsBillTaskSupported = true` configuration assumes that the Invoice number is required in the payment receipt command.
Therefore, in the results of the `DoBillCheque` command, the `CashRegisterResult.BillNumber` field must be filled in.

At the time of [printing the guest bill](https://en.syrve.help/articles/#!syrve-pos-8-6/taking-orders) the Syrve POS core polls subscribers *“At which point of sale should I print the Invoice for this order?”*.
Registering a router for printing receipts of the “Invoice‎” type is carried out using the method `INotificationsService.BillChequePosResolving()`:

- The method accepts a function with arguments `IOrder` *"order"* and `bool` *"this is the return of the bill"*.
- The function should return `IPointOfSale`: the point of sale to which the command for a check of type “Invoice‎” will be sent.
- If the function returns `null` or throws an exception, then the command for a check of type “Invoice‎” will not be sent anywhere.

Example code for a primitive case when Invoice printing should occur on the fiscal registrar of the current machine: 

```cs
var pointsOfSales = PluginContext.Operations.GetHostTerminalPointsOfSale();
var currentPos = pointsOfSales.FirstOrDefault();

if (currentPos != null)

{      subscriptions.Push(PluginContext.Notifications.BillChequePosResolving.Subscribe(_ => currentPos)); }
```


