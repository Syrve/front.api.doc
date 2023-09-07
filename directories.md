---
title: Folder structure 
layout: default
---
# Overview #
Each plugin is installed in its own folder within the *Plugins* folder ([V6+]({{ site.baseurl }}/2015/10/23/load-plugins-from-subdirectories.html)). One folder ‒ one plugin. At least two files must be in the folder: a plugin DLL file and Manifest.xml. Along with these files, the plugin folder and subfolders may include additional libraries, configuration files, resources, and others.

The Syrve POS app receives complete details on the plugin from the *Manifest.xml* file.

Syrve POS API contract files (*Resto.Front.Api.Vx.dll*, *Resto.Front.Api.Vx.xml*) are needed only during plugin development and compilation phase, they should not be distributed and installed with the plugin, in runtime contracts will be used from Syrve POS application folder directly. At connecting the contracts to the project via direct link to the dll-file should be set to `Copy Local = False`.
When [NuGet-пакета](https://www.nuget.org/profiles/iiko) чis connected via PackageReference for versions before V7Preview2 copying needs to be disabled via `<PrivateAssets>all</PrivateAssets>` ([description](https://docs.microsoft.com/en-us/nuget/consume-packages/package-references-in-project-files)), beginning with V7Preview2 copying is automatically deactivated.

# Manifest.xml file structure #

### Required Parameters ###
- `FileName` — string, name of a plugin DLL file (no path, only file name and extension). Such file should be located next to *Manifest.xml*. The case may or may not make any difference depending on the file system, therefore, as a general rule, the case must be deemed important, and the file name specified in the manifest should comply.
- `TypeName` — string, plugin class name within the DLL file `FileName` (full name, including namespace). Case sensitive.
- `ApiVersion` — string, API version used by the plugin (V4/V5/V6Preview4/V6/…). Case sensitive. The specified version must correspond to the implemented interface version `IFrontPlugin`.
- `LicenseModuleId` — 32-bit character integer number, license module ID.  This number must correspond to the value specified in the [`PluginLicenseModuleId`](https://syrve.github.io/front.api.sdk/v6/html/T_Resto_Front_Api_Attributes_PluginLicenseModuleIdAttribute.htm) attribute.

### Additional Parameters ###
- `IsSingleInstance` — `true`/**`false`**, whether the plugin should be run singularly within the group of terminals. False by default, which means that the plugin will be started at each terminal where installed. Use this flag to make the system run the plugin on one terminal only; this may be particularly handy if it is not required that several plugin copies process the same data. For instance, if a plugin tracks order changes and sends them to an external server, one plugin is enough. Since order details are synced between terminals within a group, plugin copies that run simultaneously on several terminals would see the same orders and would send duplicate calls to the external server with the same data. Corresponds to the [`SingleInstancePlugin`](https://syrve.github.io/front.api.sdk/v5/html/T_Resto_Front_Api_V5_Attributes_SingleInstancePluginAttribute.htm) attribute in versions before V5.
- `RestartOnCrash` — **`true`**/`false`, whether the plugin must be restarted after the crash or not. `True` by default, which means that the crashed plugin will be restarted. The plugin is considered crashed if its host process ends with a non-zero return code. The following limitations are set:
  - If this setting is missing in the manifest, the plugin will be restarted only if it crashed after 10 seconds of operation. It is required to avoid restarting of plugins that run a number of checks at the start and if operating conditions are not met, they shut down through a deliberate crash. To shut down the plugin properly, the [`PluginContext.Shutdown()`](https://syrve.github.io/front.api.sdk/v6/html/M_Resto_Front_Api_PluginContext_Shutdown.htm) method would be a better solution to use, but, in practice, most of the plugins are wrongfully made. The default behavior may change in the future. If the manifest distinctively specifies that the restart is required, the minimum run duration before the crash is not limited.
  - In the case of irreparably broken plugins, there is a limitation set: no more than four crashes per hour. If the plugin crashes for the fifth time within one hour, it will be considered broken and will not be restarted in this Syrve POS session.

# Outdated Approach for V4/V5 {#legacyScanning}
V5 API and earlier versions used another approach that implied several DLL files to be placed in the plugin folder (the plugin and its dependencies). Syrve POS scanned all such DLLs to find the class that implemen `IFrontPlugin` marker interface and read additional parameters from the found class attributes. This approach had a number of shortcomings and, starting from V6, the manifest was used instead. The manifest is required for V6, but V4/V5 API versions support both: if there is no manifest, DLL files are scanned, but if there is the *Manifest.xml* file, it is used to read the data. The approach that implies the use of the manifest file is more reliable and works faster than scanning DLL files, therefore, it is recommended that this optional file is added to the V4/V5 plugin folders.

However, V4/V5 plugins must hold to the contract terms and specify attributes for the scanning even if the manifest is used. As manifests are supported starting from Syrve POS version 6.4, earlier versions would always scan DLL files regardless of the manifest availability.

# Examples #
```xml
<?xml version="1.0" encoding="utf-8" ?>
<Manifest xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.w3.org/2001/XMLSchema ../Binaries/iiko/Manifest.xsd">
  <FileName>Resto.Front.Api.SamplePlugin.dll</FileName>
  <TypeName>Resto.Front.Api.SamplePlugin.SamplePlugin</TypeName>
  <ApiVersion>V6</ApiVersion>
  <LicenseModuleId>21005108</LicenseModuleId>
</Manifest>
```

This [example](https://github.com/syrve/front.api.sdk/blob/master/sample/v6/Resto.Front.Api.SamplePlugin/Manifest.xml), as well as the [*Manifest.xsd*](https://github.com/syrve/front.api.sdk/blob/master/sample/v6/Binaries/Manifest.xsd) file scheme, can be found in the Syrve POS API SDK offline package [Syrve POS API SDK](https://github.com/syrve/front.api.sdk/tree/master/sample/v6).