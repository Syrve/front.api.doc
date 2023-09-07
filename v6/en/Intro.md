---
title: Intro
layout: default
---
A plugin can be programmed using any [.Net family](http://en.wikipedia.org/wiki/List_of_CLI_languages) programming language, resulting in a .Net 4.7.2 build release, which will be loaded by Syrve POS at the startup. Syrve POS creates a separate process container to load each plugin. This allows to avoid conflicts of dependencies and provides the system reliability (a failure in one of the plugins will not affect the rest of the system). Each plugin should be installed in a separate subfolder of the `Plugins`, folder located in `C:\Program Files\<Syrve POS folder>`. The plugin build must include the class that has a default public builder (no parameters) and implements the `IFrontPlugin` interface. Plugin and Syrve POS processes interact via the IPC channel; objects are transferred by the link (such objects should be inherited from [`MarshalByRefObject`](http://msdn.microsoft.com/en-us/library/system.marshalbyrefobject(v=vs.100).aspx)) or by the value (data classes; they should be serializable).

The entry point is the plugin class. When the plugin is started, a copy of this class is created; its builder may have required data loaded, connections established, events of interest subscribed, and so on. Before stopping the operation and exporting the plugin, the `Dispose` method is called. It is used to unsubscribe from events, close connections, and free up other resources.

Interaction with the API starts with the static class `PluginContext`, which accumulates all functions and services:

- `PluginContext.Operations` — [operations service](#operationService),
- `PluginContext.Notifications` — [notifications service](#notificationService),
- `PluginContext.Licensing` — [licensing](Licensing),
- `PluginContext.Services` — all available services, including all listed above, as well as services of other API versions,
- `PluginContext.Log` — logging,
- `PluginContext.Shutdown()` — plugin shutdown.

## Main Contracts ##

### Data Structure ###
The data is accumulated in the `Resto.Front.Api.Data` namespace and is grouped in the following way:

- `Organization` — company structure and settings,
- `Assortment` — menu items,
- `Orders` — orders, including guests, items, modifiers, discounts, etc.,
- `Kitchen` — kitchen orders,
- `Brd` — abbreviation for Banquets, Reservations, and Deliveries,
- `Security` — plugin authentication data,
- `Licensing` — license control,
- ...

Data objects are mainly read-only objects: they have the get-only features interface published, therefore, the plugin cannot create new copies of such objects or change field values of existing ones. Most of the objects, for instance, `IProduct`, `ITable`, or `IUser`, cannot be edited at all as they are databases created in Syrve Office. Some object types can be [edited](Data%20editing) indirectly by applying certain actions. At last, there is a special category of objects in the `Resto.Front.Api.Data.DataTransferObjects` namespace that may be created and edited on the plugin side.

### Operations service {#operationService}
Practically all actions are performed through `IOperationService`. Methods comprising this interface can be split into the following groups:

- reading data — obtaining all objects of a certain type (`GetProductGroups`), obtaining specific object (`GetProductGroupById`), obtaining linked objects (`GetChildGroupsByProductGroup`),
- editing data — single actions (`PrintBillCheque`), package actions (using `CreateEditSession`),
- other operations — displaying messages (`AddWarningMessage`), checking permissions (`CheckCanEditOrder`), and so on.
 
When `IOperationService` is in use, the plugin prevails.

### Notifications service {#notificationService}
`INotificationService` makes it possible to subscribe to events of interest. Data change notifications are implemented using the [Reactive Extensions (Rx)](http://msdn.microsoft.com/en-us/data/gg577609.aspx) library, events are published as `IObservable<T>` sequences, where `T` is the event argument (changed object). Rx handles the flow of events as handy as LINQ handles `IEnumerable<T>`. To familiarize yourself with the reactive programming, check this useful resource: [Introduction to Rx](http://www.introtorx.com/). Here you can also subscribe to such operations as opening or closing a till shift, printing a service or cash register receipt, and so on. Along with notifications on running operations, there might be other additional features available, for instance, when closing a till shift, an additional report can be printed out, when printing a receipt, some marketing information can be added to it, when switching to the cash register screen, a gift item or loyalty account payment can be added to the order, whereas some other operations can be canceled at all by generating an `OperationCanceledException` exception.

When handling `INotificationService`, Syrve POS is the action initiator, whereas the plugin is the observer.

## Examples ##

Among other things, the SDK includes several source code [plugin examples](https://github.com/syrve/front.api.sdk/tree/master/sample/v6/). We recommend that you get familiar with them before you start developing your own plugins.

### SamplePlugin ###

This example shows various API use cases:

- Adding promotional information to receipts,
- Handling orders (creating orders, adding guests, adding and removing items and modifiers, changing the number of portions, printing kitchen tickets and guest checks, checkout, and so on),
- Handling deliveries,
- Handling banquets and reservations,
- Handling customer databases, street databases,
- Displaying modeless notifications on the terminal screen,
- Viewing the restaurant structure (rooms, tables, employees),
- Viewing the menu hierarchy,
- Viewing kitchen statistics,
- Managing licensed connections,
- Registering additional operations in Syrve POS menu,
- …

### CustomerScreenPlugin ###
This example represents a ready-to-use customer screen plugin for terminals with two displays, where the cashier display shows the Syrve POS interface, and the customer display shows the company’s logo and promotional videos, as well as the order items.

### Resto.Front.Api.SampleScalePlugin
This is a scale integration plugin example. It shows how you can use a plugin to hook up and use the scales that are not supported out-of-the-box.

### Resto.Front.Api.SampleCashRegisterPlugin
It shows how to handle a custom fiscal register

## Contact ##

- [Syrve Help](https://en.syrve.help/)
- [Syrve POS API](https://en.syrve.help/articles/api/pos-api)