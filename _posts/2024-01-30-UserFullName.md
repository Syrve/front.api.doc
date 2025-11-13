---
title: IUser full name
layout: default
tags: v8
---

In API V8, the ability to obtain the user's full name specified in Syrve Office was added.

For this purpose, the [`UserFullName`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Users_UserFullName.htm) structure was added to the API, which contains 3 fields:

- string [`FirstName`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Users_UserFullName_FirstName.htm) - Employee's name;
- string [`MiddleName`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Users_UserFullName_MiddleName.htm) - Employee's patronymic;
- string [`LastName`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Users_UserFullName_LastName.htm) - Employee's last name.

Also, a new method [`GetUserFullName`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetUserFullName.htm) was added to the API - to get the full name using it, you need to pass the user object [`IUser`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Security_IUser.htm).
