---
title: Payment screen (cash screen) allows modifications initiated by a plugin
layout: default
tags: v9preview3 v9
---

Starting from V9Preview3, a plugin can edit the current order opened on the cash register via the API without receiving an
[`EntityAlreadyInUseException`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Exceptions_EntityAlreadyInUseException.htm) :-)

Previously, in V7Preview5, the ability to edit an order opened on the editing screen at any time when no other operations are being performed was added
([details]({{ site.baseurl }}{% post_url 2020-10-20-edit-current-order %})).

To make changes to an order opened on the payment screen, the plugin needs to call the method
[`TryEditCurrentPaymentScreen`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_TryEditCurrentPaymentScreen.htm)
and pass a reference to a callback that syrveFront will invoke as soon as the opportunity arises.
If there is no activity, this will happen immediately; if other operations are being performed, the callback will be invoked right after their completion.
In any case, the `TryEditCurrentPaymentScreen` method will return control after the callback is invoked.
If the callback throws an exception, it will propagate from `TryEditCurrentPaymentScreen`.
In the event that another operation was being performed at the time of the `TryEditCurrentPaymentScreen` call, which eventually led to exiting the order payment screen, the callback cannot be invoked immediately or later, and the `TryEditCurrentPaymentScreen` method will generate an `OperationCanceledException`.

A progress bar is displayed while the callback is executing.
The following will be passed to the callback:

- the current order,
- context regarding the state of the cash register screen [`IPaymentScreenUpdatedContext`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_OperationContexts_IPaymentScreenUpdatedContext.htm),
- a local version of [`IOperationService`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_IOperationService.htm) for editing the current order,
- [`IViewManager`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_UI_IViewManager.htm) with the ability to show dialog windows and
[change the text](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_UI_IViewManager_ChangeProgressBarMessage.htm) on the progress bar.

The callback has a return value, which for most users should be `null`.

Example:
```
private void EditPayScreen()
{
    PluginContext.Operations.TryEditCurrentPaymentScreen(x =>
    {
        // In "Split by Amount" mode (currently unavailable to most users), we add a new guest to the order,
        // subtract 10 from the resulting sum of existing guests, aggregate them, and add to the new guest.
        if (x.context.PaymentSplitMode == PaymentSplitMode.SplitByPrice)
        {
            var guestsSums = new List<(Guid, decimal)>();
            var newGuest = x.os.AddOrderGuest($"Guest {x.context.PaymentSplitGuestInfos.Count + 1}", x.order, x.os.GetDefaultCredentials());

            var newGuestResultSum = 0m;
            foreach (var guestInfo in x.context.PaymentSplitGuestInfos)
            {
                if (guestInfo.ResultSum > 10)
                {
                    guestsSums.Add((guestInfo.GuestId.Value, guestInfo.ResultSum - 10));
                    newGuestResultSum += 10;
                }
                else
                {
                    guestsSums.Add((guestInfo.GuestId.Value, guestInfo.ResultSum));
                }
            }
            guestsSums.Add((newGuest.Id, newGuestResultSum));
            // Return the distribution of amounts by guests in "Split by Amount" mode.
            return guestsSums;
        }

        // In "Split by Dish" mode (currently unavailable to most users), we add a new dish to the last guest.
        if (x.context.PaymentSplitMode == PaymentSplitMode.SplitByDish)
        {
            var product = x.os.GetActiveProducts().Single(p => p.Name == "Flyer dish");
            x.os.AddOrderProductItem(1m, product, x.order, x.order.Guests.Last(), null, x.os.GetDefaultCredentials());
            // Return null, as sum distribution by guests is unavailable in "Split by Dish" mode.
            return null;
        }

        // In "Whole Order" mode (default), we add a VISA payment type to the order for the full amount.
        if (x.context.PaymentSplitMode == PaymentSplitMode.WholeOrder)
        {
            x.os.AddPaymentItem(x.context.PaymentSplitGuestInfos.Single().ResultSum, null, x.os.GetPaymentTypes().First(p => p.Name.ToUpper() == "VISA"), x.order, x.os.GetDefaultCredentials());
            // Return null, as sum distribution by guests is unavailable in "Whole Order" mode.
            return null;
        }

        throw new InvalidEnumArgumentException(nameof(x.context.PaymentSplitMode), (int)x.context.PaymentSplitMode, typeof(PaymentSplitMode));
    });
}
```