---
title: Navigation to order initiated by the plugin (continued)
layout: default
---

In API V8, they took the second step towards the ability to show dialog boxes and navigate to the order at the initiative of the plugin from different screens.

In one of the [previous news]({{ site.baseurl }}/2023/02/22/navigate-to-order.html)
we talked about ways to navigate to an order using plugin buttons or from the order editing screen.
Now this navigation is supported from other screens:

- [`IAdditionalOperationsScreen`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Screens_IAdditionalOperationsScreen.htm) — Additional operations
- [`IOpenOrdersScreen`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Screens_IOpenOrdersScreen.htm) — Open orders
- [`IClosedOrdersScreen`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Screens_IClosedOrdersScreen.htm) — Closed orders
- [`IDocumentsScreen`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Screens_IDocumentsScreen.htm) — Documents
- [`IPreliminaryOrdersScreen`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Screens_IPreliminaryOrdersScreen.htm) — Preliminary orders
- [`ISectionSchemaScreen`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Screens_ISectionSchemaScreen.htm) — Section schema
- [`IOrdersByTablesScreen`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Screens_IOrdersByTablesScreen.htm) — Orders by tables
- [`IOrdersByWaiterScreen`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Screens_IOrdersByWaiterScreen.htm) — Orders by waiter
- [`ITabsByWaiterScreen`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Screens_ITabsByWaiterScreen.htm) — Tabs by waiters
- [`IDeliveriesScreen`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Screens_IDeliveriesScreen.htm) — Deliveries
- [`IReservesScreen`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Screens_IReservesScreen.htm) — Reserves

To understand that we are on a screen that supports working with the UI, we need to subscribe to the event
[`ScreenChanged`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_INotificationService_ScreenChanged.htm).

If the screen that came to the event is one of the screens listed above, you can call a new method
[`TryExecuteUiOperation`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_TryExecuteUiOperation.htm),
to which you need to pass a link to the callback, which Syrve POS will call as soon as such an opportunity arises.
If there is no action, this will happen immediately, and if other operations are being performed at this moment, the callback will be called immediately upon their completion.
In any case, the `TryExecuteUiOperation` method will return control after calling the callback.
If the callback throws an exception, it will be thrown out of `TryExecuteUiOperation`.
If at the time of calling `TryExecuteUiOperation` another operation was in progress, which ultimately led to exit from the current screen,
callback cannot be called either immediately or delayed, and the `TryExecuteUiOperation` method will throw an `OperationCanceledException` exception.

A progressbar is shown while the callback is running. The callback will be sent
[`IViewManager`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_UI_IViewManager.htm)
with the ability to show dialog boxes and
[change text on progressbar](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ChangeProgressBarMessage.htm),
and also with the possibility
[navigate to order](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_NavigateToOrderAfterOperation.htm).

Navigation is only possible in an open order.
When calling the method, the corresponding rights of the currently logged in employee at the front are checked.
The same ones that are checked when navigating by pressing the buttons on the front itself.
Navigation to delivery order is not yet possible.