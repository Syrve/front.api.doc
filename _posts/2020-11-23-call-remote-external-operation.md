---
title: Added the ability to interact between plugins over the network 
layout: default
---

Since V7Preview5 using the method [`CallExternalOperation`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_CallExternalOperation__2.htm) a plugin can call an operation implemented by another plugin on a different terminal.

Previously, interaction between plugins was only available within one terminal ([details]({{ site.baseurl }}{% post_url 2018-08-03-external operations%})).
The `CallExternalOperation` method now has an optional argument `terminal`, which allows you to specify on which terminal the operation should be performed.
If you leave this argument `null`, the operation will be performed on the local terminal.
In any case, the terminal on which the operation is supposed to be performed must have a plugin that has registered this external operation with [`RegisterExternalOperation`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_RegisterExternalOperation__2.htm).

The ability to remotely perform external operations will simplify the development of a plugin that requires data exchange between copies installed on different terminals.
Previously, such plugins had to find each other on the network themselves, execute and process network requests; accordingly, during installation it was necessary to configure permissions for opening/listening ports, etc.
In addition, auxiliary plugins that provide other plugins with access to data from external systems can now only be installed in a single copy on the main terminal.

To allow you to specify the terminal on which the operation should be performed, a reference book has been added to the API [terminals](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Organization_ITerminal.htm).
Related changes:

* Added method [`GetTerminals`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetTerminals.htm), that returns a list of terminals of the current restaurant..
* [`GetHostTerminal`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetHostTerminal.htm) returns local terminal ([`ITerminal`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Organization_ITerminal.htm)). 
* [`GetHostTerminalSettings`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetHostTerminalSettings.htm) returns local terminal settings ([`IHostTerminalSettings`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Organization_IHostTerminalSettings.htm)).
* The separate `GetHostTerminalCultureInfo` method has been removed; information about culture can be obtained as part of the local terminal settings.