---
title: Added the ability to select a food printer
layout: default
---

Starting with API V8, it became possible to get a dish printer.

The food printer can be obtained through [`IOperationService`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_IOperationService.htm), using method [`TryGetDishPrinter`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_TryGetDishPrinter.htm).

For each department ([`IRestaurantSection`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Organization_Sections_IRestaurantSection.htm) you can get your own separate food printer. 
If you do not specify the department for which you want to get a dish printer, then an attempt will be made to get a dish printer for the restaurant department's table by default.