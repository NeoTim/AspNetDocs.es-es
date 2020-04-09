---
uid: webhooks/diagnostics/debugging
title: depuración de ASP.NET WebHooks (webHooks) Microsoft Docs
author: rick-anderson
description: Cómo depurar ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 2f1f8196478e7025a0467acb945d9ed36c8fd0ca
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675370"
---
# <a name="aspnet-webhooks-debugging"></a>depuración de ASP.NET WebHooks

## <a name="debugging-in-azure"></a>Depuración en Azure

Para depurar la aplicación web mientras se ejecuta en Azure, consulte el tutorial Solucionar problemas de [una aplicación web en Azure App Service con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Depuración con origen y símbolos

Además de depurar su propio código, es posible depurar directamente en Microsoft ASP.NET WebHooks y, de hecho, en todo .NET. Esto funciona independientemente de si usted depura localmente o remotamente. En primer lugar, configure Visual Studio para buscar el origen y los símbolos yendo a **Depurar** y, a continuación, a **Opciones y configuración**. Establezca las opciones como esta:

![Opciones y ajustes](_static/SourceSymbols.png)

A continuación, agregue un enlace a [symbolsource.org](http://symbolsource.org) para descargar la fuente y los símbolos. Vaya a la pestaña **Símbolos** del menú anterior y agregue lo siguiente como ubicación de símbolo:

```
http://srv.symbolsource.org/pdb/Public
```

Además, asegúrese de que el directorio de caché tiene un nombre corto; de lo contrario, los nombres de archivo pueden obtener demasiado tiempo, lo que hará que los símbolos no se carguen. Una ruta de acceso de ejemplo es:

```
C:\SymCache
```

La configuración debe ser similar a esto:

![Ejemplo de ubicación del archivo de símbolos de opciones](_static/SymSource.png)
