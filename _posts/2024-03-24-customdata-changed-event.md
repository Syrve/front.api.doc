---
title: Event about changing custom plugin data
layout: default
tags: v8
---

An event for changes to custom plugin data [`CustomDataChanged`] has been added to the V8 API (https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_INotificationService_CustomDataChanged.htm). 

The notification parameter [`CustomDataChangedEventArgs`] (https://syrve.github.io/front.api.sdk/v8/html/Properties_T_Resto_Front_Api_Data_Common_CustomDataChangedEventArgs.htm) consists of several fields:

- `Key` — key of the arbitrary data element;
- `Value` — value of the arbitrary data element;
- `EventType` — event type. Can have the values created, updated, or deleted.

When subscribing to an event, plugins receive notifications only about arbitrary data that has been written/updated/deleted by them or on other terminals by plugins with the same `moduleId`. If two different plugins with the same `moduleId` work with arbitrary data, notifications will be sent to both.