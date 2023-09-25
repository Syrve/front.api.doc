---
title: It is now possible to show a button on the order editing screen
layout: default
---

Starting with V7Preview5, the plugin will have the ability to add its own button to the bottom panel of the order editing screen (except for deliveries).

Added method [`AddButtonToOrderEditScreen`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_AddButtonToOrderEditScreen.htm) with the following parameters:

* `caption` — button text,
* `iconGeometry` — icon in the format [Path Markup](https://docs.microsoft.com/en-us/dotnet/desktop/wpf/graphics-multimedia/path-markup-syntax?view=netframeworkdesktop-4.8) (possible without an icon),
* `callback` — a button click handler in which you can show dialog boxes and make changes to the order.

Limitations of the current version:

* The method must be called in advance, before navigating to the order editing screen, for example, when launching the plugin. Once added, the button will appear whenever the edit screen is visited during the session until the plugin removes the button by calling `Dispose` on the object returned from the method. If at the time of adding or deleting a button the order editing screen is already open, the change will not be applied immediately, but the next time you go to this screen. Thus, for now the button can only be added to all orders at once. In future versions, changes will be applied on the fly, including to the current screen, which will allow the button to be shown taking into account the current state of a particular order.
* Depending on the settings, a different number of standard buttons may be displayed in the bottom panel of the order editing screen, but only one button from plugins is currently shown, even if there was space to show more. If there are several plug-in buttons, the general “Add-ons” button will be shown; clicking on it will open a dialog for selecting a specific plug-in button. This will be fixed in future versions - the buttons will take up all the free space, and the selection dialog will be needed only if there are still buttons that do not fit.
* For now, the button can only be added to the order editing screen in restaurants and fast food. The delivery order editing screen is different in that it does not save changes until the user completes editing by clicking “Save”; accordingly, the plugin does not see the intermediate unsaved state and cannot make changes to it. A plugin button without the ability to change the delivery order does not make sense, which is why it is not yet on the delivery screen.

In addition, buttons can already be added to the following screens:

* [`AddButtonToPluginsMenu`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_AddButtonToPluginsMenu.htm) — "Add-ons" menu [home screen](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Screens_IAdditionalOperationsScreen.htm)
* [`AddButtonToClosedOrderScreen`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_AddButtonToClosedOrderScreen.htm) — [closed order screen](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Screens_IClosedOrderScreen.htm), icons are now supported here too
* [`AddButtonToProductsReturnScreen`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_AddButtonToProductsReturnScreen.htm) — screen for returning products from an order closed in one of the previous checkout shifts.