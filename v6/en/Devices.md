---
title: Equipment
layout: default
order: 7
---
To perform many operations on Syrve POS-powered POS—from order editing to payment—you may often need certain units of equipment. Among those may be fiscal registers, printers, scales, cash drawers, customer screens, and others.

Earlier versions (before 6.2) could only handle a limited number of equipment models. Usually, those were the most popular ones. Using the API, we can now support any model of fiscal registers and scales.

## Fiscal Register ##

In the API, fiscal registers are given as the [`ICashRegister`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Devices_ICashRegister.htm) type which contains only the device identifier and name. For the most part, [`ICashRegister`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Devices_ICashRegister.htm) is a list of commands, such as till shift opening/closing, order payment/refund, money deposit/withdrawal, and others. Commands are preconfigured, whereas the implementation depends on the device. More often than not, fiscal register commands return the current status as the [`CashRegisterResult`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Device_Results_CashRegisterResult.htm) object containing the information about money counting devices, document numbers, tax amounts, and so on. 

Each fiscal register has its settings represented by the [`CashRegisterSettings`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Device_Settings_CashRegisterSettings.htm) type. When you add or edit a device in Syrve Office, you can view the settings and specify the values accordingly. [`CashRegisterSettings`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Device_Settings_CashRegisterSettings.htm) includes the following details:

- Name of a fiscal register model, for example, «MSTAR-TK. AFP protocol» ([`Code`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Device_Settings_DeviceSettings_Code.htm)).
- Fiscal data format version ([`OfdProtocolVersion`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Device_Settings_CashRegisterSettings_OfdProtocolVersion.htm)).
- Table of tax rates ([`FiscalRegisterTaxItems`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Device_Settings_CashRegisterSettings_FiscalRegisterTaxItems.htm)).
- Table of payment types — determined by the type ([`FiscalRegisterPaymentTypes`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Device_Settings_CashRegisterSettings_FiscalRegisterPaymentTypes.htm)).
- Number of characters per receipt row printed in a standard font ([`Font0Width`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Device_Settings_CashRegisterSettings_Font0Width.htm)).
- The ([`DeviceSetting`]()) settings are a list of extra settings. For example, COM port, TCP/TP address, data interchange rate, access password, and so on.

The equipment-to-Syrve POS connection diagram is the same for all types of devices. To begin using your fiscal printer, you need to register the device on the list of equipment. For this, you need to create a class implementing the [`ICashRegisterFactory`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Devices_ICashRegisterFactory.htm) that will represent your fiscal register and add the model to the list of equipment through the [`RegisterCashRegisterFactory(https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_RegisterCashRegisterFactory.htm)`]() method. 

[`ICashRegisterFactory`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Devices_ICashRegisterFactory.htm) should specify the device model name and settings. This way, you can find your fiscal register in Syrve Office by the model and add or edit its settings

The plugin sample implementing the integration with a fiscal register (printer) can be found in the `Resto.Front.Api.SampleCashRegisterPlugin` project.

## Scales ##

In the API, scales are given as the [`IScale`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Devices_IScale.htm) type. Like any other device, a scale includes the device identifier and name. The [`IScale`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Devices_IScale.htm) type contains one command — weigh ([`MeasureWeight()`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_Devices_IScale_MeasureWeight.htm)).

The weighing result is the [`ScaleWeightResult`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Data_Device_Results_ScaleWeightResult.htm) type object which includes the weight in kilos ([`Weight`](https://syrve.github.io/front.api.sdk/v7/html/P_Resto_Front_Api_Data_Device_Results_ScaleWeightResult_Weight.htm)).

To begin using your scale, you need to register the device on the list of equipment. For this, you need to create the [`IScaleFactory`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Devices_IScaleFactory.htm) type that will represent your scale and add the model to the list of equipment through [`RegisterScaleFactory(https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_IOperationService_RegisterScaleFactory.htm)`](). 

[`IScaleFactory`](https://syrve.github.io/front.api.sdk/v7/html/T_Resto_Front_Api_Devices_IScaleFactory.htm) should specify the device model name and settings. This way, you can find your scale in Syrve Office by the model and add or edit its settings. 

The plugin sample implementing the integration with a scale can be found in the `Resto.Front.Api.SampleScalePlugin` project.

### Device Automatic Startup ###

Syrve POS can start devices automatically. If the device is not running, any command would fail to execute save for the startup. To avoid starting up devices manually in Syrve Office («Equipment settings») or in Syrve POS («Tools» > «Equipment settings»), follow this. In the device settings in Syrve Office, check the «Autorun» option. In target methods (for example, DoCheque, OpenSession for FCR), add the device status check and throw the [`DeviceNotStartedException`](http://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Exceptions_DeviceNotStartedException.htm) exception:

```cs
private void CheckStarted()
{
    if (state != State.Running)
        throw new DeviceNotStartedException("Device not started");
}
```

When such an exception occurs, Syrve POS would try to start the device and then run the target command.
