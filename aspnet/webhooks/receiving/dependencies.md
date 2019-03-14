---
uid: webhooks/receiving/dependencies
title: Dependencias del receptor de ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: Dependencias del receptor y la inserción de dependencias en ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: c44cfe3ed310aa728a989b108c410e8786e4f514
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048732"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>Dependencias del receptor de WebHooks de ASP.NET

Microsoft ASP.NET WebHooks está diseñado con la inserción de dependencias en mente. La mayoría de las dependencias en el sistema se pueden reemplazar con implementaciones alternativas mediante un motor de inyección de dependencia.

Consulte [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) para obtener una lista de dependencias del receptor. Si no se ha registrado ninguna dependencia, se usa una implementación predeterminada. Consulte [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) para obtener una lista de las implementaciones predeterminadas.
