---
title: Navigation to order initiated by plugin
layout: default
---

In API V8 we took the first step towards the ability to navigate between SyrvePOS screens at the initiative of a plugin.
For now, only order navigation via plug-in buttons or from the order editing screen is supported.

In [`IViewManager`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_UI_IViewManager.htm)
a new method has been added
[`NavigateToOrderAfterOperation`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_NavigateToOrderAfterOperation.htm).

It can be called in the view manager, which comes to the plugin when you click the plugin buttons:

- on SUP - Supplements —
[`AddButtonToPluginsMenu`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_AddButtonToPluginsMenu.htm)
- on the closed order screen —
[`AddButtonToClosedOrderScreen`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_AddButtonToClosedOrderScreen.htm)
- on the order return screen for closed cash register shifts —
[`AddButtonToProductsReturnScreen`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_AddButtonToProductsReturnScreen.htm)
- on the order editing screen —
[`AddButtonToOrderEditScreen`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_AddButtonToOrderEditScreen.htm)

And also in the view manager, which comes to the plugin when editing from the API the current order opened at the SyrvePOS:

- calling inside
[`TryEditCurrentOrder`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_TryEditCurrentOrder.htm)

Navigation is only possible in an open order.
When calling the method, the corresponding rights of the currently logged in employee at the SyrvePOS are checked.
The same ones that are checked when navigating by pressing the buttons on the front itself.
Navigation to delivery order is not yet possible.