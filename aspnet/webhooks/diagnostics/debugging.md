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
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="46eb0-103">depuración de ASP.NET WebHooks</span><span class="sxs-lookup"><span data-stu-id="46eb0-103">ASP.NET WebHooks debugging</span></span>

## <a name="debugging-in-azure"></a><span data-ttu-id="46eb0-104">Depuración en Azure</span><span class="sxs-lookup"><span data-stu-id="46eb0-104">Debugging in Azure</span></span>

<span data-ttu-id="46eb0-105">Para depurar la aplicación web mientras se ejecuta en Azure, consulte el tutorial Solucionar problemas de [una aplicación web en Azure App Service con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="46eb0-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="46eb0-106">Depuración con origen y símbolos</span><span class="sxs-lookup"><span data-stu-id="46eb0-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="46eb0-107">Además de depurar su propio código, es posible depurar directamente en Microsoft ASP.NET WebHooks y, de hecho, en todo .NET.</span><span class="sxs-lookup"><span data-stu-id="46eb0-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="46eb0-108">Esto funciona independientemente de si usted depura localmente o remotamente.</span><span class="sxs-lookup"><span data-stu-id="46eb0-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="46eb0-109">En primer lugar, configure Visual Studio para buscar el origen y los símbolos yendo a **Depurar** y, a continuación, a **Opciones y configuración**.</span><span class="sxs-lookup"><span data-stu-id="46eb0-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="46eb0-110">Establezca las opciones como esta:</span><span class="sxs-lookup"><span data-stu-id="46eb0-110">Set the options like this:</span></span>

![Opciones y ajustes](_static/SourceSymbols.png)

<span data-ttu-id="46eb0-112">A continuación, agregue un enlace a [symbolsource.org](http://symbolsource.org) para descargar la fuente y los símbolos.</span><span class="sxs-lookup"><span data-stu-id="46eb0-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="46eb0-113">Vaya a la pestaña **Símbolos** del menú anterior y agregue lo siguiente como ubicación de símbolo:</span><span class="sxs-lookup"><span data-stu-id="46eb0-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="46eb0-114">Además, asegúrese de que el directorio de caché tiene un nombre corto; de lo contrario, los nombres de archivo pueden obtener demasiado tiempo, lo que hará que los símbolos no se carguen.</span><span class="sxs-lookup"><span data-stu-id="46eb0-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="46eb0-115">Una ruta de acceso de ejemplo es:</span><span class="sxs-lookup"><span data-stu-id="46eb0-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="46eb0-116">La configuración debe ser similar a esto:</span><span class="sxs-lookup"><span data-stu-id="46eb0-116">The settings should look similar to this:</span></span>

![Ejemplo de ubicación del archivo de símbolos de opciones](_static/SymSource.png)
