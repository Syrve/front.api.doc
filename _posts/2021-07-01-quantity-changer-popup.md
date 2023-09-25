---
title: Window for working with a group of elements and their quantity
layout: default
---

In API version V7Preview6, a new window was added to make it easier to work with the quantity of a group of elements
[`IViewManager.ShowQuantityChangerPopup`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_UI_IViewManager_ShowQuantityChangerPopup.htm).

![quantity-changer-popup-1](../../../img/viewmanager/quantity-changer-popup-1.PNG)

The method accepts

- `string title` — window title,
- `string text` — text with description,
- `int minimalQuantity` — total minimum possible number for the group,
- `int maximalQuantity` — total maximum possible number for the group,
- `IReadOnlyCollection<(string name, int quantity, int minimalQuantity, int maximalQuantity)> items` — the list of elements itself.

Each element is

- `string name` — his name,
- `int quantity` — current item quantity,
- `int minimalQuantity` — minimum possible number of element,
- `int maximalQuantity` — maximum possible number of element.

As a result, the method returns a list `IReadOnlyCollection<int>`, which contains the number of each element in the order corresponding to the order in the received argument `items`.

The `+` and `-` buttons allow you to increase or decrease the number of elements by one.
The central area of ​​the element with its name is also clickable and allows you to open a digital popup for advanced editing of the quantity of the selected element.
To the right of the name is the current quantity of the element.

![quantity-changer-popup-3](../../../img/viewmanager/quantity-changer-popup-3.PNG)

In the case where the minimum possible quantity of each element is zero,
the maximum possible quantity of each element is equal to the maximum possible quantity for the group,
the current quantity of one of the elements is equal to the maximum possible quantity for the group,
and the current number of remaining elements is zero,
clicking on the central area of ​​the remaining elements does not open the digital popup,
a completely transfers the quantity of the previously selected element to the current element.

![quantity-changer-popup-2](../../../img/viewmanager/quantity-changer-popup-2.PNG)

##### Example

```cs
private static void ShowListWithQuantitiesPopup(IViewManager viewManager)
{
    const string title = "Santa's gift";
    const string hintText = "Choose sweets";
    var list = new List<(string name, int quantity, int minimalQuantity, int maximalQuantity)>
    {
        ("Chocolate",   0, 0, 3),
        ("Tangerines",  0, 0, 3),
        ("Candies",     0, 0, 3),
        ("Waffles",     0, 0, 3),
        ("Biscuits",    0, 0, 3),
        ("Marshmallow", 0, 0, 3),
        ("Meringue",    0, 0, 3),
    };

    var inputResult = viewManager.ShowQuantityChangerPopup(title, hintText, 3, 3, list);
    ShowNotification(inputResult == null
            ? "Nothing"
            : $"Selected : {string.Join(", ", inputResult.Zip(list, (quantity, item) => (name: item.name, quantity: quantity)).Where(i => i.quantity != 0).Select(i => $"{i.quantity} × {i.name}"))}",
        TimeSpan.FromSeconds(10));
}
```