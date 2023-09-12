---
title: API V8Preview6 cash register operating
layout: default
---

Methods for working directly with cash register have been added to the V8Preview6 API

Opening a cash register session at cash register
[`OpenCashRegisterSession`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_OpenCashRegisterSession.htm)
Supported only if module 21052601 is included in the license. For correct operation, the device must be running and the shift on the device must be closed.
To perform the operation, the user must have the CAN_EXECUTE_FISCAL_REGISTER_COMMANDS permission.

Closing a cash register session (printing a Z report)
[`DoZReport`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_DoZReport.htm)
Supported only if module 21052601 is included in the license. For correct operation, the device must be running and the shift on the device must be closed.
To perform the operation, the user must have the CAN_EXECUTE_FISCAL_REGISTER_COMMANDS permission.
If the parameter [`printCashRegisterTape`] the daily log will be printed along with the Z report.

Opening a cash drawer 
[`CashRegisterOpenDrawer`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_CashRegisterOpenDrawer.htm)
To perform the operation, the user must have the CAN_EXECUTE_FISCAL_REGISTER_COMMANDS permission.
This version only supports opening a cash drawer connected to the cash register; working with an external cash drawer is not supported.

Receiving the current status of the cash register 
[`GetCashRegisterStatus`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetCashRegisterStatus.htm)
To perform the operation, the user must have the CAN_EXECUTE_FISCAL_REGISTER_COMMANDS permission.
The method accepts a list [`CashRegisterStatusField`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Device_Tasks_CashRegisterStatusField.htm)
and fills the properties of the returned object [`CashRegisterStatus`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Device_Results_CashRegisterStatus.htm) corresponding to those passed in the list.
When passing an empty list, the default object [`CashRegisterStatus`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Device_Results_CashRegisterStatus.htm) is returned.
 
Returns additional supported operations
[`GetQueryInfo`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_GetQueryInfo.htm)
Returns additional supported operations [`QueryInfoResult.SupportedCommands`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Device_Results_QueryInfoResult_SupportedCommands.htm)
that can be called by the method [`CashRegisterDirectIO`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_CashRegisterDirectIO.htm)

Performing an additional operation
[`CashRegisterDirectIO`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_CashRegisterDirectIO.htm)
To perform the operation, the user must have the CAN_EXECUTE_FISCAL_REGISTER_COMMANDS permission.
Allows you to perform an additional operation, you need to pass it in the parameter [`CommandExecute`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Device_Tasks_CommandExecute.htm)
the name of the additional operation [`Name`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Device_Tasks_CommandExecute_Name.htm) and parameter values [`Parameters`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Device_Tasks_CommandExecute_Parameters.htm)
This method is used to perform operations specific to a particular cash register ​​model that do not correspond to any method of the general interface[`ICashRegister `](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Devices_ICashRegister.htm)
 
Сash register start
[`CashRegisterStart`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_CashRegisterStart.htm)
 
Сash register stop
[`CashRegisterStop`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_CashRegisterStop.htm)
Devices start when SyrvePOS starts and stop when SyrvePOS shuts down (if autostart is enabled in the device settings).
The cash register start and stop commands are needed so that you can stop the device and free up a COM port or other port that the device occupies and then start the device
without restarting SyrvePOS.

Checking the marking code 
[`CheckFfd12Marking`](https://syrve.github.io/front.api.sdk/v8/html/M_Resto_Front_Api_IOperationService_CheckFfd12Marking.htm)
To perform the operation, the user must have the CAN_EXECUTE_FISCAL_REGISTER_COMMANDS permission.
Checks the marking code in FFD format 1.2 of the check item [`ChequeSale`](https://syrve.github.io/front.api.sdk/v8/html/T_Resto_Front_Api_Data_Device_Tasks_ChequeSale.htm)
The parameter must have a non-empty property [`Ffd12`](https://syrve.github.io/front.api.sdk/v8/html/P_Resto_Front_Api_Data_Device_Tasks_ChequeSale_Ffd12.htm)