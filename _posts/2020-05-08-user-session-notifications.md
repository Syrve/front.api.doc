---
title: Added notification about opening and closing personal shifts of users
layout: default
---

Since V7Preview2 a notification has been added [`UserSessionChanged`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_UserSessionChanged.htm), that makes it convenient to track property changes [`IUser.IsSessionOpen`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Security_IUser_IsSessionOpen.htm).

In some scenarios, personal shifts may open or close for a group of users at once, so the notification will contain a list of users whose personal shift has opened or closed, and the list may simultaneously include those whose personal shift has just opened and those who who has just closed.

In previous versions of the API, you had to use a more general notification to track personal shifts [`UserChanged`](https://syrve.github.io/front.api.sdk/v6/html/P_Resto_Front_Api_INotificationService_UserChanged.htm), it, as before, is triggered by any user change, including opening and closing a personal shift.

By the way, the plugin can not only monitor personal changes of users, but also [manage]({{ site.baseurl }}/2019/12/27/open-close-personal-sessions.html) them.