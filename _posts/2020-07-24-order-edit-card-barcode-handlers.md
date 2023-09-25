---
title: Added processing of cards and barcodes on the order editing screen
layout: default
---

Starting with V7Preview4, the plugin can handle card rolling or barcode scanning events on the order editing screen.
This can be used, for example, for integration with an external loyalty system.

Added two notifications to `PluginContext.Notifications`:

* [`OrderEditCardSlided`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_OrderEditCardSlided.htm) — rolling card
* [`OrderEditBarcodeScanned`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_OrderEditBarcodeScanned.htm) — barcode scanning

These notifications are triggered only directly on the order editing screen (without open pop-ups) and only when swiping a card or scanning a barcode that was not recognized by the built-in processors.
Assigning a discount using a discount card registered in Syrve Office, adding a product to an order using a packaging barcode, and other similar operations work as before, but if the rolled card or scanned barcode is unknown to the Syrve POS application, then it’s the plugin’s turn.
The handler registered by the plugin will receive a barcode or [card details](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_View_CardInputDialogResult.htm) and will have to report the result - whether the notification is considered processed.
If the plugin says that the event has been processed, the process ends and handlers for other plugins will not be called.
If a plugin doesn't know what a barcode or card it is, it should return `false` so that other plugins' handlers are called.
If the event ultimately remains unprocessed, the user will receive a standard message stating that the swiped card or scanned barcode is unknown to the system.

While the notification is being processed, a progress bar is shown on the screen.
In addition to the barcode or card data, the plugin will receive the current order, local version [`IOperationService`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_IOperationService.htm) to edit the current order, as well as [`IViewManager`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_UI_IViewManager.htm) with the ability to show dialog boxes and [change text](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_UI_IViewManager_ChangeProgressBarMessage.htm) on the progressbar.

Examples of use have been added to SamplePlugin: `OrderEditScreen.AddProductByBarcode` and `OrderEditScreen.AddDiscountByCard`.