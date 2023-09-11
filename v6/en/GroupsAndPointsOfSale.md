---
title: Groups & Points Of Sale
layout: default
---
## Basic Definitions ##

In Syrve, a group may correspond to a structural or logical accounting unit in the store. For example, if a venue has two floors of dining rooms, you can register two groups — one per floor. Groups and sections are defined by the settings. Such settings should be made to allow the system to function properly.

A group must have one or more terminals to register orders and process payments. To ensure that the real-time information is available on all the terminals, such data as orders, reservations, personal and till shifts, and a transaction day must be synced. For this, you need to add the so-called «main cash register» to a group. This cash register is the central element in syncing the data between terminals. In the «Restaurant» mode, the main cash register is required, but it is optional in the «Fast Food» mode. If the main cash register is not specified, the syncing will not take place, and each terminal will have its own copy of data

Point of Sale is a logical unit that stands for one cash register.

A terminal can be linked to one POS, several POS, or none. At the time of payment, orders can be split between POS according to the settings.

## Key API Types & Methods ##
- `ITerminalsGroup` — is a group. Basic settings: name (`Name`), an attribute indicating whether or not cashiers should be asked to perform split printing (`AskCashierForMultiCashRegisterPayment`).
- The `IOperationService.GetHostTerminalsGroup` method allows to get the terminal group where the plugin is running.
- `IPointOfSale` — point of sale; basic settings: name (`Name`), «Main Cash Register» attribute (`IsMain`).
- The `IOperationService.GetHostTerminalPointsOfSale` method allows to get a list of points of sale linked to the terminal where the plugin is running.
- The `PointOfSale` property of the order payment item (`IPaymentItem`) allows to identify the POS where the payment was made (if the payment is posted).
- POS details can be found in the event of transition to the checkout screen `INotificationService.SubscribeOnNavigatingToPaymentScreen`.
- Now the `IOperationService.NeedToSplitOrderBeforePayment` method returns the `CheckSplitResultDto` object with the information on distribution of orders among points of sale as if it happened at the moment on this terminal. The `CheckSplitRequiredResult` property indicates whether the split printing is permissible/required. The `PointOfSale` property indicates the designated POS if it can be clearly determined.

## Examples ##
A split order example can be found in the Resto.Front.Api.SamplePlugin (`EditorTester.SplitOrder`) plugin as part of the SDK.