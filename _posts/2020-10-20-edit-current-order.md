---
title: The order editing screen allows you to make changes initiated by the plugin
layout: default
---

Starting from V7Preview5, the plugin can edit the current order via the API without receiving [`EntityAlreadyInUseException`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Exceptions_EntityAlreadyInUseException.htm) :-)

Previously, V7Preview4 added the ability to edit the current order when processing a card or barcode ([more details]({{ site.baseurl }}{% post_url 2020-07-24-order-edit-card-barcode-handlers%})).
Now a similar feature is available at any time when other operations are not performed.

To make changes to an order opened on the editing screen, the plugin needs to call the method [`TryEditCurrentOrder`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_TryEditCurrentOrder.htm) and pass into it a link to the callback, which iikoFront will call as soon as such an opportunity arises.
If there is no action, this will happen immediately, and if other operations are being performed at this moment, the callback will be called immediately upon their completion.
In any case, the `TryEditCurrentOrder` method will return control after calling the callback.
If the callback throws an exception, it will be thrown from the `TryEditCurrentOrder`.
If at the time of calling `TryEditCurrentOrder` another operation was in progress, which ultimately led to exiting the order editing screen, the callback will not be called either immediately or delayed, and the `TryEditCurrentOrder` method will generate an `OperationCanceledException` exception.

For now, the `TryEditCurrentOrder` method is only supported on the order editing screen in fast food or restaurant mode ([`IOrderEditScreen`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Screens_IOrderEditScreen.htm)).
Later there will be support for delivery editing screens, banquet screens, payment screens, etc.

A progress bar is shown while the callback is running.
The current order and local version will be sent to the callback [`IOperationService`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_IOperationService.htm) to edit the current order, as well as [`IViewManager`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_UI_IViewManager.htm) with the ability to show dialog boxes and [change text](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_UI_IViewManager_ChangeProgressBarMessage.htm) on the progressbar.

In the `EditorTester` window included in the SamplePlugin, where examples of order editing are collected, a checkbox has been added “Apply changes to order that is opened in iikoFront”; when enabled, the changes are applied to the currently open order on the screen.