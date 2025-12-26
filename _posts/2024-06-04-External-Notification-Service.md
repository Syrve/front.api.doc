---
title: Inter-plugin Events
layout: default
tags: v9preview2 v9

---

In API V9Preview2, the ability to create and use custom events was added.

For this purpose, the following operations were introduced:

- [`GetExternalNotificationSubscription`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_GetExternalNotificationSubscription.htm)
- [`InvokeExternalNotification`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_InvokeExternalNotification.htm)
- [`GetAllExternalNotificationsNames `](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_GetAllExternalNotificationsNames.htm)


[`GetExternalNotificationSubscription(string subscriptionName)`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_GetExternalNotificationSubscription.htm) - this operation allows you to subscribe to a custom event named ***subscriptionName***. If the event does not exist, it will be registered and subscribed to immediately. The maximum length of the event name is **256** characters.

Usage example:

	IDisposable subscription;
	subscription =	
		PluginContext.Operations
			.GetExternalNotificationSubscription(subscriptionName)
			.Subscribe(Console.WriteLine);

[`GetAllExternalNotificationsNames() `](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_GetAllExternalNotificationsNames.htm) - this operation allows you to retrieve a list of the names of all events registered on the terminal.

[`InvokeExternalNotification(string subscriptionName, string notificationData)`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_InvokeExternalNotification.htm) - this operation triggers a custom event notification named ***subscriptionName*** for all its subscribers and transmits information in text format as ***notificationData***. The maximum length of the information is **32,000** characters.

Usage example:

	var subscriptionName = PluginContext.Operations.GetAllExternalNotificationsNames().Last();
    PluginContext.Operations.InvokeExternalNotification(subscriptionName, Guid.NewGuid().ToString());

It is important to know that events are shared across all terminals in the group, which allows notifications to be sent not only to local plugins but also to plugins on other terminals. Additionally, an event notification can be triggered from any plugin within the terminal group, even if the plugin itself is not subscribed to that event.

A [`usage example`](https://github.com/syrve/front.api.sdk/blob/master/sample/v9preview2/Resto.Front.Api.SamplePlugin/ExternalNotificationsTester.cs) of custom events has been added to the [`SamplePlugin`](https://github.com/syrve/front.api.sdk/tree/master/sample/v9preview2/Resto.Front.Api.SamplePlugin).