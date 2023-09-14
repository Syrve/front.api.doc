---
title: Discounts & Surcharges
layout: default
order: 15
---
Syrve Office has a built-in discount system. External loyalty systems (Plazius, Syrve Loyalty, and others) are not covered in this article. Syrve Office discount system allows applying discounts and surcharges under various conditions. A discount means an item price decrease for a particular amount, and a surcharge — increase. To simplify the terms we use hereunder and since the difference between a discount and a surcharge is merely the sign of the amount in question, let us call both the discounts (in the code [`DiscountType`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Orders_IDiscountType.htm), [`DiscountItem`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Orders_IDiscountItem.htm), [`DiscountCard`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Orders_IDiscountCard.htm) etc.), implying that the positive [`DiscountSum`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IAppliedDiscountItem_DiscountSum.htm) value corresponds to the discount and negative — to the surcharge if used within the subject matter.

Generally, a discount can be so configured that within an order it may concurrently decrease the price of one item, acting as a discount, and increase the price of another, acting like a surcharge, therefore, the separation of discounts and surcharges seems quite symbolic.

## Restrictions ##
Rules and conditions for the discount ([`DiscountItem`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Orders_IDiscountItem.htm)) application are defined by the type of discount ([`DiscountType`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Orders_IDiscountType.htm)). It can be a fixed amount (coupon), percentage of the price, rounding off (cents dropping), and so on, but the amount after discount ([`AppliedDiscountItem`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Orders_IAppliedDiscountItem.htm)) is always an absolute value, a specific number. Discounts application algorithms depend on the [item category](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Assortment_IProduct_Category.htm), [order section](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Organization_Sections_IRestaurantSection.htm), order amount ([`FullSum`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrder_FullSum.htm)), [service mode](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Organization_OrderServiceType.htm), current time, and [service printing time](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrderRootItem_PrintTime.htm); discounts can be combined or not, can be applied both in parallel (to the full price), and in series (with account to previous discounts), — all such conditions are implementation specifics and are not published in the API.

General settings are given in the API, such as:

- [`IsActive`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IDiscountType_IsActive.htm) — whether or not a discount is active in the [current group](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_GetHostTerminalsGroup.htm),
- [`IsAutomatic`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IDiscountType_IsAutomatic.htm) — can a discount be added to orders automatically or not,
- [`CanApplyManually`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IDiscountType_CanApplyManually.htm) — can a discount be added to orders manually or not by selecting it from the list,
- [`CanApplyByCardNumber`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IDiscountType_CanApplyByCardNumber.htm) — can a discount be added to orders using a discount card number (by entering the card No. on the on-screen keyboard) or not,
- [`CanApplyByDiscountCard`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IDiscountType_CanApplyByDiscountCard.htm) — can a discount be added to orders using a discount card (by reading a card track),
- [`DiscountByFlexibleSum`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IDiscountType_DiscountByFlexibleSum.htm) — whether or not a discount amount should be specified when adding the discount,
- [`CanApplySelectively`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IDiscountType_CanApplySelectively.htm) — can a discount be applied selectively to several order items.

The discount amount is calculated for each item separately even if the discount applies to the entire order. Therefore, an order discount is all item discounts put together. The discount amount as any other monetary amount is divisible by the minimum currency face value. The total of all applicable discounts cannot exceed the price, in other words, the item price after all discounts can be zero (100% discount) but cannot be a negative amount. Surcharges do not have such a restriction — any amount can be added to the order price.

## Life Cycle ##
The discount added to the order may have one of the two states: fixed or not fixed. Discounts may change from one state to another, but they can’t be managed as the state is linked to the order life cycle. Presently, discounts are fixed when an order takes the [status](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrder_Status.htm) `Bill` and unfixed when the order takes the `New`status, however, this procedure is subject to change.

### Unfixed Discount ###
When the discount is added to the order, the system stores only calculating conditions. Amounts are calculated upon the request, whereas calculation results are not stored. Since each time the calculation takes place most recent available parameters of the discount application algorithm are used, including Syrve Office settings and current time, the calculation may have a different result. Therefore, the total order value may change.

Automatic discounts ([`IsAutomatic`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IDiscountType_IsAutomatic.htm)) are not visible in orders with unfixed discounts and are not given on the list of [`Discounts`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrder_Discounts.htm) of each order, however, once discounts are fixed, the visibility depends on the application effect.

### Fixed Discount ###
When the order total amount can no longer change, automatically calculated values (prices, discounts, etc) will be fixed. The last calculation result is saved and used for further reference instead of recalculating it. This makes it possible to work regardless of the discount settings and other application parameters used. From this point on, along with the algorithm and discount application parameters, the system remembers amounts per order item. However, if at the time of fixation, the discount is not in effect (has zero effect), it is removed from the order.

