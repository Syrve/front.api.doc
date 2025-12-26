---
title: Changing the comment on a printed dish
layout: default
tags: v8
---

Api V8 now allows you to change comments on printed dishes.

To do this, use the following operation: [`ChangeOrderItemComment`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Editors_IEditSession_ChangeOrderItemComment.htm).

To delete a comment on a printed dish, the operation [`DeletePrintedOrderItemComment`] has been added (https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_Editors_IEditSession_DeletePrintedOrderItemComment.htm).

To use these features, you must have the new F_CCCP (CAN_CHANGE_COMMENT_PRINTED) right to change comments for printed dishes.
