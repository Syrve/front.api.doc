---
title: Additional currency support in IPaymentItem
layout: default
tags: v9preview1 v9
---

The [`IPaymentItem`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Payments_IPaymentItem.htm) interface now has a [`CurrencyInfo`](https://syrve.github.io/front.api.sdk/v9/html/P_Resto_Front_Api_Data_Payments_IPaymentItem_CurrencyInfo.htm) property, which allows storing data on an additional payment currency.
To support the use of additional currencies in payments, the ability to set the amount in an additional currency the [`AddPaymentItem`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Editors_IEditSession_AddPaymentItem.htm), [`AddExternalPaymentItem`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Editors_IEditSession_AddExternalPaymentItem.htm), [`AddExternalFiscalizedPaymentItem`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Editors_IEditSession_AddExternalFiscalizedPaymentItem.htm) methods was added, as well as specify this currency via interface [`IAdditionalCurrency`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Data_Payments_IAdditionalCurrency.htm).
In addition, a method for obtaining a list of currencies with their current rates [`GetCurrencyRates`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_GetCurrencyRates.htm) was added, as well as a list of currencies with rates for a specific order [`GetCurrencyRatesForOrder`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_IOperationService_GetCurrencyRatesForOrder.htm)
