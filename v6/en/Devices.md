---
title: Equipment
layout: default
---
To perform many operations on Syrve POS-powered POS—from order editing to payment—you may often need certain units of equipment. Among those may be fiscal registers, printers, scales, cash drawers, customer screens, and others.

Earlier versions (before 6.2) could only handle a limited number of equipment models. Usually, those were the most popular ones. Using the API, we can now support any model of fiscal registers and scales.

## Fiscal Register ##

In the API, fiscal registers are given as the `ICashRegister` type which contains only the device identifier and name. For the most part, `ICashRegister`  is a list of commands, such as till shift opening/closing, order payment/refund, money deposit/withdrawal, and others. Commands are preconfigured, whereas the implementation depends on the device. More often than not, fiscal register commands return the current status as the `CashRegisterResult`object containing the information about money counting devices, document numbers, tax amounts, and so on. 

Each fiscal register has its settings represented by the `CashRegisterSettings` type.When you add or edit a device in Syrve Office, you can view the settings and specify the values accordingly. `CashRegisterSettings` includes the following details:

- Name of a fiscal register model, for example, «MSTAR-TK. AFP protocol» (`Code`).
- Fiscal data format version (`OfdProtocolVersion`).
- Table of tax rates (`FiscalRegisterTaxItems`).
- Table of payment types — determined by the type (`FiscalRegisterPaymentTypes`).
- Number of characters per receipt row printed in a standard font (`Font0Width`).
- The (`DeviceSetting`) settings are a list of extra settings. For instance, COM port, TCP/TP address, data interchange rate, access password, and so on.

The equipment-to-Syrve POS connection diagram is the same for all types of devices. To begin using your fiscal printer, you need to register the device on the list of equipment. For this, you need to create a class implementing the `ICashRegisterFactory`that will represent your fiscal register and add the model to the list of equipment through the `RegisterCashRegisterFactory()` method. 

`ICashRegisterFactory` should specify the device model name and settings. This way, you can find your fiscal register in Syrve Office by the model and add or edit its settings

The plugin sample implementing the integration with a fiscal register (printer) can be found in the `Resto.Front.Api.SampleCashRegisterPlugin` project.

## Scales ##

In the API, scales are given as the `IScale` type. Like any other device, a scale includes the device identifier and name. The `IScale` type contains one command — weigh (`MeasureWeight()`).

The weighing result is the `ScaleWeightResult` type object which includes the weight in kilos (`Weight`).

ДTo begin using your scale, you need to register the device on the list of equipment. For this, you need to create the `IScaleFactory`type that will represent your scale and add the model to the list of equipment through `RegisterScaleFactory()`. 

`IScaleFactory` should specify the device model name and settings. This way, you can find your scale in Syrve Office by the model and add or edit its settings. 

The plugin sample implementing the integration with a scale can be found in the `Resto.Front.Api.SampleScalePlugin` project.

### Device Automatic Startup ###

Syrve POS can start devices automatically. If the device is not running, any command would fail to execute save for the startup. To avoid starting up devices manually in Syrve Office («Equipment settings») or in Syrve POS («Tools» > «Equipment settings»), follow this. In the device settings in Syrve Office, check the «Autorun» option. In target methods (for instance, DoCheque, OpenSession for FCR), add the device status check and throw the [`DeviceNotStartedException`](http://iiko.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Exceptions_DeviceNotStartedException.htm) exception:

```cs
private void CheckStarted()
{
    if (state != State.Running)
        throw new DeviceNotStartedException("Device not started");
}
```

When such an exception occurs, Syrve POS would try to start the device and then run the target command.
