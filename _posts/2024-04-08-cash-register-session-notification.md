---
title: Added methods called before and after opening a shift on the FR
layout: default
tags: v8

---


The [`IChequeTaskProcessor`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Devices_IChequeTaskProcessor.htm) 
[`BeforeOpenSession`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Devices_IChequeTaskProcessor_BeforeOpenSession.htm) 
and [`BeforeOpenSession`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Devices_IChequeTaskProcessor_BeforeOpenSession.htm) methods have been added.
The methods are called before and after opening a shift on the fiscal register, respectively, for all plugins that implement the `IChequeTaskProcessor` interface.
If one of the subscribers throws an exception, the methods for other subscribers are not called and the entire shift opening operation ends with an error.