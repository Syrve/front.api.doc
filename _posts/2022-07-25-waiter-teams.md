---
title: Teams of waiters 
layout: default
---

API V8Preview2 now has the ability to work with teams of waiters.

The waiter brigade interface has been added [`IWaiterTeam`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Device_IWaiterTeam.htm).

Methods have also become available in the API [`GetWaiterTeams`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetWaiterTeams.htm), 
which makes it possible to obtain a list of all brigades, and [`TryGetWaiterTeamForUser`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_TryGetWaiterTeamForUser.htm), 
allowing you to get the team to which the employee is assigned.

More details about the functioning of waiter teams in [`documentation`](https://en.syrve.help/articles/#!releasenotes/2022-summer/a/h2_442097477).