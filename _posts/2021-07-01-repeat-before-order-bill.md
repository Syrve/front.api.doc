---
title: BeforeOrderBill and repeated bill
layout: default
---

In API version V7Preview7, notification about the order pre-check operation
[`BeforeOrderBill`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_BeforeOrderBill.htm)
will be generated when you check again.

If you need to leave the old behavior and ignore repeated bills,
then if the plugin is translated to the specified or newer version of the API, you will need to filter orders by status in the notification handler:

```cs
PluginContext.Notifications.BeforeOrderBill.Subscribe(x =>
{
    if (x.order.Status != OrderStatus.New)
        return;

    // Processing the original bill
});
```