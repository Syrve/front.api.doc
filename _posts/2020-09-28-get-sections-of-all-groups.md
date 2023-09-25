---
title: Added the ability to get a list of available groups and a list of branches of each group.
layout: default
---

In V7Preview4 it became possible to get a list of all groups, as well as a list of branches of any group, not just the current one.

Methods added:
- [`GetTerminalsGroups()`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetTerminalsGroups.htm) - returns all existing groups
- [`GetRestaurantSectionsByTerminalsGroup(ITerminalsGroup terminalsGroup)`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_GetRestaurantSectionsByTerminalsGroup.htm) - returns branches of the group specified in the arguments

In addition, the branch now has a link to the parent group:
[`IRestaurantSection.TerminalsGroup`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Organization_Sections_IRestaurantSection_TerminalsGroup.htm)

There will be no `GetRestaurantSections` method for getting branches of the current group in V7Preview4. You can use it instead
```cs
// Get all branches of the current group
var currentGroupSections = PluginContext.Operations.GetRestaurantSectionsByTerminalsGroup(PluginContext.Operations.GetHostTerminalsGroup());
```
