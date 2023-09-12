---
title: CashChequePrinting on reversal
layout: default
---

Notification about printing a receipt upon payment
[`CashChequePrinting`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_INotificationService_CashChequePrinting.htm),
allowing you to expand the markup of the receipt in the header and footer, starting from version SyrvePOS 8.5.1 it will also be generated when canceling (returning) an order.

Example:
```
public sealed class CashChequePrintingHandler : IDisposable
{
    private readonly IDisposable subscription;

    public CashChequePrintingHandler()
    {
        subscription = PluginContext.Notifications.CashChequePrinting.Subscribe(OnCashChequePrinting);
    }

    private static CashCheque OnCashChequePrinting(Guid orderId)
    {
        PluginContext.Log.Info("On cash cheque printing subscription.");

        var order = PluginContext.Operations.GetOrderById(orderId);
        var message = order.Status == OrderStatus.Closed
            ? $"Order #{order.Number} storno."
            : $"Order #{order.Number} pay.";
        
        return new CashCheque
        {
            BeforeCheque = new XElement(Tags.Center, message),
            AfterCheque = new XElement(Tags.QRCode, message)
        };
    }

    public void Dispose()
    {
        try
        {
            subscription.Dispose();
        }
        catch (RemotingException)
        {
            // nothing to do with the lost connection
        }
    }
}
```