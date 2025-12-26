---
title: Changing the text on the progress bar
layout: default
tags: v9 v9preview2

---

The V9Preview2 API now allows you to change the text on the progress bar 
- When performing [external fiscal register operations](https://syrve.github.io/front.api.doc/v6/ru/CashRegisters.html):
  - [`DoOpenSession`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Devices_ICashRegister_DoOpenSession.htm): triggered when opening a cash register shift;
  - [`DoXReport`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Devices_ICashRegister_DoXReport.htm): triggered when opening a cash register shift and when printing an X-report;
  - [`DoZReport`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Devices_ICashRegister_DoZReport.htm): triggered when closing a cash shift;
  - [`DoCheque`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Devices_ICashRegister_DoCheque.htm): triggered when printing a check, including when closing a cash shift, if there are open orders that need to be closed. During payment to the courier and when changing the payment type for an order. Also during any prepayments and cancellations;
- [`DoPayIn`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Devices_ICashRegister_DoPayIn.htm) and [`DoPayOut`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Devices_ICashRegister_DoPayOut.htm): withdrawal and deposit of funds;
  - [`OpenDrawer`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Devices_ICashRegister_OpenDrawer.htm): opening the cash drawer;
  - [`DoBillCheque`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Devices_ICashRegister_DoBillCheque.htm): printing a receipt if it is printed by a fiscal register.
- In [extensions for fiscal registers](https://syrve.github.io/front.api.doc/v6/ru/ChequeTaskProcessor.html): all methods [`IChequeTaskProcessor`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_Devices_IChequeTaskProcessor.htm).


Now, [`IViewManager`](https://syrve.github.io/front.api.sdk/v9/html/T_Resto_Front_Api_UI_IViewManager.htm) is passed to all these methods with the option to call the method [`ChangeProgressBarMessage`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_UI_IViewManager_ChangeProgressBarMessage.htm).