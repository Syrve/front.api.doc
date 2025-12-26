---
title: Ability to add buttons to the delivery orders screen
layout: default
tags: v9preview4 v9
---

In API V9Preview4, the ability to add buttons to the delivery orders screen appeared, through which plugins can display their own windows. 

The [`AddButtonToDeliveriesScreen`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_AddButtonToDeliveriesScreen.htm) method was added with the following parameters:

* `string caption` — button title,
* `bool isChecked` — whether the button is selected,
* `bool isEnabled` — whether the button is available for clicking,
* `Action<(IViewManager vm, (Guid , string , bool , string ) state)> callback` — click handler for the button, in which it is possible to display dialog windows and make changes,
* `iconGeometry` — icon in [Path Markup](https://docs.microsoft.com/en-us/dotnet/desktop/wpf/graphics-multimedia/path-markup-syntax?view=netframeworkdesktop-4.8) format (optional argument).

The button click handler accepts an instance of [`IViewManager`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_UI_IViewManager.htm) for displaying windows, as well as the current state of the button - `(Guid buttonId, string caption, bool isChecked, string iconGeometry) state`.