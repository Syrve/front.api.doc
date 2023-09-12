---
title: Changing the order of the ICredentials parameter in IOperationService methods
layout: default
---

In API V8Preview7 for unification with [`OperationServiceExtensions`](https://syrve.github.io/front.api.sdk/v8/html/Methods_T_Resto_Front_Api_Extensions_OperationServiceExtensions.htm), where parameter type [`ICredentials`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Security_ICredentials.htm) is the last of the parameters without a default value, and interface methods [`IOperationService`](https://syrve.github.io/front.api.sdk/v8/html/Methods_T_Resto_Front_Api_IOperationService.htm) this parameter is now passed last. 

Change sample:
`os.DeteteOrder(auth, order)` => `os.DeteteOrder(order, auth)`