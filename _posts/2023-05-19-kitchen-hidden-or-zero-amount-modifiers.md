---
title: Default Modifier information
layout: default
---

In the kitchen order version of API V8Preview6, it became possible to find out details about modifiers that should be hidden, and about remote modifiers that would be hidden if they were part of the dish.

- A property [`IsHidden`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Kitchen_IKitchenOrderModifierItem_IsHidden.htm) been added to the
kitchen modifier [`IKitchenOrderModifierItem`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Kitchen_IKitchenOrderModifierItem.htm)
which means that the modifier should not be shown either in the service receipt or on the kitchen screen (KDS).
Such a modifier has the “Hide if default quantity” checked, “Default quantity” is greater than zero, and the modifier is added to the dish with just the default quantity.

- Added a list of modifiers to the regular dish, compound dish, and compound dish component that were added to the dish with a non-zero default amount and then removed from the dish (`x0` modifiers):
	- [`IKitchenOrderProductItem.ZeroAmountModifiersData`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Kitchen_IKitchenOrderProductItem_ZeroAmountModifiersData.htm)
	- [`IKitchenOrderCompoundItem.ZeroAmountCommonModifiersData`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Kitchen_IKitchenOrderCompoundItem_ZeroAmountCommonModifiersData.htm)
	- [`IKitchenOrderCompoundItemComponent.ZeroAmountModifiersData`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Kitchen_IKitchenOrderCompoundItemComponent_ZeroAmountModifiersData.htm)