---
title: Overriding text on standard dialog buttons
layout: default
---

In API V8, it became possible to override the texts “Yes”, “No”, “OK”, “Cancel”, “Close”, “Retry”, “Continue” on standard dialog buttons.

New optional parameters for button texts have been added to the methods for displaying standard dialogs.
By default, the parameters do not need to be specified; the buttons will display default values.
If you set parameters, the buttons will display the specified text.

List of modified methods:

- [`IViewManager.ShowOkPopup`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowOkPopup.htm)
- [`IViewManager.ShowClosePopup`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowClosePopup.htm)
- [`IViewManager.ShowErrorPopup`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowErrorPopup.htm)
- [`IViewManager.ShowYesNoPopup`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowYesNoPopup.htm)
- [`IViewManager.ShowOkCancelPopup`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowOkCancelPopup.htm)
- [`IViewManager.ShowRetryCancelPopup`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowRetryCancelPopup.htm)
- [`IViewManager.ShowYesNoCancelPopup`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowYesNoCancelPopup.htm)
- [`IViewManager.ShowRetryIgnoreCancelPopup`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowRetryIgnoreCancelPopup.htm)
- [`IViewManager.ShowInputDialog`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowInputDialog.htm)
- [`IViewManager.ShowCheckPermissionPopup`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowCheckPermissionPopup.htm)
- [`IViewManager.ShowCheckPermissionsPopup`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowCheckPermissionsPopup.htm)
- [`IViewManager.ShowChooserPopup`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowChooserPopup.htm)
- [`IViewManager.ShowQuantityChangerPopup`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowQuantityChangerPopup.htm)
- [`IViewManager.ShowExtendedInputDialog`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowExtendedInputDialog.htm)
- [`IViewManager.ShowKeyboard`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowKeyboard.htm)
- [`IViewManager.ShowExtendedKeyboardDialog`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowExtendedKeyboardDialog.htm)
- [`IViewManager.ShowDateNumpadPopup`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowDateNumpadPopup.htm)
- [`IViewManager.ShowCalendarPopup`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowCalendarPopup.htm)
- [`IViewManager.ShowDateTimePopup`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_UI_IViewManager_ShowDateTimePopup.htm)