---
title: Displaying the relative amount of modifiers
layout: default
order: 4
---

# Displaying the relative amount of modifiers

Considering the value of the [`IRestaurant.DisplayRelativeNumberOfModifiers`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Organization_IRestaurant_DisplayRelativeNumberOfModifiers.htm) setting in Syrve POS calculates the number of modifier portions in string form, which is displayed on the UI.

For example, a dish has the Sour cream modifier, which is included in the group of modifiers. For this modifier is run:
- the amount depends on the amount of the main dish
- modifier is free
- the default amount of modifier is 3

Then if [`IRestaurant.DisplayRelativeNumberOfModifiers`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Organization_IRestaurant_DisplayRelativeNumberOfModifiers.htm) is `true` it will display:
- _+2 Sour cream_, if increase the amount of the modifier by 2
- _- Sour cream_, if reducing the amount by 1

If [`IRestaurant.DisplayRelativeNumberOfModifiers`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Organization_IRestaurant_DisplayRelativeNumberOfModifiers.htm) is `false` the absolute amount of modifier in the dish will be displayed:
- _x5 Sour cream_, if increase the amount of the modifier by 2
- _x2 Sour cream_, if reducing the amount by 1

For the convenience of plugin developers and the possibilities to transfer the logic to their UI, here is an example of how to get a line of the number of modifiers.

The `CalculateModifierAmountString` method takes

- `decimal modifierAmount` — the amount of portions of the modifier,
- `int defaultAmount` — the amount of modifier portions by default,
- `bool hideIfDefaultAmount` — the amount of modifier portions by default "Hide if default quantity",
- `bool isPaid` — whether the modifier is a paid,
- `bool isAmountIndependentOfParentAmount` — whether the "Quantity is independent of the amount of the dish" is set for this modifier.

As a result it returns a line of the form `<sign><number>` which has to be displayed on the UI near the name of the modifier for the user can see on the screen `<sign><number> <modifier name>`.

```cs
public static string CalculateModifierAmountString(decimal modifierAmount, int defaultAmount, bool hideIfDefaultAmount, bool isPaid, bool isAmountIndependentOfParentAmount)
{
    // Set the method of displaying the amount of group modifiers of a dish.
    var showDeltaAmount = PluginContext.Operations.GetHostRestaurant().DisplayRelativeNumberOfModifiers;

    // If the option "Quantity is independent of the quantity of the dish" is enabled, we always write "+N".
    if (isAmountIndependentOfParentAmount)
        return $"+{modifierAmount}";

    // If the modifier is paid or we show the absolute number of modifiers, we write "×N".
    const string charX = "\u00D7";
    var multiplyAmountString = $"{charX}{modifierAmount}";

    if (isPaid || !showDeltaAmount)
        return multiplyAmountString;

    // Shows the relative amount of modifiers.
    var deltaAmount = modifierAmount - defaultAmount;

    switch (deltaAmount)
    {
        case 1:
            return "+";
        case -1:
            return "-";
        case 0 when hideIfDefaultAmount:
            return string.Empty;
        case 0:
            return multiplyAmountString;
        default:
            return $"{deltaAmount:+#;-#;0}";
    }
}
```