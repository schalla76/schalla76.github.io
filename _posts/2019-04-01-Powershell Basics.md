---
layout: post
title: "Powershell basic commands"
date: 2019-04-01
categories: powershell
---

## Spliting the path

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
Set-Itemproperty -path 'HKCU:\Control Panel\Colors' -Name 'ActiveBorder' -value '0 255 255'
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
