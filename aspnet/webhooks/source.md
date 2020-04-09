---
uid: webhooks/source
title: ASP.NET el código fuente de WebHooks y los paquetes NuGet Microsoft Docs
author: rick-anderson
description: Vínculos a ASP.NET código fuente de WebHooks y paquetes NuGet
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: ad368125878871c0e38f35152c86fe4eea143924
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675321"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>ASP.NET el código fuente de WebHooks y los paquetes NuGet

Microsoft ASP.NET WebHooks forma parte de la familia de módulos Microsoft ASP.NET y se hospeda como un proyecto de [código abierto en GitHub.](https://github.com/aspnet/WebHooks) Esto significa que aceptamos contribuciones, pero por favor, consulte las Directrices de [contribución](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) antes de enviar una solicitud de extracción.

Esta documentación en línea que está leyendo ahora también se hospeda como [código abierto en GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) y también acepta contribuciones.

## <a name="nuget-packages"></a>Paquetes NuGet

Los [paquetes NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) se dividen en tres partes:

* [Común](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): Un paquete común que se comparte entre remitentes y receptores.

* [Remitente](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): Un conjunto de paquetes que admiten el envío de sus propios WebHooks a otros. La funcionalidad para enviar WebHooks se describe con más detalle en Envío de [WebHooks](sending/senders.md).

* [Receptores](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): Un conjunto de paquetes que admiten la recepción de WebHooks de otros. La funcionalidad para recibir WebHooks se describe con más detalle en Recepción de [WebHooks](receiving/index.md).
