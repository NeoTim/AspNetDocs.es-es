---
uid: webhooks/receiving/dependencies
title: ASP.NET las dependencias del receptor de WebHooks Microsoft Docs
author: rick-anderson
description: Dependencias del receptor e inserción de dependencias en ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: b50442b3d95512bc0db7583b93de3bbef2d4bb4a
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675206"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>dependencias del receptor de ASP.NET WebHooks

Microsoft ASP.NET WebHooks está diseñado teniendo en cuenta la inserción de dependencias. La mayoría de las dependencias del sistema se pueden reemplazar con implementaciones alternativas mediante un motor de inserción de dependencias.

Consulte [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) para obtener una lista de dependencias del receptor. Si no se ha registrado ninguna dependencia, se utiliza una implementación predeterminada. Consulte [ReceiverServices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) para obtener una lista de las implementaciones predeterminadas.
