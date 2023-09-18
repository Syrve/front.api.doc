---
title: Showing relative number of modifiers
layout: default
---

In API V7 for a restaurant, it became possible to customize the way the number of group modifiers for a dish is displayed.
New property added [`bool IRestaurant.DisplayRelativeNumberOfModifiers`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Organization_IRestaurant_DisplayRelativeNumberOfModifiers.htm), indicating that in the case of `false` the quantity of the dish modifier should be displayed as an absolute quantity (as was previously), and in the case of `true` as the difference between the absolute quantity of the modifier and its default quantity.

This setting affects the display of modifiers on the screens for editing an order, a closed order, a list of deliveries, a list of orders, CDS and in service receipts.

(for more details [Showing relative number of modifiers]({{ site.baseurl }}/v7/en/DisplayRelativeNumberOfModifiers.html)).