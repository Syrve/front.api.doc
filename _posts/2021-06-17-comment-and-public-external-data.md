---
title: Added Comment fields and changed the signature of the AddOrderExternalData method
layout: default
---

In API V7Preview7, a field has been added to the order [`Comment`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Orders_IOrder_Comment.htm), the method is used to change the value [`IEditSession.ChangeOrderComment`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Editors_IEditSession_ChangeOrderOriginName.htm).

Method signature changed [`AddOrderExternalData`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Editors_IEditSession_AddOrderExternalData.htm) — flag added `isPublic`.

Data recorded using the `AddOrderExternalData` method with the `isPublic` flag set to `true` will become available in iikoRMS for uploading to the OLAP sales report. If, when calling the `AddOrderExternalData` method, you pass the `isPublic` flag equal to `false`, then such data, as before, is saved only in Syrve POS and will not be accessible externally.
