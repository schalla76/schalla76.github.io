---
layout: post
title: "Debugging Settings for .Net"
date: 2019-03-01
categories: debug .net
---

## Generate INI files

Run the PS command `SDAGenerateINIFileForDebug` to generate the ini files.<br>
[Source url](https://www.jonathanmedd.net/2015/01/how-to-make-use-of-functions-in-powershell.html)

## Environment variables

Set `COMPlus_ZapDisable=1` to avoid loading the pre-compiled to native code.  
[Source url](https://docs.microsoft.com/en-us/visualstudio/debugger/jit-optimization-and-debugging?view=vs-2017)

Set `COMPlus_JITMinOpts=1`  
[Source url](https://github.com/dotnet/coreclr/issues/20647)

## Visual Studio settings

Check/Uncheck below debug optons in Visual Studio.

- [x] `Suppress JIT optimization on module load (Managed only)`
- [ ] `Enable Just My Code`
- [x] `Supress JIT optimization on module load`
- [x] `Use Managed Compatability Mode` -- This option helps in viewing the COM components in debug mode
- [x] `Use Native Compatability Mode` -- This option helps in viewing the COM components in debug mode.

## Visual Studio Debug Tips

- After attaching to the VS code modify the Web.config or global.asmx file.
- This will reloaded all the binaries with no optmizations.

### The following will invoke an app domain restart

- Modification to web.config or Global.asax
- Change to the contents of the application’s bin directory
- Change to the physical path of the virtual directory
- Deletion of the subdirectory

### Viewing byte array in debug window

- Use below expression to convert the bytes into string

```c#
//buffer is byte array
System.Text.Encoding.UTF8.GetString(buffer, 0, buffer.Length)
```

## Reverse Engineer tools

### .Net Reflector

Used for Visual Studio debugging

### dnSpy

Used for editing binaries and debugging  
[Source link](https://github.com/0xd4d/dnSpy)

### ILSpy

Core framework used for debugging  
[Source link](https://github.com/icsharpcode/ILSpy)

## SDx logs

### Desktop Client tracing

- Update <Target Folder>\SmartPlant\Foundation\SPFDesktopClient\CurrentVersion\tracesettings.config with below for getting complete trace

```xml
<categorySources>
    <!-- Possible All, Critical, Error, Warning, Information, Verbose, Off -->
    <add switchValue="All" name="General">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
    <add switchValue="All" name="Performance">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
    <add switchValue="All" name="WebRequest">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
    <add switchValue="All" name="Schema">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
    <add switchValue="All" name="DatabaseAccess">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
    <add switchValue="All" name="SQL">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
    <add switchValue="All" name="Core">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
    <add switchValue="All" name="Email">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
    <add switchValue="All" name="Login">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
    <add switchValue="All" name="TEF">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
    <add switchValue="All" name="RemoteServices">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
    <add switchValue="All" name="DocMgmt">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
    <add switchValue="All" name="Loader">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
    <add switchValue="All" name="Scheduler">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
    <add switchValue="All" name="Custom">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
    <add switchValue="All" name="APITrace">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
    <add switchValue="All" name="Metering">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
    <add switchValue="All" name="IHUB">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
    <add switchValue="All" name="Cache">
      <listeners>
        <add name="Rolling TraceListener"/>
      </listeners>
    </add>
  </categorySources>
```

- Update <Target Folder>\SmartPlant\Foundation\SPFDesktopClient\settings.xml file as below

```xml
<configuration>
  <debug level="9" dir="C:\Trace\Clientlogs" />
  <images dir="..\..\Icons" />
  <Exported dir="" />
</configuration>
```
