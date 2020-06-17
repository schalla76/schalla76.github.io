---
layout: post
title: "Understanding SmartPlant Products"
date: 2020-04-20
categories: SmartPlant Products
---

## SDA Tokens

Once the system gets the tokens it only verifies the resource id and this happens in function Intergraph.WebApi.Core.Security.UseOAuthAuthentication in the dll Intergraph.WebApi.Core.dll

## SDA Picking Revision Schemes: GetApplicableRevSchemes

- Gets all the interfaces on the document Revision
- For each interface finds the related rev scheme (1. startingrev: list of revs to start)
- Gets the current create and all its parents (2. configs: config of interests except config top)
- Gets the rev classification and all its parents (3. classifications)
- Expands the revision scheme: config(SPFRevisionSchemeConfigurationItem), classification(SPFRevisionSchemeObjClass)
- Priority 1 rev schemes: Finds intersection between 1. startingrev, 2. config and 3. classifications
- Priority 2 rev schemes: Finds intersection between 1. startingrev, 2. config level 3. classifications
- Priority 3 rev schemes: Finds intersection between 1. startingrev, 2. classifications
- Priority 4 rev schemes: Finds intersection between 1. startingrev, 2. configs
- Priority 5 rev schemes: Finds intersection between 1. startingrev, 2. config level
- Priority 6 rev schemes: Finds 1. startingrevs with no configs, classifications and config level

## Routing of API in SDX

- The routes are selected in the class **Intergraph.SPF.Server.API.Infrastructure.Routing.DynamicControllerSelector** in the Intergraph.SPF.Server.API.dll
- For each type of route a dynamic controller is create based on the type of the route. This dynamic controllers namespace Intergraph.SPF.Server.API.Controllers in the Intergraph.SPF.Server.API.dll
-
