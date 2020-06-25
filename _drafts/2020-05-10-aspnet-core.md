---
layout: post
title: "ASPNet Core"
date: 2020-05-10
categories: aspnet, entityframework, udemy
---

## Course

[Udemy Course: Build an app with ASPNET Core and Angular from scratch ](https://www.udemy.com/course/build-an-app-with-aspnet-core-and-angular-from-scratch/)

[Udemy Author: Neil Cummings](https://github.com/TryCatchLearn)

[Github Source](https://github.com/TryCatchLearn/DatingApp30)

## Creating Angular

```cmd
ng new <client name>
```

## Get dotnet command line help

```cmd
dotnet -h
dotnet new -h
```

## Creating ASP.Net Core Web API

```cmd
dotnet new webapi
```

### Adding files to project

- Add Data/DataContext inheriting DBContext
- Add below line statup.cs/ConfigureServices :

```cs
services.AddDbContext<DataContext>(option => option.UseSqlite("ConnectionString"))
```

- Add Model/<files.cs> is mapped to table

### Adding Repository files

## EntityFrameWork Commands

### Initial DB Create

```cmd
dotnet ef migrations add InitialCreate
```

### DB Update

```cmd
dotnet ef database update
```

### DB Drop

```cmd
dotnet ef database drop
```

### Adding test cases

[Integrated testing](https://asp.net-hacker.rocks/2019/01/18/integration-testing-data-access-dotnetcore.html)
