---
layout: post
title: "Powershell basic commands"
date: 2019-04-01
categories: powershell
---

- [Splitting the path](#splitting-the-path)
- [Get All environment variables](#get-all-environment-variables)
- [Powershell version](#powershell-version)
- [Powershell History using PSReadline](#powershell-history-using-psreadline)
- [Get Profile path](#get-profile-path)
- [Calling the powershell functions](#calling-the-powershell-functions)
- [Get Powershell modules path](#get-powershell-modules-path)
- [Get/Set Reqiestry Keys](#getset-reqiestry-keys)
  - [List all the drives including the registy Keys](#list-all-the-drives-including-the-registy-keys)
  - [Change to a drives](#change-to-a-drives)
  - [To set a Keys](#to-set-a-keys)
- [Setting Execution Policy](#setting-execution-policy)
- [Get the file abosulte file path](#get-the-file-abosulte-file-path)
- [Power shell history location](#power-shell-history-location)
- [Power shell command to find the assemble target framework](#power-shell-command-to-find-the-assemble-target-framework)
- [Az cli commands](#az-cli-commands)
- [Get Path of the exe or command](#get-path-of-the-exe-or-command)

## Splitting the path

```ps
$env:path.split(";")
```

## Get All environment variables

```ps
gci env:
```

## Powershell version

```ps
$PSVersionTable.PSVersion
$PSVersionTable
```

## Powershell History using PSReadline

```ps
(Get-PSReadlineOption).HistorySavePath
```

## Get Profile path

```ps
$profile
$PROFILE | Get-Member -MemberType noteproperty
$PROFILE | Get-Member -MemberType noteproperty | select name
```

## Calling the powershell functions

[Source link](https://www.jonathanmedd.net/2015/01/how-to-make-use-of-functions-in-powershell.html)

## Get Powershell modules path

```ps
$env:PSModulePath
$env:PSModulePath.split(";")
```

## Get/Set Reqiestry Keys

### List all the drives including the registy Keys

```ps
Get-PSDrive
```

### Change to a drives

```ps
CD HKCU:\
```

### To set a Keys

```ps
Set-ItemProperty -path 'HKCU:\Control Panel\Colors' -Name 'ActiveBorder' -value '0 255 255'
```

## Setting Execution Policy

```ps
Get-ExecutionPolicy -List
Set-ExecutionPolicy -ExecutionPolicy Unrestricted
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope LocalMachine
```

## Get the file abosulte file path

```ps
get-childitem -recurse | where {$_.extension -like ".xls*"} | % { Write-Host $_.FullName }
```

## Power shell history location

- By default command history is saved in %userprofile%AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
- By default cmder history is saved in cmder-directory\config\.history

## Power shell command to find the assemble target framework

```ps
# Get .net framework runtime verion
[Reflection.Assembly]::ReflectionOnlyLoadFrom(<assembly path>).ImageRuntimeVersion

# Get assemble target framework
[Reflection.Assembly]::ReflectionOnlyLoadFrom(<assembly path>).CustomAttributes |
Where-Object {$_.AttributeType.Name -eq "TargetFrameworkAttribute" } |
Select-Object -ExpandProperty ConstructorArguments |
Select-Object -ExpandProperty value
```

## Az cli commands

```powershell

# Login into Azure
az login

# List accounts
az account list --query '[].[name, id]' -o tsv

# Set context account
az account set --subscription '[Subscription Name]'

# Start lab VM
az lab vm start --name [VMName] --resource-group [Resour Group Name] --lab-name [Lab Name]

# List webapps
az webapp list --query '[].{rg: resourceGroup, url:defaultHostName, name:name}' | ConvertFrom-Json -AsHashtable

# List VMs in L

az lab vm list --lab-name [lab name] --resource-group [resource group name]
az lab vm list --lab-name [lab name] --resource-group [resource group name] --query '[].{VMName:fqdn}' | ConvertFrom-Json -AsHashtable

```

## Get Path of the exe or command

```powershell
# This gets the command
get-command ws
# This gets the path of the command
(get-command ws).path
```
