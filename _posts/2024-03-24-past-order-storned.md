---
title: PastOrder cancellation notification
layout: default
tags: v8
---

A new notification [`PastOrderStorned`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_INotificationService_PastOrderStorned.htm) has been added to Api V8, which is triggered after a successful cancellation of [`PastOrder`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Orders_PastOrder.htm).

The notification transmits information about the canceled closed order in the form of an object of the newly added class [`StornedPastOrderInfo`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Orders_StornedPastOrderInfo.htm).
