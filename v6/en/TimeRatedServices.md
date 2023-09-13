---
title: Pay-Per-Time Services
layout: default
---
## Overview

The Syrve POS app can be used not only to sell goods and items but also for services charged per time. Venues, where customers pay per time they spend there (water parks, bowling, anticafés), can be a good example. In this article, we use pool to describe the subject matter.

### Start & Stop
You start and stop the service. The system tracks time while the service is running; the «quantity» is growing and thus growing the total amount. When the service is stopped, the time meter is stopped as well. Customers only pay for the playtime.

Customers may choose to pay for a specific time in advance. In this case, the service is paid in full even if customers decide to leave earlier. Suppose a guest buys one hour of pool. In this case, the time limit and the price are set and won’t change, unlike the «quantity», which will grow as time passes. When the limit is reached, the service will stop automatically. Alternatively, customers may choose to pay upon completion of services for the overall playtime. In this case, the time limit is not set, and the time will continue to progress until the maximum permissible duration of 12 hours is reached.

The starting and stopping can be accompanied by an additional action linked to the table where the order is placed. In the case of pool, it can be putting the light on over the table during the playtime: by default, the lights are off, when the service is started, the light turns on, when the service is stopped, the light goes off. This creates a restriction — no more than one concurrently running service per table. For now, this restriction applies to all tables regardless of whether such additional actions are set or not. In later versions, this restriction will not apply to the tables or services with no links to external devices.

### Rates
The service price may depend on the day of the week and change with time. There are two rate types:

- By time of day — in the middle of the day the service may be cheaper than in the evening, which helps to balance the low-demand and high-demand periods out.
- By service time — the first hour may be the most expansive while each successive hour is cheaper than the previous, which makes it tempting to purchase more time.

The time is tracked according to the following rules:

- A measurement unit needs to be set. The service time is rounded up to the nearest rated interval, for instance, if the hourly rate charging is set and the time since the service start is 70 minutes, customers should pay for two hours.
- Minimum service duration can be set. In this case, if, for instance, a service is used for 10 minutes only, whereas the minimum duration is set to half an hour, a customer will need to pay for half an hour even if the per-minute charging is enabled. If the service with the same settings is used for 31 minutes, then a customer will need to pay for 31 minutes only.
- As time passes, the system switches from rate to rate automatically and stores the service time used under each rate. Periods, corresponding to different rates, are priced differently.  

## Data Structure ##

### Service
[`IOrderServiceItem`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Orders_IOrderServiceItem.htm) — order item that corresponds to the time-based service. It is located in the [`IOrder.Items`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrder_Items.htm) collection together with food items ([`IOrderCookingItem`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Orders_IOrderCookingItem.htm)).
A pay-per-time service is given as the ([`IProduct`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Assortment_IProduct.htm)) product on the menu with the [`ProductType.Service`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Assortment_ProductType.htm) type and has the [`IProduct.RateSchedule`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Assortment_IProduct_RateSchedule.htm) charging parameters (if charging parameters are not set, the service has no time-based rates and can be added to the order and paid as a regular item).

Key properties of the pay-per-time service:

- [`Service`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrderServiceItem_Service.htm) — the product associated with the service defines the availability on the menu, base price (default rate), and so on.
- [`Periods`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrderServiceItem_Periods.htm) — a list of service intervals representing different rates. When the service is started, the first period will be added to the list. The period time and value will grow until the next rate is enabled, which will add the second period, and so on. Each rate has one period (interval) associated with it: if—according to the rate scale—rates are enabled in turns, the time spent in the first period will increase, then the same will be true for the second period, and then the first period again.
- [`IsStarted`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrderServiceItem_IsStarted.htm) — whether or not the service is running at the moment.
- [`Price`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrderServiceItem_Price.htm) — base unit price (certain periods can have different rates).
- [`TimeLimit`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrderServiceItem_TimeLimit.htm) — a time limit. If set, the service will stop automatically when the limit is reached. Even if you stop the service prematurely, the entire service time needs to be paid anyways, in which case, used time is registered and paid under preconfigured periods, whereas, unused time — under the service itself (see  `RemainingLimitCost`). 
- [`RemainingLimitCost`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrderServiceItem_RemainingLimitCost.htm) — unused time price. If the limit is not set, 0. When the limit is set, the entire time limit price will be due (the service is not yet started, no ordered time is used). With time, the time limit total value will decrease (until dropped to zero), while the period total value will increase (unused limit time will grow smaller, and used time will become larger).
- [`Cost`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrderServiceItem_Cost.htm) — overall service value includes amounts accumulated over all periods and the value of unused time.
- [`ResultSum`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrderServiceItem_ResultSum.htm) — total service value, including discounts, surcharges, and the VAT.

