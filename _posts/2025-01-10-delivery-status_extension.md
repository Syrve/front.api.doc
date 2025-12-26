---
title: Extension of delivery statuses DeliveryStatus
layout: default
tags: v9preview3 v9
---

Starting from V9Preview3, the delivery status table [DeliveryStatus](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Brd_DeliveryStatus.htm) has been updated.

The `Unconfirmed` status was excluded (replaced by the `New` status), and `New` itself branched into `Confirmed`, `CookingStarted`, `CookingCompleted`, and `Packed`.

**Current delivery status table**

| Status | Description |
| ------ | ----------- |
| New | _New (waiting for confirmation)_ |
| Confirmed | _Confirmed (waiting for cooking to start)_ |
| CookingStarted | _Cooking_ |
| CookingCompleted | _Cooked (being assembled)_ |
| Packed | _Assembled (waiting for courier or pickup)_ |
| Waiting | _Courier assigned (waiting for dispatch)_ |
| OnWay | _On the way (travelling to the client)_ |
| Delivered | _Delivered (courier is returning)_ |
| Closed | _Closed_ |
| Cancelled | _Cancelled_ |

Don't forget about the [order assembly process](https://en.syrve.help/articles/syrve-8-9/souschef1/a/h2_204041478). With the assembly process disabled, a delivery moving to the `CookingCompleted` status will automatically reach the status...