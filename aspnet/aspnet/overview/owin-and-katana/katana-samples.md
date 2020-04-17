---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Muestras de Katana ? Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 15cc1084b16db2619f2295ee21dec4f49eb2e354
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81540446"
---
# <a name="katana-samples"></a><span data-ttu-id="96db0-102">Ejemplos de Katana</span><span class="sxs-lookup"><span data-stu-id="96db0-102">Katana Samples</span></span>

<span data-ttu-id="96db0-103">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="96db0-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="96db0-104">Ejemplos de Katana</span><span class="sxs-lookup"><span data-stu-id="96db0-104">Katana Samples</span></span>

<span data-ttu-id="96db0-105">**ASP.NET código fuente** | [de](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes) muestra de rutas</span><span class="sxs-lookup"><span data-stu-id="96db0-105">**ASP.NET Routes Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span></span>  
<span data-ttu-id="96db0-106">En algunas aplicaciones, querrá conectar componentes OWIN en la tabla de rutas Asp.Net lado a lado con componentes que no sean OWIN.</span><span class="sxs-lookup"><span data-stu-id="96db0-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="96db0-107">En este ejemplo se muestra cómo utilizar los métodos de extensión RouteCollection MapOwinPath y MapOwinRoute proporcionados por Microsoft.Owin.Host.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="96db0-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="96db0-108">**Código** | [fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines) de muestra de tuberías de bifurcación</span><span class="sxs-lookup"><span data-stu-id="96db0-108">**Branching Pipelines Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span></span>  
<span data-ttu-id="96db0-109">Las canalizaciones de procesamiento de solicitudes OWIN no necesitan ser lineales, se pueden bifurcar para procesar solicitudes de diferentes maneras.</span><span class="sxs-lookup"><span data-stu-id="96db0-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="96db0-110">En este ejemplo se muestra cómo construir una canalización de bifurcación basada en rutas de acceso de solicitud u otros datos de solicitud, como encabezados.</span><span class="sxs-lookup"><span data-stu-id="96db0-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="96db0-111">Estos componentes están disponibles en el paquete nuget Microsoft.Owin.Mapping.</span><span class="sxs-lookup"><span data-stu-id="96db0-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="96db0-112">**Custom Server Sample** | [Código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) de ejemplo de servidor personalizado </span><span class="sxs-lookup"><span data-stu-id="96db0-112">**Custom Server Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span></span>  
<span data-ttu-id="96db0-113">Muestra cómo utilizar un servidor OWIN personalizado al hospedar oWIN de forma automática.</span><span class="sxs-lookup"><span data-stu-id="96db0-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="96db0-114">**Embedded Sample** | [Código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded) de muestra integrado</span><span class="sxs-lookup"><span data-stu-id="96db0-114">**Embedded Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span></span>  
<span data-ttu-id="96db0-115">Algunos servidores OWIN se pueden ejecutar&quot;dentro de&quot;su propio proceso (autohospedado).</span><span class="sxs-lookup"><span data-stu-id="96db0-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="96db0-116">En este ejemplo se muestra cómo iniciar una aplicación OWIN mediante las herramientas proporcionadas por el paquete nuget Microsoft.Owin.Hosting.</span><span class="sxs-lookup"><span data-stu-id="96db0-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="96db0-117">**Código** | [fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld) de muestra HelloWorld</span><span class="sxs-lookup"><span data-stu-id="96db0-117">**HelloWorld Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span></span>  
<span data-ttu-id="96db0-118">OWIN es una abstracción de API de servidor HTTP que permite la portabilidad de aplicaciones en varios servidores.</span><span class="sxs-lookup"><span data-stu-id="96db0-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="96db0-119">En este ejemplo se muestra cómo escribir una aplicación Hello World mediante algunos **contenedores simples** alrededor de la abstracción OWIN sin procesar y ejecutarla en un servidor web como ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="96db0-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="96db0-120">**Hello World Raw OWIN Código** | [fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin) de muestra</span><span class="sxs-lookup"><span data-stu-id="96db0-120">**Hello World Raw OWIN Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span></span>  
<span data-ttu-id="96db0-121">En este ejemplo se muestra cómo escribir una aplicación Hello World mediante la abstracción **OWIN sin procesar** y ejecutarla en un servidor web como Asp.Net.</span><span class="sxs-lookup"><span data-stu-id="96db0-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="96db0-122">**SignalR Sample** | [Código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR) de muestra de SignalR</span><span class="sxs-lookup"><span data-stu-id="96db0-122">**SignalR Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span></span>  
<span data-ttu-id="96db0-123">Muestra cómo autohospedar SignalR mediante OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="96db0-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="96db0-124">Para obtener más información sobre el autohospedaje de SignalR, vea [Tutorial: Autohospedaje de SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="96db0-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="96db0-125">**Static Files Sample** | [Código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) de ejemplo de archivos estáticos </span><span class="sxs-lookup"><span data-stu-id="96db0-125">**Static Files Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span></span>  
<span data-ttu-id="96db0-126">Muestra cómo admitir solicitudes HTTP para archivos estáticos mediante OWIN / Katana.</span><span class="sxs-lookup"><span data-stu-id="96db0-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="96db0-127">**Web API** | [Código fuente de](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) la API web </span><span class="sxs-lookup"><span data-stu-id="96db0-127">**Web API** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span></span>  
<span data-ttu-id="96db0-128">En este ejemplo se muestra cómo hospedar OWIN en IIS y agregar api web a la canalización de OWIN.</span><span class="sxs-lookup"><span data-stu-id="96db0-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="96db0-129">**Web Socket Sample** | [Código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) de muestra de Web Socket </span><span class="sxs-lookup"><span data-stu-id="96db0-129">**Web Socket Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span></span>  
<span data-ttu-id="96db0-130">Muestra cómo admitir Web Sockets en OWIN mediante la clase [System.Net.WebSockets.WebSocket.](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="96db0-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
