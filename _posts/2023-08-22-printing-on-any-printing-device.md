---
title: Printing to a custom printer
layout: default
---

A new method has been added to the V8Preview7 API [`GetPrintingDeviceInfos`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetPrintingDeviceInfos.htm), which returns a list of objects with an interface [`PrintingDeviceInfo`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Device_IPrintingDeviceInfo.htm).

This interface stores two fields: [`Id`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Common_IEntity_Id.htm) - unique device ID for printing and [`FriendlyName`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Device_IPrintingDeviceInfo_FriendlyName.htm) - set in Syrve Office in Administration => Equipment Settings. [`FriendlyName`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Device_IPrintingDeviceInfo_FriendlyName.htm) created for the convenience of the plugin developer, so that it is easy to determine which printing device is being worked with.

List of devices returned [`GetPrintingDeviceInfos`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetPrintingDeviceInfos.htm), it may contain not only printers, but also fiscal recorders with printing capabilities. This list may include devices not only from the current restaurant group, but also from others, and those that have not been defined for any specific group.

Printing a document on any of these printers is possible using the method [`Print `](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_Print.htm).
