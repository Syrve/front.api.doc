---
title: Adding new GetCashRegisterStatus features.
layout: default
---

The V8Preview6 API has added the ability to retrieve additional property using [`IOperationService.GetCashRegisterStatus`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetCashRegisterStatus.htm).

Newly added fields:
* [`SerialNumber`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Device_Results_CashRegisterStatus_SerialNumber.htm) - Cash register serial number
* [`SessionNumber`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Device_Results_CashRegisterStatus_SessionNumber.htm) - Current session number
* [`RefundsCount`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Device_Results_CashRegisterStatus_RefundsCount.htm) - Number of refunds per shift
* [`RefundsSum`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Device_Results_CashRegisterStatus_RefundsSum.htm)- Sum of refunds per shift
* [`CancelCount`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Device_Results_CashRegisterStatus_CancelCount.htm) - Number of cancellations per shift
* [`CancelSum`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Device_Results_CashRegisterStatus_CancelSum.htm) - Sum of cancellations per shift
* [`SalesCount`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Device_Results_CashRegisterStatus_SalesCount.htm) - Number of sales per shift
* [`SalesSum`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Device_Results_CashRegisterStatus_SalesSum.htm) - Sum of sales per shift
* [`SalesSumTotal`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Device_Results_CashRegisterStatus_SalesSumTotal.htm) - Total sales of cash register
* [`CashPaymentSum`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Device_Results_CashRegisterStatus_CashPaymentSum.htm) - Sum cash payment sales per shift
* [`NonCashPaymentsSum`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Device_Results_CashRegisterStatus_NonCashPaymentsSum.htm) - Sum non-cash payment sales per shift

To receive data you must pass additional values ​​in the [`GetCashRegisterStatusTask.StatusFields`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Device_Tasks_GetCashRegisterStatusTask_StatusFields.htm) that correspond to the required data. 
List of all possible values- [`CashRegisterStatusField`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Device_Tasks_CashRegisterStatusField.htm).