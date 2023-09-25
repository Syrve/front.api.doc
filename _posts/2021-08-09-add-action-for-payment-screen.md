---
title: Added the ability to expand the functionality of the cash register screen   
layout: default
---
Method added to API V7 [`AddButtonToPaymentScreen`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_AddButtonToPaymentScreen.htm), which allows you to add your transaction to the cash register screen (see article «[Cashier screen]({{ site.baseurl }}/v7/en/ActionOnPaymentScreenView.html)»).



Also added method [`UpdatePaymentScreenButtonState`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_UpdatePaymentScreenButtonState.htm), which allows you to update the state of the button on the checkout screen, as well as the event [`PaymentScreenUpdated`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_INotificationService_PaymentScreenUpdated.htm) to track changes on the cash register screen.

