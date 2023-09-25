---
title: Sign of belonging to the current department for IUser
layout: default
---

Since API V8PreviewV7:
1. Method to get all users [`IOperationService.GetUsers`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetUsers.htm) by default returns only employees of the current enterprise. To get all Syrve HQ users you need to pass the parameter `fromAllDepartments = true`.
1. Added property [`IUser.AssignedToCurrentDepartment`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Security_IUser_AssignedToCurrentDepartment.htm) — property "whether users belong to the current enterprise".