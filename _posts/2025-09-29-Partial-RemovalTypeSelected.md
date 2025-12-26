---
title: Write-off method for full, partial return or order deletion
layout: default
tags: v9preview7 v9
---
In API V9Preview7, the ability to get the write-off method for full, partial return or order deletion has appeared.

Two new notifications were added:
- [`RemovalTypeSelected`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_INotificationService_RemovalTypeSelected.htm) is used to notify the plugin of the reason for writing off the order on which a full return or deletion of a voided order is being performed. If one of the subscribed plugins throws an `OperationCanceledException`, the further operation will be cancelled.
- [`PartialOrderItemsRemovalTypeSelected`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_INotificationService_PartialOrderItemsRemovalTypeSelected.htm) is used to notify the plugin of the reason for write-off when a partial return of items in the order is performed. If one of the subscribed plugins throws an `OperationCanceledException`, the further operation will be cancelled.