---
title: Order numbering reset
layout: default
tags: v9preview1 v9
---

In API V9Preview1, the ability to reset order numbers within a group via API was added.

For this purpose, a new method, [`ResetOrderCounter`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_ResetOrderCounter.htm), was added to the API. To use it, the till shifts on every terminals in the group must be closed. This method resets the order numbering for new orders to 1.

This method is analogous to the __Start order numbering from 1 in each till shift__ setting in Syrve Office in the __[`Other Settings`](https://en.syrve.help/articles/#!office-8-2/topic-252/a/h2__1076523567)__ section. However, it allows resetting order numbering only when needed, without changing the settings in Syrve Office.

