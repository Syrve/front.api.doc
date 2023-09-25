---
title: Information about the current user authorized on Syrve POS
layout: default
---

From version V7Preview4 it will be possible to subscribe to a notification about changing the current user to Syrve POS: [`CurrentUserChanged`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_CurrentUserChanged.htm).

At the time of subscription, the current user is returned. If no one is authorized on Syvre POS, it returns `null`.