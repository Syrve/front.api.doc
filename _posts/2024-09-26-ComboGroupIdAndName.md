---
title: Combo groups when creating via API
layout: default
tags: v9preview3 v9
---

In V9Preview3, the API method [AddOrderCombo](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Editors_IEditSession_AddOrderCombo.htm) was modified: the `IReadOnlyDictionary<Guid, IOrderCookingItemStub> comboItems` argument was replaced with `IReadOnlyDictionary<ComboGroupIdAndName, IOrderCookingItemStub> comboItems`. Additionally, a new class [ComboGroupIdAndName](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Orders_ComboGroupIdAndName.htm) was introduced for this change. 

This modification allows passing the desired name of the [dish group](https://ru.syrve.help/articles/syrvecard/topic-18/a/h2_829400620) of the combo in syrveFront. This is applicable when maintaining reports using the [OLAP sales report](https://ru.syrve.help/articles/syrveoffice-8-9/olap-sales) for cases where several combo groups contain the same dishes (look at the `Combo Name` field).

A similar change was also implemented for the API method [UpdateOrderComboItems](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Editors_IEditSession_UpdateOrderComboItems.htm)

**IMPORTANT**

Due to these modifications, when switching to API versions V9Preview3 and higher, it is necessary to correct the application of the aforementioned methods in the plugins used. Note that in [ComboGroupIdAndName](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Orders_ComboGroupIdAndName.htm), [GroupName](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_ComboGroupIdAndName_GroupName.htm) cannot have a `null` value. If the group name does not matter to you, simply pass an empty string.