### Service Period
[`IOrderServiceItemPeriod`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Orders_IOrderServiceItemPeriod.htm) — a period a service operates at a particular rate, which includes all time intervals under this rate.

Key properties of the service period:

- [`Service`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrderServiceItemPeriod_Service.htm) — product associated with the period rate. Alternatively, it is the [`ProductType.Service`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Assortment_ProductType.htm) product specified in the service as the default rate or the [`ProductType.Rate`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Assortment_ProductType.htm) product specified in the rate scale.
- [`Price`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrderServiceItemPeriod_Price.htm) — price per time unit.
- [`ElapsedTime`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrderServiceItemPeriod_ElapsedTime.htm) — overall time the service is used under the particular rate. If the service is started and stopped more than once or rates are switched back and forth, the system would combine all time intervals associated with the rate into one. It would be a precise, not rounded period of time. The system registers the actual time, however, a guest would pay for a rounded time. For instance, if the service is charged hourly and the ElapsedTime is 10 minutes only, the period value will correspond to one hour and will not change during the next 50 minutes under this rate.
- [`Cost`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrderServiceItemPeriod_Cost.htm) — пoverall period value. It is already included in the service value (both  [overall](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrderServiceItem_Cost.htm) and [total](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_IOrderServiceItem_ResultSum.htm)), therefore, there is no need to sum them up. This property is only intended to break the service cost down by rates.

### Rate Settings
[`RateSchedule`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Orders_RateSchedule.htm) — charging mode: by service time or by the time of day.

- [`TimingMode`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_RateSchedule_TimingMode.htm) — charging mode: by service time or by the time of day.
- [`Items`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_RateSchedule_Items.htm) — rate scale, list of [`RateScheduleItem`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Orders_RateScheduleItem.htm) items that define time intervals with rates other than the base rate. The time interval ([`RateScheduleInterval`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Data_Orders_RateScheduleInterval.htm)) is defined by the day of the week and the shift of the interval beginning and the end against the rate scale beginning. Depending on the selected charging mode (`TimingMode`) н, the scale begins together with the day (entire scale is *[0;24)*  hours) or together with the service (entire scale is *[0;12]* hours). Such time intervals are sure will not overlap. If the list is empty, the base rate is enabled all the time.
- [`TimingStep`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_RateSchedule_TimingStep.htm) — measurement unit. The real service time is rounded up to the nearest rate step.
- [`MinimumDuration`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_Data_Orders_RateSchedule_MinimumDuration.htm) — minimum service duration or null if the lower limit is not set.

## Service Management
[`AddOrderServiceItem`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_Editors_IEditSession_AddOrderServiceItem.htm) — adding the service to the order.

[`StartService`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_StartService.htm) — starting the service.

[`StopService`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_IOperationService_StopService.htm) — stopping the service. If a guest bill is printed or the order is paid, the service will stop automatically.

## Examples
The SamplePlugin included in the SDK shows examples of the service adding, starting, and stopping — [`StartService`] and [`StopService`] methods in the [`EditorTester`](https://github.com/syrve/front.api.sdk/blob/main/sample/v6/Resto.Front.Api.SamplePlugin/EditorTester.cs) class:

```cs
private static void StartService()
{
    var order = PluginContext.Operations.GetOrders().Last(x => x.Status == OrderStatus.New);
    var serviceProduct = PluginContext.Operations.GetActiveProducts().Last(x => x.Type == ProductType.Service && x.RateSchedule != null);
    var credentials = PluginContext.Operations.GetCredentials();
    var service = PluginContext.Operations.AddOrderServiceItem(serviceProduct, order, order.Guests.Last(), credentials, TimeSpan.FromHours(2));
    PluginContext.Operations.StartService(credentials, order, service);
}

private static void StopService()
{
    var order = PluginContext.Operations.GetOrders().Last(x => x.Status == OrderStatus.New);
    var service = order.Items.OfType<IOrderServiceItem>().Last(x => x.IsStarted);
    PluginContext.Operations.StopService(PluginContext.Operations.GetCredentials(), order, service);
}
```