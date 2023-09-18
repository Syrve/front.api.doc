---
title: Notification of deletion of unprinted dishes and current user
layout: default
---

In API V7 we added a notification about deleting unprinted dishes
[`BeforeDeleteNonPrintedItems`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_BeforeDeleteNonPrintedItems.htm),
and also forwarded the current user responsible for the operation being performed to some events.

Removal of unprinted dishes can be interrupted by throwing `OperationCanceledException` in the handler.
A notification is generated both when you try to delete a dish from the iikoFront UI, and when you try to delete it from the API.

Notifications and methods that began to accept the input of the current
[user](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Security_IUser.htm),
performing a particular operation:

- [`BeforeOrderBill`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_BeforeOrderBill.htm)
— bill of lading, printing a duplicate bill of the order;
- [`OrderBillCancelled`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_OrderBillCancelled.htm)
— cancellation of pre-check order;
- [`BeforeDeleteOrder`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_BeforeDeleteOrder.htm)
— deleting an order;
- [`OrderSplittedByCashRegisters`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_OrderSplittedByCashRegisters.htm)
— division of an order into several cash ragisters;
- [`OrderStorned`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_OrderStorned.htm)
— order cancellation;
- [`BeforeDeletePrintedItems`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_BeforeDeletePrintedItems.htm)
— deleting printed dishes in an order;
- [`BeforeDeleteNonPrintedItems`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_BeforeDeleteNonPrintedItems.htm)
— deleting unprinted dishes in an order;
- [`ICashRegister.DoZReport`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Devices_ICashRegister_DoZReport.htm)
— printing a Z-report during the closing of a cash register shift on a plug-in cash register;
- [`ICashRegister.DoXReport`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Devices_ICashRegister_DoXReport.htm)
— printing X-report on a plug-in cash register;
- [`ICashRegister.DoOpenSession`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Devices_ICashRegister_DoOpenSession.htm)
— opening a cash register shift on a plug-in cash register;
- [`ICashRegister.OpenDrawer`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Devices_ICashRegister_OpenDrawer.htm)
- opening the cash drawer on a plug-in cash register.