---
title: Item Size
layout: default
---
In earlier versions (before 5.0), it was assumed that items are cooked the same way all the time and that all the ingredients and sizes are covered by the recipe. To sell items that may have different sizes, like pizza, pies, beverages, and so on, users had to register each item of each size as a separate item, and so create modifiers for each size and duplicate other item and modifier settings. When editing orders in the POS, users had to remove one item and then add another one in order to change the size. This resulted in the menu having a large number of buttons and reports with many rows. We had to introduce a standalone concept — *item size*.  

The item size is the additional property that defines the ingredients and their quantity, ready-made item output weight, and, therefore, the price. A set of interchangeably used sizes makes the size scale. If an item has a size scale assigned, users need to select one of the sizes from the scale when adding the item. Some items do not have all the sizes available. A modifier size corresponds to the item size, though some modifiers may be unavailable for the selected size.

## Key Types ##
- `IProduct` — stocklist item has the `Scale` property with a link to the size scale. A `CompoundItemTemplate` buildup item also has a similar link to the size scale. If an item has not size scale assigned, then `null`. 
- `IProductScale` — size scale with the scale `Name` and the `DefaultSize`. A list of the scale sizes can be obtained using the `GetProductScaleSizes` service method.
- `IProductSize` —  item size has two name options: full name — `Name` and short name — `KitchenName`.
- Methods `AddOrderProductItem` and `AddOrderCompoundItem` have the size argument that allows specifying the item size.
- `OrderCookingItem` — order items (`OrderProductItem` and `OrderCompoundItem`) have the `Size` property with a link to the selected size.
- The selected size can be changed using the `ChangeOrderCookingItemSize` edit method.
- One and the same scale can be used for many items, however, not all the items are available in all sizes. To learn which sizes are not available for this or that item, use the `GetDisabledSizes` service method.
- The `GetPrice` method now allows getting the item price with account to the size (along with the price category and date).


## Examples ##
A size handling example can be found in the Resto.Front.Api.SamplePlugin (`EditorTester.AddCompoundItem`) plugin as part of the SDK.