---
title: Buttons on the document print screen 
layout: default
tags: v9preview1 v9

---

The V9Preview1 API now allows you to add buttons to the document print screen in the front so that plugins can call their own windows. 


The [`AddButtonToDocumentsScreen`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_AddButtonToDocumentsScreen.htm) method has been added with the following parameters:

* `caption` — button text,
* `iconGeometry` — icon in [Path Markup](https://docs.microsoft.com/en-us/dotnet/desktop/wpf/graphics-multimedia/path-markup-syntax?view=netframeworkdesktop-4.8) format (can be omitted),
* `callback` — button click handler, in which you can display dialog boxes and make changes.

In API V9Preview1, another type was added: `IDocument` (https://syrve.github.io/front.api.sdk/v9/html/Properties_T_Resto_Front_Api_Data_Documents_IDocument.htm), which transmits the document type and number when the button is clicked.

