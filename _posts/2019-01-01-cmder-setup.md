---
layout: post
title: "Cmder Setup Steps"
date: 2019-01-01
categories: cmder setup
---

## Working director

Set this on the short cut working directory.

## For Command history Install the PSReadLine

`Install-Module -Name PSReadLine`

Copy the Set-PSReadLineKeyHandler instructions to "C:\Program Files\cmder_mini\vendor\profile.ps1" file

Copy below to "C:\Program Files\cmder_mini\vendor\profile.ps1" file

```ps
$MyModules = "C:\Users\Administrator\Work\bitbucket\myutilities\MyScripts\MyModules"

if ( -not $env:PSModulePath.Contains($MyModules) ) {
$env:PSModulePath = $env:PSModulePath.Insert(0, "$MyModules;")
}
```
