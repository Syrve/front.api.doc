---
title: Added the ability to send delivery on the way (without payment)
layout: default
---

Starting with V7Preview3, you can prepare your shipment for shipment, print a delivery note, assign a courier, and send it on its way.

### Innovations

- Setting up delivery payment time [PluginContext.Operations.GetHostDeliverySettings().DeliveryPaymentTimeOption](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Organization_IDeliverySettings_DeliveryPaymentTimeOption.htm) — before shipping or before closing. Payment for deliveries via the API is not yet supported, so only sending without payment will work.
- Setting the time for printing the delivery note - manually or automatically, and if automatically, at what point: [PluginContext.Operations.GetHostDeliverySettings().DeliveryBillPrintTimeOption](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Organization_IDeliverySettings_DeliveryBillPrintTimeOption.htm).
- Preparing delivery for dispatch (transfer to “Pending” status): [PluginContext.Operations.PrepareDeliveryForSending(credentials, delivery)](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_PrepareDeliveryForSending.htm). Here, work schedule and mapping restrictions will be checked, and auto-added dishes and services will be added. You can specify the optional throwOnChanges flag to cause the operation to abort if the contents or value of the order changes, this will allow the plugin to react to the changes and check if it wants to send the delivery with those changes. By default, the flag is disabled; the plugin agrees to any auto changes.
- Printing a delivery note: [PluginContext.Operations.PrintDeliveryBill(credentials, delivery)](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_PrintDeliveryBill.htm). Delivery stickers will also be printed here if the settings indicate printing them along with the invoice.
- Sending the delivery on its way: [PluginContext.Operations.SendDelivery(credentials, delivery)](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_SendDelivery.htm).
- If the delivery note is configured to automatically print at the start of preparation or upon dispatch, it will be printed when calling methods from the plugin [PrintOrderItems](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_PrintOrderItems.htm) and [SendDelivery](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_SendDelivery.htm) respectively.

### Usage example

```cs
private void CreateAndSendDelivery()
{
    if (PluginContext.Operations.GetHostDeliverySettings().DeliveryPaymentTimeOption == DeliveryPaymentTimeOption.BeforeSending)
        return; // not supported yet

    var credentials = PluginContext.Operations.AuthenticateByPin(pin);

    var delivery = CreateDelivery(false); // EditorTester.CreateDelivery method from SamplePlugin
    if (PluginContext.Operations.IsDeliveryConfirmationActive())
    {
        Debug.Assert(delivery.DeliveryStatus == DeliveryStatus.Unconfirmed);
        PluginContext.Operations.ChangeDeliveryConfirmTime(DateTime.Now, delivery, credentials);
        delivery = PluginContext.Operations.GetDeliveryOrderById(delivery.Id);
    }

    PluginContext.Operations.PrintOrderItems(credentials, delivery, delivery.Items.OfType<IOrderCookingItem>().ToList());

    Debug.Assert(delivery.DeliveryStatus == DeliveryStatus.New);
    PluginContext.Operations.PrepareDeliveryForSending(credentials, delivery);
    delivery = PluginContext.Operations.GetDeliveryOrderById(delivery.Id);
    Debug.Assert(delivery.DeliveryStatus == DeliveryStatus.Waiting);

    var courier = PluginContext.Operations.GetUsers().Single(x => x.Name == courierName);
    Debug.Assert(delivery.Courier == null);
    PluginContext.Operations.ChangeDeliveryCourier(true, delivery, courier, credentials);
    delivery = PluginContext.Operations.GetDeliveryOrderById(delivery.Id);
    Debug.Assert(Equals(delivery.Courier, courier));

    Debug.Assert(!delivery.IsPrintedBillActual);
    PluginContext.Operations.PrintDeliveryBill(credentials, delivery);
    delivery = PluginContext.Operations.GetDeliveryOrderById(delivery.Id);
    Debug.Assert(delivery.IsPrintedBillActual);

    PluginContext.Operations.SendDelivery(credentials, delivery);
    delivery = PluginContext.Operations.GetDeliveryOrderById(delivery.Id);
    Debug.Assert(delivery.DeliveryStatus == DeliveryStatus.OnWay);
}
```