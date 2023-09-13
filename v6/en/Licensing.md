---
title: Licensing
layout: default
---
Plugin licensing is compulsory. Before you start developing a plugin, get the developer license, which allows you to start an Syrve POS test instance for the development, debugging, and testing. Users that want to install a ready-made plugin need to purchase a corresponding license. This approach protects plugin developers from unauthorized use of their plugins and allows Syrve to control certain functions.

## Licensing Plans ##

Technically, there are two licensing plans:

- Pay per plugin instance: the number of concurrently running plugin instances is limited.
- Pay per external connection to plugin: the number of devices concurrently working with is limited.

### Pay per plugin instance ##

This option can be used for complete and fully functional plugins with intrinsic value. For instance, a plugin that displays current order items on the customer screen should be licensed under this plan. Licenses are monitored automatically by Syrve POS — the plugin will be loaded only if the license is available. To use a plugin simultaneously on several POS terminals, a license should be issued for the required number of plugin instances. Thus, if a plugin should work on 10 POS terminals, a license for 10 plugin copies is required.

To protect the plugin under this plan, you need to:

- Register in Syrve to get a licensed module ID.
- Mark the plugin class with the [`PluginLicenseModuleId`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Attributes_PluginLicenseModuleIdAttribute.htm) attribute and specify the acquired registration ID.

### Pay per external connection to plugin ##
This plan can work for plugins used as intermediaries between Syrve POS and external devices. In this case, a plugin controls licenses. Along with the module licensing, other additional restrictions can be imposed, the number of slots being the main one. A module slot is a kind of cell that can be temporarily taken by a plugin or a device connected to it. Two devices can use one slot in turns, but they will need two slots to be able to work at the same time. Suppose a plugin enables the operation of mobile terminals in the store consisting of two groups of sections, each of which has one plugin instance installed, whereas both groups have 5 mobile terminals each. Such a setting requires a license for 10 slots. In this case, the number of active instances is not taken into account.

To implement the protection procedure, follow this:

- Register in Syrve to get IDs for two licensed modules:
    - Plugin module, a formality unlike the previous plan, which is only required to link the plugin via the [`PluginLicenseModuleId`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Attributes_PluginLicenseModuleIdAttribute.htm)attribute used to start the plugin. In practice, this can be a free-of-charge module with no quantity restriction.
    - External connection module to be further used.
- Provide for the device identification so that each device has a unique static ID.
- When establishing an external connection, invoke the [`AcquireSlot`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_ILicensingService_AcquireSlot.htm) method of the [`ILicensingService`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_ILicensingService.htm) service and specify the module ID acquired at the registration and the device ID. Remember the link to the object returned by the method.
- When completing the external connection, invoke the `Dispose` method for the object returned by the `AcquireSlot` method at the time the connection was established.

When `AcquireSlot` is invoked, one of the module slots is marked as used by the specified device. If there are no free slots, the method generates [`InsufficientLicenseException`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Exceptions_InsufficientLicenseException.htm), which means that a maximum permissible number of devices is connected, and the next device in queue must be declined.

It should be noted that a slot can be freed up at any time: the plugin or Syrve POS app operation can be interrupted by an unexpected workstation power-off, network failure (license handling may require network communication), and so on. To avoid the slot «leakage» when slots are taken but not released, it is important to provide a uniform device identification. If, for instance, a device had connected to the plugin and used a slot, then in case of an unexpected reboot, this device will retake its slot provided it has the same ID. If it has a new ID, the device will need one more slot, and the first slot will remain permanently used.

## Registration ##
A developer needs to register their plugin as a licensed module in Syrve, and a user needs to obtain a license for this module. At the registration, a plugin is assigned an ID, an integer number, which is, in fact, the module ID. This can be whether a new module created specifically for this plugin or a multipurpose module used for some minor individual modifications. A developer links a plugin to the module by specifying its ID using the   [`PluginLicenseModuleId`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Attributes_PluginLicenseModuleIdAttribute.htm) attribute. A user needs to obtain a license that includes a corresponding module.

To register, obtain a developer license, obtain a licensed module ID, and sign a contract, contact your Syrve vendor.

## Links ##
- [Syrve API Documentation](https://en.syrve.help/articles/api/getting-started-api)