## Data Structure ##
- [`DiscountType`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Orders_IDiscountType.htm) —  discount type, an item in the discount database. The database can be acquired using the [`GetDiscountTypes`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_GetDiscountTypes.htm) method.
- [`DiscountItem`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Orders_IDiscountItem.htm) — added discount. Use the [`AddDiscount`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_Editors_IEditSession_AddDiscount.htm), [`AddFlexibleSumDiscount`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_Editors_IEditSession_AddFlexibleSumDiscount.htm), [`AddDiscountByCardNumber`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_Editors_IEditSession_AddDiscountByCardNumber.htm), [`AddFlexibleSumDiscountByCardNumber`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_Editors_IEditSession_AddFlexibleSumDiscountByCardNumber.htm) methods to add a discount to the order.
Use [`DeleteDiscount`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_Editors_IEditSession_DeleteDiscount.htm) to remove a discount, [`Discounts`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrder_Discounts.htm) to remove a list of added discounts.
- [`AppliedDiscountItem`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Orders_IAppliedDiscountItem.htm) — a result of earlier added discounts; use the [`GetOrderAppliedDiscounts`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_GetOrderAppliedDiscounts.htm)method to get it; whereby, discounts can be applied automatically at the time the request is made, or the system returns the result of earlier applied discounts if it was fixed in the order.
- [`FullSum`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrder_FullSum.htm) — total order amount before discounts (subtotal).
- [`ResultSum`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrder_ResultSum.htm) — total order amount after discounts.

Note. We do not recommend defining the total discount amount as the difference between the last two parameters for they differ not only in discounts but also in the VAT, not included in the item prices. The total discount amount is better be obtained by adding [`DiscountSum`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IAppliedDiscountItem_DiscountSum.htm) values.


## Selective Use of Discounts ##
By default, discounts apply to all items, including those added after the discount. However, discounts with the [`CanApplySelectively`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IDiscountType_CanApplySelectively.htm) setting enabled can be selectively applied to one or more items and hence affect only them. For example, if a venue sells bakery products, hot buns can be sold at a full price, whereas, cooled ones — at a discount. The [Stocklist item](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Assortment_IProduct.htm) is the same in both cases. It is up to a user (or a plugin) to decide which items and modifiers a discount should be applied to. This decision (or limitation) works as a *whitelist*, therefore, such a selective discount will not apply to the items added thereafter. All other limitations (category, time, amount, etc.) remain in force, meaning the discount will only apply to those whitelist items that comply with other conditions as well.

If used selectively, discounts apply only to the specified order items. For example, if there is an item with priced modifiers in the order, and a discount is applied to the item, it is only the item that will be sold at a discount. To apply a discount to both the item and modifiers, they should be expressly specified.

### Key functions ###

- [`ChangeSelectiveDiscount`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_Editors_IEditSession_ChangeSelectiveDiscount.htm) — allows creating a *whitelist* of order items that can be discounted. Taking into account various types of items existing in the system, there are in fact three lists:  [simple items](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Orders_IOrderProductItem.htm), [components of composite items](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Orders_IOrderCompoundItemComponent.htm) and [modifiers](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Orders_IOrderModifierItem.htm).
If `null` is set for all three lists, the discount will not be applied selectively, but rather to the entire order.
- [`GetSelectiveDiscountItemSettings`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_GetSelectiveDiscountItemSettings.htm) — allows returning selective application parameters (above-mentioned lists) or `null` if the discount applies to the entire order without particular items specified.
- [`IsSelectivelyApplied`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IDiscountItem_IsSelectivelyApplied.htm) — shows whether the discount applies selectively or to the entire order.

### Examples ###
An example of a selectively applied discount can be found in the Resto.Front.Api.SamplePlugin (`EditorTester.AddSelectiveDiscount`) plugin as part of the SDK:

```cs
/// <summary>
/// Adding a selective discount.
/// </summary>
private void AddSelectiveDiscount()
{
    var order = PluginContext.Operations.GetOrders().Last();
    var selectedDish = new List<IOrderProductItemStub> { order.Items.OfType<IOrderProductItem>().Last() };
    var discountType = PluginContext.Operations.GetDiscountTypes().Last(x => !x.Deleted && x.IsActive && x.CanApplySelectively);
    var editSession = PluginContext.Operations.CreateEditSession();
    editSession.AddDiscount(discountType, order);
    editSession.ChangeSelectiveDiscount(order, discountType, selectedDish, null, null);
    PluginContext.Operations.SubmitChanges(PluginContext.Operations.GetCredentials(), editSession);
}
```