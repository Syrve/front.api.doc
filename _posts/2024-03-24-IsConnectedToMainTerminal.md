---
title: Obtaining MT connection status
layout: default
tags: v8
---

The [`IsConnectedToMainTerminal`] method has been added to Api V8 (https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_IsConnectedToMainTerminal.htm) to obtain connection status to the Main Terminal.

This method is an alternative to the notification [`ConnectionToMainTerminalChanged`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_INotificationService_ConnectionToMainTerminalChanged.htm). It is designed for cases when plugins do not have time to activate by the moment a notification about connecting the terminal to the GT is received, so that they can independently monitor the connection status.