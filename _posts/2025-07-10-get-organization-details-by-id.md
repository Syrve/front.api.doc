---
title: Obtaining organization data by identifier
layout: default
tags: v9preview6 v9
---
In API V9Preview6, the ability to obtain organization data by Id for Poland has been added.

- In [`IOrder`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Orders_IOrder.htm), a new property [`EInvoiceOrganizationId`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Orders_IOrder_EInvoiceOrganizationId.htm) has been added — the ID of the organization for which the invoice will be created.
- A new method [`GetOrganizationDetailsById`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_GetOrganizationDetailsById.htm) has been added, which accepts the organization ID and returns organization data [`OrganizationDetailsInfo`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Payments_OrganizationDetailsInfo.htm).
