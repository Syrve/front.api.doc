---
title: Default account for Front API plugins
layout: default
tags: v8
---

A new method has been added to the V8 API:
[`GetDefaultCredentials()`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetDefaultCredentials.htm)
for obtaining credentials associated with a new user (“Front.Api User”).

This eliminates the need to use built-in accounts or create a new account when attempting to obtain `ICredentials`
[by PIN code](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_AuthenticateByPin.htm),
if the plugin does not need to perform operations under a specific user.
