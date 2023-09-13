---
title: Address of the neighboring terminal 
layout: default
---

In API V8Preview2 it became possible to find out the address of a neighboring terminal.

A method has been added [`TryGetTerminalAddress`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_TryGetTerminalAddress.htm), which returns the address of the given terminal. The return value can be an IPv4/IPv6 address, domain name, or NetBIOS name.