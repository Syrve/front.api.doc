---
title: Debugging
layout: default
redirect_from:
  - /v6/en/Debugging
  - /v7/en/Debugging
order: 2
---
When starting up, the Syrve POS application scans for plugins and launches `Resto.Front.Api.Host.exe` process-container for each of them, in the context of which the plugin will work. For the convenience of debugging, it is possible to run this process directly from the development interface, to do that in the plugin project debugging parameters you need to set the `Resto.Front.Api.Host.exe` host process as an external program and transfer the following command line arguments:

- `-a`, `--assembly`=file — full path to the plugin file
- `-c`, `--class`=name    — full plugin class name (including namespace)
- `-l`, `--log`=log       — full path to log file
- `-m`, `--module`=id     — license  module specified in `PluginLicenseModuleId` attribute  ([Licensing](Licensing)),
- `-n`, `--nowindow`      — do not display the console window

Then just start Syrve POS application once (without installing plugin into `Plugins` folder) and you will be able to start and stop plugin repeatedly directly from development environment (`F5 / Shift+F5` in Visual Studio, `Alt-F5` in Rider) without restarting Syrve POS. The debugger will connect automatically, therefore breakpoints will work, and debugging will be interrupted when exceptions arise (in Visual Studio/Rider you can set up for which types of exceptions you need or don't need to interrupt debugging). The log records (`PluginContext.Log`) will be duplicated in the console window, if it's not hided by `--nowindow` key, and in the Output window (`Ctrl+Alt+O`), if the debugger is connected.

To debug the plugin this way, the plugin developer's license is needed.

## Visual Studio

### Classic .csproj
Example of filling out the fields on the Debug tab for a project in the old format:

- Start Action: Start external program: `C:\Program Files\...\Resto.Front.Api.Host.exe.`.
- Start Options: Command line arguments: `/a="C:\Projects\Front.Api Sdk\Resto.Front.Api.SamplePlugin\bin\Debug\Resto.Front.Api.SamplePlugin.dll" /c=Resto.Front.Api.SamplePlugin.SamplePlugin /m=21005108 /l="C:\Projects\Front.Api Sdk\Logs\sample-plugin.log"`

### SDK-style .csproj
For the new project format, the debugging parameters have been transferred to a separate file *launchsettings.json*, which could be created/edited either manually or via UI: in the project properties Debug → General → Open debug launch profiles UI → Create a new profile → Executable. Example of the *launchsettings.json* content:

```json
{
  "profiles": {
    "Resto.Front.Api.SamplePlugin": {
      "commandName": "Executable",
      "executablePath": "C:\\Program Files\\...\\Front.Net\\Resto.Front.Api.Host.exe",
      "commandLineArgs": "/a=\"C:\\Projects\\Front.Api Sdk\\Resto.Front.Api.SamplePlugin\\bin\\Debug\\Resto.Front.Api.SamplePlugin.dll\" /c=Resto.Front.Api.SamplePlugin.SamplePlugin /m=21005108 /l=\"C:\\Projects\\Front.Api Sdk\\Logs\\sample-plugin.log\""
    }
  }
}
```

#### Note for .NET Standard
Currently, there is a [bug](https://github.com/dotnet/project-system/issues/5009) in Visual Studio that doesn't allow you to debug a plugin under .NET Standard by running it via the `Resto.Front.Api.Host.exe` process-container, which is run under the .NET Framework.

Solutions options:

- Compiling the plugin under the .NET Framework
- Use Rider or other development platform, which does not have this bug.
- Add a fictitious project under .NET Framework, set it as Startup Project and specify the same debugging parameters as for the plugin. Read more [here](https://stackoverflow.com/a/61033312).

## Rider
JetBrains Rider can set up its own Debug Configurations with the `Exe path` and Program `arguments` fields, as well as use the Visual Studio-compatible *launchsettings.json* format.

## Alternatives
If there is no plugin developer license, it will be necessary to install the plugin into the Plugins folder for Syrve POS application to launch it by standard way, but each time the plugin is edited the application needs to stop and start it again. To minimize the inconvenience in this case the following ways are helpful:

- In Visual Studio project settings at `Build` tab in section `Output` set as `Output path` the way to the subfolder of the plug-in in `Plugins` folder. As a result plug-in will be published to this folder after each building, it will not need of being copied manually.
- At the beginning of the plugin class constructor, add a call to `Debugger.Launch()` to get a offer to connect the debugger. Of course, you could attach it yourself by selecting `Debug → Attach to Process`, but that would require more clicks, besides you'd have to select from a few container processes with the same name.
