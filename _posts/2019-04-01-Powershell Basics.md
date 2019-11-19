---
layout: post
title: "Powershell basic commands"
date: 2019-04-01
categories: powershell
---

## Spliting the path

`$env:path.split(";")`

## Get All environment variables

`gci env:`

## Powershell version

`$PSVersionTable.PSVersion`

`$PSVersionTable`

## Powershell History using PSReadline

`(Get-PSReadlineOption).HistorySavePath`

## Get Profile path

`$profile`

`$PROFILE | Get-Member -MemberType noteproperty`

`$PROFILE | Get-Member -MemberType noteproperty | select name`

## Calling the powershell functions

[Source link](https://www.jonathanmedd.net/2015/01/how-to-make-use-of-functions-in-powershell.html)

## Get Powershell modules path

`$env:PSModulePath`

`$env:PSModulePath.split(";")`

## Get/Set Reqiestry Keys

### List all the drives including the registy Keys

`Get-PSDrive`

### Change to a drives

`CD HKCU:\`

### To set a Keys

`Set-Itemproperty -path 'HKCU:\Control Panel\Colors' -Name 'ActiveBorder' -value '0 255 255'`

## Setting Execution Policy

`Get-ExecutionPolicy -List`

`Set-ExecutionPolicy -ExecutionPolicy Unrestricted`

`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope LocalMachine`
