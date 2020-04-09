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
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="1c9e1-103">ASP.NET el código fuente de WebHooks y los paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="1c9e1-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="1c9e1-104">Microsoft ASP.NET WebHooks forma parte de la familia de módulos Microsoft ASP.NET y se hospeda como un proyecto de [código abierto en GitHub.](https://github.com/aspnet/WebHooks)</span><span class="sxs-lookup"><span data-stu-id="1c9e1-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="1c9e1-105">Esto significa que aceptamos contribuciones, pero por favor, consulte las Directrices de [contribución](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) antes de enviar una solicitud de extracción.</span><span class="sxs-lookup"><span data-stu-id="1c9e1-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="1c9e1-106">Esta documentación en línea que está leyendo ahora también se hospeda como [código abierto en GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) y también acepta contribuciones.</span><span class="sxs-lookup"><span data-stu-id="1c9e1-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="1c9e1-107">Paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="1c9e1-107">NuGet packages</span></span>

<span data-ttu-id="1c9e1-108">Los [paquetes NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) se dividen en tres partes:</span><span class="sxs-lookup"><span data-stu-id="1c9e1-108">The [NuGet packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are divided into three parts:</span></span>

* <span data-ttu-id="1c9e1-109">[Común](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): Un paquete común que se comparte entre remitentes y receptores.</span><span class="sxs-lookup"><span data-stu-id="1c9e1-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="1c9e1-110">[Remitente](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): Un conjunto de paquetes que admiten el envío de sus propios WebHooks a otros.</span><span class="sxs-lookup"><span data-stu-id="1c9e1-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="1c9e1-111">La funcionalidad para enviar WebHooks se describe con más detalle en Envío de [WebHooks](sending/senders.md).</span><span class="sxs-lookup"><span data-stu-id="1c9e1-111">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/senders.md).</span></span>

* <span data-ttu-id="1c9e1-112">[Receptores](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): Un conjunto de paquetes que admiten la recepción de WebHooks de otros.</span><span class="sxs-lookup"><span data-stu-id="1c9e1-112">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="1c9e1-113">La funcionalidad para recibir WebHooks se describe con más detalle en Recepción de [WebHooks](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="1c9e1-113">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
