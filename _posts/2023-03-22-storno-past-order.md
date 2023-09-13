---
title: Products and orders returns from closed cash register shifts
layout: default
---

Now it is possible to return products or orders from closed cash register shifts not only from [UI SyrvePOS](https://en.syrve.help/articles/#!syrve-pos-8-5/product-return), but also from the V8 API.

In one of the [previous news]({{ site.baseurl }}/2023/03/09/past-orders.html)
we talked about ways to receive orders from closed cash register shifts.
Now, if necessary, you can adjust the list of dishes of such an order and return it by calling the method
[`StornoPastOrder`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_StornoPastOrder.htm).

If you need to return goods that are not tied to a specific order, you can use the method
[`StornoPastOrderItems`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_StornoPastOrderItems.htm),
having previously generated a list of goods `items` to be returned.
[`PastOrderItem.Price`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_PastOrderItem_Price.htm) and
[`PastOrderItem.SumWithoutDiscounts`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_PastOrderItem_SumWithoutDiscounts.htm)
are not used; they can be left blank when passed to a method.

You can also specify from the API [dish size](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_PastOrderItem_ProductSize.htm), if sizes are supported by it.
Each of the passed `items` can be marked as a modifier or main dish
([`PastOrderItem.IsMainDish`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_PastOrderItem_IsMainDish.htm).
Similar to the UI and without strict checks for compliance with the type of item from the card.
But the modifier cannot be the first element of the list.
Also, a dish marked with a modifier cannot have a size.
Each [`PastOrderItem`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Orders_PastOrderItem.htm)
must indicate the place of preparation and the type of place of preparation.

At the moment, a refund is only possible on the default fiscal registrar of the current terminal.
The cash register shift must be open and there must be enough cash for cash returns.

The payment type for reversal can be obtained via
[`GetPaymentTypesToStornoPastOrderItems`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetPaymentTypesToStornoPastOrderItems.htm).
You can also return [plugin payment type]({{ site.baseurl }}/v6/ru/PaymentProcessor.html)
(with checkbox `canProcessPaymentReturnWithoutOrder` equal `true`, transferred to
[`RegisterPaymentSystem`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_RegisterPaymentSystem.htm).

The deletion type for reversal can be obtained via
[`GetRemovalTypesToStornoPastOrderItems`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetRemovalTypesToStornoPastOrderItems.htm).
The type of deletion can be

- without write-off
([`IRemovalType.WriteoffType`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IRemovalType_WriteoffType.htm)` == WriteoffType.None`)
- with write-off
([`IRemovalType.WriteoffType`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Orders_IRemovalType_WriteoffType.htm)`.HasFlag(WriteoffType.Cafe`))

When reversing with a write-off, the presence of the custom payment type "Return from customer" is checked.

The branch must belong to the current group and the branch must have a warehouse.

The tax system is transferred only if it was received through
[`GetTaxationSystemsToStornoPastOrderItems`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetTaxationSystemsToStornoPastOrderItems.htm).


When reversing with a write-off, the right F_STRN (Return payment) is checked..
When reversing without writing off, the right F_SWWOFF (Return payment on a check without returning to the warehouse) is checked..

Examples:
```
private void StornoPastOrder()
{
    var pastOrder = PluginContext.Operations.GetPastOrders().First();
    var paymentType = pastOrder.Payments.First();

    // rt.WriteoffType.HasFlag(WriteoffType.Cafe) - reversal with write-off
    // rt.WriteoffType == WriteoffType.None - reversal without write-off
    var removalType = PluginContext.Operations.GetRemovalTypesToStornoPastOrderItems().First(rt => rt.WriteoffType.HasFlag(WriteoffType.Cafe));
    var section = pastOrder.RestaurantSection;
    var orderType = pastOrder.OrderType;
    var taxationSystem = PluginContext.Operations.GetTaxationSystemsToStornoPastOrderItems().Select(ts => (TaxationSystem?)ts).FirstOrDefault();

    PluginContext.Operations.StornoPastOrder(
        PluginContext.Operations.GetCredentials(),
        pastOrder,
        paymentType,
        removalType,
        section,
        orderType,
        null,
        taxationSystem,
        null);
}

private void StornoProducts()
{
    var paymentType = PluginContext.Operations.GetPaymentTypesToStornoPastOrderItems().First(pt => pt.Name.Contains("SampleApiPayment"));

    // rt.WriteoffType.HasFlag(WriteoffType.Cafe) - reversal with write-off
    // rt.WriteoffType == WriteoffType.None - reversal without write-off
    var removalType = PluginContext.Operations.GetRemovalTypesToStornoPastOrderItems().First(rt => rt.WriteoffType.HasFlag(WriteoffType.Cafe));
    var section = PluginContext.Operations.GetTerminalsGroupRestaurantSections(PluginContext.Operations.GetHostTerminalsGroup()).First();
    var orderType = PluginContext.Operations.GetOrderTypes().FirstOrDefault(ot => ot.OrderServiceType == OrderServiceTypes.Common);
    var taxationSystem = PluginContext.Operations.GetTaxationSystemsToStornoPastOrderItems().Select(ts => (TaxationSystem?)ts).FirstOrDefault();

    var activeProducts = PluginContext.Operations.GetActiveProducts()
        .Where(product => product.Template == null &&
            (product.Type == ProductType.Dish || product.Type == ProductType.Modifier || product.Type == ProductType.Goods || product.Type == ProductType.ForPurchase))
        .ToList();

    PluginContext.Operations.StornoPastOrderItems(
        PluginContext.Operations.GetCredentials(),
        GetPastOrderItems(activeProducts).ToList(),
        paymentType,
        removalType,
        section,
        orderType,
        null,
        taxationSystem,
        null);
}

private IEnumerable<PastOrderItem> GetPastOrderItems(IReadOnlyList<IProduct> activeProducts)
{
    var product = activeProducts.First(p => p.Name.Contains("Чай"));
    yield return new PastOrderItem
    {
        IsMainDish = true,
        Product = product,
        ProductSize = PluginContext.Operations.GetProductSizes().FirstOrDefault(ps => ps.Name.Contains("S")),
        Amount = 2,
        SumWithDiscounts = 100.5m,
        Price = 0, // not used in StornoPastOrderItems
        SumWithoutDiscounts = 0, // not used in StornoPastOrderItems
    };

    product = activeProducts.First(p => p.Name.Contains("Сливки"));
    yield return new PastOrderItem
    {
        IsMainDish = false,
        Product = product,
        ProductSize = null,
        Amount = 2,
        SumWithDiscounts = 0m,
        Price = 0, // not used in StornoPastOrderItems
        SumWithoutDiscounts = 0, // not used in StornoPastOrderItems
    };

    product = activeProducts.First(p => p.Name.Contains("Пюре"));
    yield return new PastOrderItem
    {
        IsMainDish = true,
        Product = product,
        ProductSize = null,
        Amount = 1,
        SumWithDiscounts = 90m,
        Price = 0, // not used in StornoPastOrderItems
        SumWithoutDiscounts = 0, // not used in StornoPastOrderItems
    };

    product = activeProducts.First(p => p.Name.Contains("Бутылка"));
    yield return new PastOrderItem
    {
        IsMainDish = true,
        Product = product,
        ProductSize = null,
        Amount = 2,
        SumWithDiscounts = -15m,
        Price = 0, // not used in StornoPastOrderItems
        SumWithoutDiscounts = 0, // not used in StornoPastOrderItems
    };
}
```