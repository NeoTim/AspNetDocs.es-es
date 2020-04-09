---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Guía de la API de ASP.NET SignalR Hubs - Servidor (C-) Microsoft Docs
author: bradygaster
description: Este documento proporciona una introducción a la programación del lado del servidor de la API de ASP.NET SignalR Hubs para SignalR versión 2, con ejemplos de código que demuestran...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c681b104b15bfc4a04587c7abf685dcf20def2ca
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675806"
---
# <a name="aspnet-signalr-hubs-api-guide---server-c"></a><span data-ttu-id="71a74-103">Guía de la API de ASP.NET SignalR Hubs - Servidor (C-)</span><span class="sxs-lookup"><span data-stu-id="71a74-103">ASP.NET SignalR Hubs API Guide - Server (C#)</span></span>

<span data-ttu-id="71a74-104">por [Patrick Fletcher](https://github.com/pfletcher), Tom [Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="71a74-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="71a74-105">Este documento proporciona una introducción a la programación del lado del servidor de la API de concentradores de ASP.NET SignalR para SignalR versión 2, con ejemplos de código que demuestran opciones comunes.</span><span class="sxs-lookup"><span data-stu-id="71a74-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 2, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="71a74-106">La API de SignalR Hubs le permite realizar llamadas a procedimientos remotos (RPC) desde un servidor a clientes conectados y desde clientes al servidor.</span><span class="sxs-lookup"><span data-stu-id="71a74-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="71a74-107">En el código de servidor, se definen métodos a los que pueden llamar los clientes y se llama a métodos que se ejecutan en el cliente.</span><span class="sxs-lookup"><span data-stu-id="71a74-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="71a74-108">En el código de cliente, se definen métodos a los que se puede llamar desde el servidor y se llama a métodos que se ejecutan en el servidor.</span><span class="sxs-lookup"><span data-stu-id="71a74-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="71a74-109">SignalR se encarga de todas las tuberías de cliente a servidor por usted.</span><span class="sxs-lookup"><span data-stu-id="71a74-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="71a74-110">SignalR también ofrece una API de nivel inferior denominada Conexiones persistentes.</span><span class="sxs-lookup"><span data-stu-id="71a74-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="71a74-111">Para obtener una introducción a SignalR, concentradores y conexiones persistentes, vea [Introducción a SignalR 2](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="71a74-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR 2](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="71a74-112">Versiones de software utilizadas en este tema</span><span class="sxs-lookup"><span data-stu-id="71a74-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="71a74-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="71a74-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="71a74-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="71a74-114">.NET 4.5</span></span>
> - <span data-ttu-id="71a74-115">SignalR versión 2</span><span class="sxs-lookup"><span data-stu-id="71a74-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="topic-versions"></a><span data-ttu-id="71a74-116">Versiones temáticas</span><span class="sxs-lookup"><span data-stu-id="71a74-116">Topic versions</span></span>
> 
> <span data-ttu-id="71a74-117">Para obtener información acerca de las versiones anteriores de SignalR, vea [Versiones anteriores](../older-versions/index.md)de SignalR .</span><span class="sxs-lookup"><span data-stu-id="71a74-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="71a74-118">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="71a74-118">Questions and comments</span></span>
> 
> <span data-ttu-id="71a74-119">Por favor, deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="71a74-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="71a74-120">Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el [ASP.NET foro](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) de SignalR o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="71a74-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="71a74-121">Información general</span><span class="sxs-lookup"><span data-stu-id="71a74-121">Overview</span></span>

<span data-ttu-id="71a74-122">Este documento contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="71a74-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="71a74-123">Cómo registrar el middleware de SignalR</span><span class="sxs-lookup"><span data-stu-id="71a74-123">How to register SignalR middleware</span></span>](#route)

    - [<span data-ttu-id="71a74-124">La URL /signalr</span><span class="sxs-lookup"><span data-stu-id="71a74-124">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="71a74-125">Configuración de las opciones de SignalR</span><span class="sxs-lookup"><span data-stu-id="71a74-125">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="71a74-126">Cómo crear y usar clases de Hub</span><span class="sxs-lookup"><span data-stu-id="71a74-126">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="71a74-127">Duración del objeto de concentrador</span><span class="sxs-lookup"><span data-stu-id="71a74-127">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="71a74-128">Camel-casing de nombres hub en clientes JavaScript</span><span class="sxs-lookup"><span data-stu-id="71a74-128">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="71a74-129">Múltiples hubs</span><span class="sxs-lookup"><span data-stu-id="71a74-129">Multiple Hubs</span></span>](#multiplehubs)
    - [<span data-ttu-id="71a74-130">Concentradores fuertemente tipados</span><span class="sxs-lookup"><span data-stu-id="71a74-130">Strongly-Typed Hubs</span></span>](#stronglytypedhubs)
- [<span data-ttu-id="71a74-131">Cómo definir métodos en la clase Hub a los que los clientes pueden llamar</span><span class="sxs-lookup"><span data-stu-id="71a74-131">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="71a74-132">Camel-casing de nombres de métodos en clientes JavaScript</span><span class="sxs-lookup"><span data-stu-id="71a74-132">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="71a74-133">Cuándo ejecutar de forma asincrónica</span><span class="sxs-lookup"><span data-stu-id="71a74-133">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="71a74-134">Definición de sobrecargas</span><span class="sxs-lookup"><span data-stu-id="71a74-134">Defining overloads</span></span>](#overloads)
    - [<span data-ttu-id="71a74-135">Informes del progreso de las invocaciones de métodos de concentrador</span><span class="sxs-lookup"><span data-stu-id="71a74-135">Reporting progress from hub method invocations</span></span>](#progress)
- [<span data-ttu-id="71a74-136">Cómo llamar a métodos de cliente desde la clase Hub</span><span class="sxs-lookup"><span data-stu-id="71a74-136">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="71a74-137">Selección de qué clientes recibirán el RPC</span><span class="sxs-lookup"><span data-stu-id="71a74-137">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="71a74-138">No hay validación en tiempo de compilación para nombres de método</span><span class="sxs-lookup"><span data-stu-id="71a74-138">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="71a74-139">Coincidencia de nombre de método que no distingue mayúsculas de minúsculas</span><span class="sxs-lookup"><span data-stu-id="71a74-139">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="71a74-140">Ejecución asincrónica</span><span class="sxs-lookup"><span data-stu-id="71a74-140">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="71a74-141">Cómo administrar la pertenencia a grupos desde la clase Hub</span><span class="sxs-lookup"><span data-stu-id="71a74-141">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="71a74-142">Ejecución asincrónica de métodos Add y Remove</span><span class="sxs-lookup"><span data-stu-id="71a74-142">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="71a74-143">Persistencia de la pertenencia al grupo</span><span class="sxs-lookup"><span data-stu-id="71a74-143">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="71a74-144">Grupos de un solo usuario</span><span class="sxs-lookup"><span data-stu-id="71a74-144">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="71a74-145">Cómo controlar los eventos de duración de la conexión en la clase Hub</span><span class="sxs-lookup"><span data-stu-id="71a74-145">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="71a74-146">Cuando OnConnected, OnDisconnected y OnReconnected se llaman</span><span class="sxs-lookup"><span data-stu-id="71a74-146">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="71a74-147">Estado de llamada no poblado</span><span class="sxs-lookup"><span data-stu-id="71a74-147">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="71a74-148">Cómo obtener información sobre el cliente desde la propiedad Context</span><span class="sxs-lookup"><span data-stu-id="71a74-148">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="71a74-149">Cómo pasar el estado entre los clientes y la clase Hub</span><span class="sxs-lookup"><span data-stu-id="71a74-149">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="71a74-150">Cómo controlar los errores en la clase Hub</span><span class="sxs-lookup"><span data-stu-id="71a74-150">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="71a74-151">Cómo llamar a métodos de cliente y administrar grupos desde fuera de la clase Hub</span><span class="sxs-lookup"><span data-stu-id="71a74-151">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="71a74-152">Llamar a métodos de cliente</span><span class="sxs-lookup"><span data-stu-id="71a74-152">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="71a74-153">Gestión de la pertenencia a grupos</span><span class="sxs-lookup"><span data-stu-id="71a74-153">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="71a74-154">Cómo habilitar el seguimiento</span><span class="sxs-lookup"><span data-stu-id="71a74-154">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="71a74-155">Cómo personalizar la canalización de Hubs</span><span class="sxs-lookup"><span data-stu-id="71a74-155">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="71a74-156">Para obtener documentación sobre cómo programar clientes, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="71a74-156">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="71a74-157">SignalR Hubs API Guide - Cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="71a74-157">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)
- [<span data-ttu-id="71a74-158">SignalR Hubs API Guide - .NET Client</span><span class="sxs-lookup"><span data-stu-id="71a74-158">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="71a74-159">Los componentes del servidor para SignalR 2 solo están disponibles en .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="71a74-159">The server components for SignalR 2 are only available in .NET 4.5.</span></span> <span data-ttu-id="71a74-160">Los servidores que ejecutan .NET 4.0 deben usar SignalR v1.x.</span><span class="sxs-lookup"><span data-stu-id="71a74-160">Servers running .NET 4.0 must use SignalR v1.x.</span></span>

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a><span data-ttu-id="71a74-161">Cómo registrar el middleware de SignalR</span><span class="sxs-lookup"><span data-stu-id="71a74-161">How to register SignalR middleware</span></span>

<span data-ttu-id="71a74-162">Para definir la ruta que los clientes usarán para conectarse al concentrador, llame al `MapSignalR` método cuando se inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="71a74-162">To define the route that clients will use to connect to your Hub, call the `MapSignalR` method when the application starts.</span></span> <span data-ttu-id="71a74-163">`MapSignalR`es un [método](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) `OwinExtensions` de extensión para la clase.</span><span class="sxs-lookup"><span data-stu-id="71a74-163">`MapSignalR` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `OwinExtensions` class.</span></span> <span data-ttu-id="71a74-164">En el ejemplo siguiente se muestra cómo definir la ruta de SignalR Hubs mediante una clase de inicio OWIN.</span><span class="sxs-lookup"><span data-stu-id="71a74-164">The following example shows how to define the SignalR Hubs route using an OWIN startup class.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

<span data-ttu-id="71a74-165">Si va a agregar la funcionalidad de SignalR a una aplicación MVC ASP.NET, asegúrese de que la ruta de SignalR se agrega antes que las otras rutas.</span><span class="sxs-lookup"><span data-stu-id="71a74-165">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="71a74-166">Para obtener más información, vea [Tutorial: Introducción a SignalR 2 y MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="71a74-166">For more information, see [Tutorial: Getting Started with SignalR 2 and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="71a74-167">La URL /signalr</span><span class="sxs-lookup"><span data-stu-id="71a74-167">The /signalr URL</span></span>

<span data-ttu-id="71a74-168">De forma predeterminada, la dirección URL de ruta que los clientes utilizarán para conectarse a su concentrador es "/signalr".</span><span class="sxs-lookup"><span data-stu-id="71a74-168">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="71a74-169">(No confunda esta URL con la DIRECCIÓN URL "/signalr/hubs", que es para el archivo JavaScript generado automáticamente.</span><span class="sxs-lookup"><span data-stu-id="71a74-169">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="71a74-170">Para obtener más información sobre el proxy generado, consulte [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).)</span><span class="sxs-lookup"><span data-stu-id="71a74-170">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).)</span></span>

<span data-ttu-id="71a74-171">Puede haber circunstancias extraordinarias que hagan que esta dirección URL base no se pueda usar para SignalR; por ejemplo, tiene una carpeta en el proyecto denominada *signalr* y no desea cambiar el nombre.</span><span class="sxs-lookup"><span data-stu-id="71a74-171">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="71a74-172">En ese caso, puede cambiar la dirección URL base, como se muestra en los ejemplos siguientes (reemplazar "/signalr" en el código de ejemplo con la dirección URL deseada).</span><span class="sxs-lookup"><span data-stu-id="71a74-172">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="71a74-173">**Código de servidor que especifica la dirección URL**</span><span class="sxs-lookup"><span data-stu-id="71a74-173">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

<span data-ttu-id="71a74-174">**Código de cliente JavaScript que especifica la dirección URL (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="71a74-174">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

<span data-ttu-id="71a74-175">**Código de cliente JavaScript que especifica la dirección URL (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="71a74-175">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="71a74-176">**Código de cliente .NET que especifica la dirección URL**</span><span class="sxs-lookup"><span data-stu-id="71a74-176">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="71a74-177">Configuración de las opciones de SignalR</span><span class="sxs-lookup"><span data-stu-id="71a74-177">Configuring SignalR Options</span></span>

<span data-ttu-id="71a74-178">Las sobrecargas `MapSignalR` del método le permiten especificar una dirección URL personalizada, un solucionador de dependencias personalizado y las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="71a74-178">Overloads of the `MapSignalR` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="71a74-179">Habilite las llamadas entre dominios mediante CORS o JSONP desde clientes de explorador.</span><span class="sxs-lookup"><span data-stu-id="71a74-179">Enable cross-domain calls using CORS or JSONP from browser clients.</span></span>

    <span data-ttu-id="71a74-180">Normalmente, si el explorador `http://contoso.com`carga una página desde , la `http://contoso.com/signalr`conexión de SignalR está en el mismo dominio, en .</span><span class="sxs-lookup"><span data-stu-id="71a74-180">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="71a74-181">Si la `http://contoso.com` página de `http://fabrikam.com/signalr`realiza una conexión a , es decir, una conexión entre dominios.</span><span class="sxs-lookup"><span data-stu-id="71a74-181">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="71a74-182">Por motivos de seguridad, las conexiones entre dominios están deshabilitadas de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="71a74-182">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="71a74-183">Para obtener más información, consulte [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="71a74-183">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](hubs-api-guide-javascript-client.md#crossdomain).</span></span>
- <span data-ttu-id="71a74-184">Habilite los mensajes de error detallados.</span><span class="sxs-lookup"><span data-stu-id="71a74-184">Enable detailed error messages.</span></span>

    <span data-ttu-id="71a74-185">Cuando se producen errores, el comportamiento predeterminado de SignalR es enviar a los clientes un mensaje de notificación sin detalles sobre lo que sucedió.</span><span class="sxs-lookup"><span data-stu-id="71a74-185">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="71a74-186">No se recomienda enviar información de error detallada a los clientes en producción, ya que los usuarios malintencionados pueden usar la información en ataques contra la aplicación.</span><span class="sxs-lookup"><span data-stu-id="71a74-186">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="71a74-187">Para solucionar problemas, puede usar esta opción para habilitar temporalmente informes de errores más informativos.</span><span class="sxs-lookup"><span data-stu-id="71a74-187">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="71a74-188">Deshabilite los archivos proxy JavaScript generados automáticamente.</span><span class="sxs-lookup"><span data-stu-id="71a74-188">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="71a74-189">De forma predeterminada, se genera un archivo JavaScript con servidores proxy para las clases hub en respuesta a la dirección URL "/signalr/hubs".</span><span class="sxs-lookup"><span data-stu-id="71a74-189">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="71a74-190">Si no desea utilizar los servidores proxy de JavaScript, o si desea generar este archivo manualmente y hacer referencia a un archivo físico en sus clientes, puede usar esta opción para deshabilitar la generación de proxy.</span><span class="sxs-lookup"><span data-stu-id="71a74-190">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="71a74-191">Para obtener más información, consulte [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](hubs-api-guide-javascript-client.md#manualproxy).</span><span class="sxs-lookup"><span data-stu-id="71a74-191">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](hubs-api-guide-javascript-client.md#manualproxy).</span></span>

<span data-ttu-id="71a74-192">En el ejemplo siguiente se muestra cómo especificar la dirección `MapSignalR` URL de conexión de SignalR y estas opciones en una llamada al método.</span><span class="sxs-lookup"><span data-stu-id="71a74-192">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapSignalR` method.</span></span> <span data-ttu-id="71a74-193">Para especificar una dirección URL personalizada, reemplace "/signalr" en el ejemplo por la dirección URL que desea utilizar.</span><span class="sxs-lookup"><span data-stu-id="71a74-193">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="71a74-194">Cómo crear y usar clases de Hub</span><span class="sxs-lookup"><span data-stu-id="71a74-194">How to create and use Hub classes</span></span>

<span data-ttu-id="71a74-195">Para crear un concentrador, cree una clase que derive de [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="71a74-195">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="71a74-196">En el ejemplo siguiente se muestra una clase Hub simple para una aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="71a74-196">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

<span data-ttu-id="71a74-197">En este ejemplo, un cliente `NewContosoChatMessage` conectado puede llamar al método y, cuando lo hace, los datos recibidos se transmiten a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="71a74-197">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="71a74-198">Duración del objeto de concentrador</span><span class="sxs-lookup"><span data-stu-id="71a74-198">Hub object lifetime</span></span>

<span data-ttu-id="71a74-199">No se crea una instancia de la clase Hub ni se llama a sus métodos desde su propio código en el servidor; todo lo que se hace por usted por la canalización de SignalR Hubs.</span><span class="sxs-lookup"><span data-stu-id="71a74-199">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="71a74-200">SignalR crea una nueva instancia de la clase Hub cada vez que necesita controlar una operación de concentrador, como cuando un cliente se conecta, se desconecta o realiza una llamada de método al servidor.</span><span class="sxs-lookup"><span data-stu-id="71a74-200">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="71a74-201">Dado que las instancias de la clase Hub son transitorias, no puede usarlas para mantener el estado de una llamada de método a la siguiente.</span><span class="sxs-lookup"><span data-stu-id="71a74-201">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="71a74-202">Cada vez que el servidor recibe una llamada de método de un cliente, una nueva instancia de la clase Hub procesa el mensaje.</span><span class="sxs-lookup"><span data-stu-id="71a74-202">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="71a74-203">Para mantener el estado a través de varias conexiones y llamadas a métodos, utilice algún otro `Hub`método, como una base de datos, o una variable estática en la clase Hub o una clase diferente que no se derive de .</span><span class="sxs-lookup"><span data-stu-id="71a74-203">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="71a74-204">Si conserva los datos en la memoria, mediante un método como una variable estática en la clase Hub, los datos se perderán cuando el dominio de aplicación se recicle.</span><span class="sxs-lookup"><span data-stu-id="71a74-204">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="71a74-205">Si desea enviar mensajes a los clientes desde su propio código que se ejecuta fuera de la clase Hub, no puede hacerlo creando una instancia de clase Hub, pero puede hacerlo obteniendo una referencia al objeto de contexto SignalR para la clase Hub.</span><span class="sxs-lookup"><span data-stu-id="71a74-205">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="71a74-206">Para obtener más información, vea [Cómo llamar a métodos](#callfromoutsidehub) de cliente y administrar grupos desde fuera de la clase Hub más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="71a74-206">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="71a74-207">Camel-casing de nombres hub en clientes JavaScript</span><span class="sxs-lookup"><span data-stu-id="71a74-207">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="71a74-208">De forma predeterminada, los clientes de JavaScript hacen referencia a Hubs mediante una versión con mayúsculas y minúsculas del nombre de clase.</span><span class="sxs-lookup"><span data-stu-id="71a74-208">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="71a74-209">SignalR realiza automáticamente este cambio para que el código JavaScript pueda cumplir las convenciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="71a74-209">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="71a74-210">El ejemplo anterior se `contosoChatHub` denominaría en código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="71a74-210">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="71a74-211">**Server**</span><span class="sxs-lookup"><span data-stu-id="71a74-211">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

<span data-ttu-id="71a74-212">**Cliente JavaScript mediante proxy generado**</span><span class="sxs-lookup"><span data-stu-id="71a74-212">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

<span data-ttu-id="71a74-213">Si desea especificar un nombre diferente para que `HubName` los clientes lo usen, agregue el atributo.</span><span class="sxs-lookup"><span data-stu-id="71a74-213">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="71a74-214">Cuando se `HubName` utiliza un atributo, no hay ningún cambio de nombre en el caso de camello en los clientes JavaScript.</span><span class="sxs-lookup"><span data-stu-id="71a74-214">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="71a74-215">**Server**</span><span class="sxs-lookup"><span data-stu-id="71a74-215">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

<span data-ttu-id="71a74-216">**Cliente JavaScript mediante proxy generado**</span><span class="sxs-lookup"><span data-stu-id="71a74-216">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="71a74-217">Múltiples hubs</span><span class="sxs-lookup"><span data-stu-id="71a74-217">Multiple Hubs</span></span>

<span data-ttu-id="71a74-218">Puede definir varias clases de concentrador en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="71a74-218">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="71a74-219">Al hacerlo, la conexión se comparte, pero los grupos son independientes:</span><span class="sxs-lookup"><span data-stu-id="71a74-219">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="71a74-220">Todos los clientes usarán la misma dirección URL para establecer una conexión de SignalR con el servicio ("/signalr" o la dirección URL personalizada si especificó una), y esa conexión se usa para todos los concentradores definidos por el servicio.</span><span class="sxs-lookup"><span data-stu-id="71a74-220">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="71a74-221">No hay ninguna diferencia de rendimiento para varios concentradores en comparación con la definición de toda la funcionalidad de concentrador en una sola clase.</span><span class="sxs-lookup"><span data-stu-id="71a74-221">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="71a74-222">Todos los concentradores obtienen la misma información de solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="71a74-222">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="71a74-223">Puesto que todos los concentradores comparten la misma conexión, la única información de solicitud HTTP que obtiene el servidor es lo que viene en la solicitud HTTP original que establece la conexión SignalR.</span><span class="sxs-lookup"><span data-stu-id="71a74-223">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="71a74-224">Si usa la solicitud de conexión para pasar información del cliente al servidor especificando una cadena de consulta, no puede proporcionar cadenas de consulta diferentes a diferentes concentradores.</span><span class="sxs-lookup"><span data-stu-id="71a74-224">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="71a74-225">Todos los Hubs recibirán la misma información.</span><span class="sxs-lookup"><span data-stu-id="71a74-225">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="71a74-226">El archivo de proxies de JavaScript generado contendrá proxies para todos los concentradores en un archivo.</span><span class="sxs-lookup"><span data-stu-id="71a74-226">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="71a74-227">Para obtener información acerca de los servidores proxy de JavaScript, consulte [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).</span><span class="sxs-lookup"><span data-stu-id="71a74-227">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).</span></span>
- <span data-ttu-id="71a74-228">Los grupos se definen en Los concentradores.</span><span class="sxs-lookup"><span data-stu-id="71a74-228">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="71a74-229">En SignalR puede definir grupos con nombre para difundir a subconjuntos de clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="71a74-229">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="71a74-230">Los grupos se mantienen por separado para cada concentrador.</span><span class="sxs-lookup"><span data-stu-id="71a74-230">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="71a74-231">Por ejemplo, un grupo denominado "Administradores" incluiría un conjunto de clientes para la `ContosoChatHub` clase `StockTickerHub` y el mismo nombre de grupo haría referencia a un conjunto diferente de clientes para la clase.</span><span class="sxs-lookup"><span data-stu-id="71a74-231">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a><span data-ttu-id="71a74-232">Concentradores fuertemente tipados</span><span class="sxs-lookup"><span data-stu-id="71a74-232">Strongly-Typed Hubs</span></span>

<span data-ttu-id="71a74-233">Para definir una interfaz para los métodos de concentrador a los que el `Hub<T>` cliente puede hacer referencia (y `Hub`habilitar Intellisense en los métodos de concentrador), derive el concentrador de (introducido en SignalR 2.1) en lugar de:</span><span class="sxs-lookup"><span data-stu-id="71a74-233">To define an interface for your hub methods that your client can reference (and enable Intellisense on your hub methods), derive your hub from `Hub<T>` (introduced in SignalR 2.1) rather than `Hub`:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="71a74-234">Cómo definir métodos en la clase Hub a los que los clientes pueden llamar</span><span class="sxs-lookup"><span data-stu-id="71a74-234">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="71a74-235">Para exponer un método en el concentrador al que desea que se pueda llamar desde el cliente, declare un método público, como se muestra en los ejemplos siguientes.</span><span class="sxs-lookup"><span data-stu-id="71a74-235">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="71a74-236">Puede especificar un tipo de valor devuelto y parámetros, incluidos los tipos complejos y matrices, como lo haría en cualquier método de C.</span><span class="sxs-lookup"><span data-stu-id="71a74-236">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="71a74-237">Los datos que recibe en parámetros o vuelve al autor de la llamada se comunican entre el cliente y el servidor mediante JSON y SignalR controla automáticamente el enlace de objetos complejos y matrices de objetos.</span><span class="sxs-lookup"><span data-stu-id="71a74-237">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="71a74-238">Camel-casing de nombres de métodos en clientes JavaScript</span><span class="sxs-lookup"><span data-stu-id="71a74-238">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="71a74-239">De forma predeterminada, los clientes de JavaScript hacen referencia a los métodos Hub mediante una versión con mayúsculas y minúsculas del nombre del método.</span><span class="sxs-lookup"><span data-stu-id="71a74-239">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="71a74-240">SignalR realiza automáticamente este cambio para que el código JavaScript pueda cumplir las convenciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="71a74-240">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="71a74-241">**Server**</span><span class="sxs-lookup"><span data-stu-id="71a74-241">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="71a74-242">**Cliente JavaScript mediante proxy generado**</span><span class="sxs-lookup"><span data-stu-id="71a74-242">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="71a74-243">Si desea especificar un nombre diferente para que `HubMethodName` los clientes lo usen, agregue el atributo.</span><span class="sxs-lookup"><span data-stu-id="71a74-243">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="71a74-244">**Server**</span><span class="sxs-lookup"><span data-stu-id="71a74-244">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="71a74-245">**Cliente JavaScript mediante proxy generado**</span><span class="sxs-lookup"><span data-stu-id="71a74-245">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="71a74-246">Cuándo ejecutar de forma asincrónica</span><span class="sxs-lookup"><span data-stu-id="71a74-246">When to execute asynchronously</span></span>

<span data-ttu-id="71a74-247">Si el método se ejecuta durante mucho tiempo o tiene que realizar un trabajo que implique esperar, como [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) una búsqueda de `void` base de datos o una llamada de servicio web, haga que el método Hub sea asincrónico devolviendo un objeto Task (en lugar de return) o [Task&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) (en lugar del `T` tipo de valor devuelto).</span><span class="sxs-lookup"><span data-stu-id="71a74-247">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="71a74-248">Cuando devuelve `Task` un objeto del método, SignalR `Task` espera a que se complete y, a continuación, envía el resultado desencapsulado al cliente, por lo que no hay ninguna diferencia en cómo codificar la llamada al método en el cliente.</span><span class="sxs-lookup"><span data-stu-id="71a74-248">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="71a74-249">Hacer un método Hub asincrónico evita bloquear la conexión cuando usa el transporte WebSocket.</span><span class="sxs-lookup"><span data-stu-id="71a74-249">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="71a74-250">Cuando un método Hub se ejecuta sincrónicamente y el transporte es WebSocket, las invocaciones posteriores de métodos en el concentrador desde el mismo cliente se bloquean hasta que se completa el método Hub.</span><span class="sxs-lookup"><span data-stu-id="71a74-250">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="71a74-251">En el ejemplo siguiente se muestra el mismo método codificado para ejecutarse de forma sincrónica o asincrónica, seguido de código de cliente JavaScript que funciona para llamar a cualquiera de las versiones.</span><span class="sxs-lookup"><span data-stu-id="71a74-251">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="71a74-252">**Sincrónica**</span><span class="sxs-lookup"><span data-stu-id="71a74-252">**Synchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="71a74-253">**Asincrónica**</span><span class="sxs-lookup"><span data-stu-id="71a74-253">**Asynchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="71a74-254">**Cliente JavaScript mediante proxy generado**</span><span class="sxs-lookup"><span data-stu-id="71a74-254">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="71a74-255">Para obtener más información acerca de cómo usar métodos asincrónicos en ASP.NET 4.5, vea Uso de [métodos asincrónicos en ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="71a74-255">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="71a74-256">Definición de sobrecargas</span><span class="sxs-lookup"><span data-stu-id="71a74-256">Defining Overloads</span></span>

<span data-ttu-id="71a74-257">Si desea definir sobrecargas para un método, el número de parámetros de cada sobrecarga debe ser diferente.</span><span class="sxs-lookup"><span data-stu-id="71a74-257">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="71a74-258">Si diferencia una sobrecarga simplemente especificando diferentes tipos de parámetros, la clase Hub se compilará, pero el servicio SignalR producirá una excepción en tiempo de ejecución cuando los clientes intenten llamar a una de las sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="71a74-258">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a><span data-ttu-id="71a74-259">Informes del progreso de las invocaciones de métodos de concentrador</span><span class="sxs-lookup"><span data-stu-id="71a74-259">Reporting progress from hub method invocations</span></span>

<span data-ttu-id="71a74-260">SignalR 2.1 agrega compatibilidad con el patrón de [informes](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) de progreso introducido en .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="71a74-260">SignalR 2.1 adds support for the [progress reporting pattern](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduced in .NET 4.5.</span></span> <span data-ttu-id="71a74-261">Para implementar informes `IProgress<T>` de progreso, defina un parámetro para el método de concentrador al que el cliente pueda tener acceso:</span><span class="sxs-lookup"><span data-stu-id="71a74-261">To implement progress reporting, define an `IProgress<T>` parameter for your hub method that your client can access:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

<span data-ttu-id="71a74-262">Al escribir un método de servidor de ejecución prolongada, es importante usar un patrón de programación asincrónica como Async/ Await en lugar de bloquear el subproceso de concentrador.</span><span class="sxs-lookup"><span data-stu-id="71a74-262">When writing a long-running server method, it is important to use an asynchronous programming pattern like Async/ Await rather than blocking the hub thread.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="71a74-263">Cómo llamar a métodos de cliente desde la clase Hub</span><span class="sxs-lookup"><span data-stu-id="71a74-263">How to call client methods from the Hub class</span></span>

<span data-ttu-id="71a74-264">Para llamar a métodos de `Clients` cliente desde el servidor, use la propiedad en un método de la clase Hub.</span><span class="sxs-lookup"><span data-stu-id="71a74-264">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="71a74-265">En el ejemplo siguiente `addNewMessageToPage` se muestra el código de servidor que llama a todos los clientes conectados y el código de cliente que define el método en un cliente JavaScript.</span><span class="sxs-lookup"><span data-stu-id="71a74-265">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="71a74-266">**Server**</span><span class="sxs-lookup"><span data-stu-id="71a74-266">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

<span data-ttu-id="71a74-267">Invocar un método de cliente es `Task`una operación asincrónica y devuelve un archivo .</span><span class="sxs-lookup"><span data-stu-id="71a74-267">Invoking a client method is an asynchronous operation and returns a `Task`.</span></span> <span data-ttu-id="71a74-268">Use `await`:</span><span class="sxs-lookup"><span data-stu-id="71a74-268">Use `await`:</span></span>

* <span data-ttu-id="71a74-269">Para asegurarse de que el mensaje se envía sin errores.</span><span class="sxs-lookup"><span data-stu-id="71a74-269">To ensure the message is sent without error.</span></span> 
* <span data-ttu-id="71a74-270">Para habilitar la detección y el control de errores en un bloque try-catch.</span><span class="sxs-lookup"><span data-stu-id="71a74-270">To enable catching and handling errors in a try-catch block.</span></span>

<span data-ttu-id="71a74-271">**Cliente JavaScript mediante proxy generado**</span><span class="sxs-lookup"><span data-stu-id="71a74-271">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

<span data-ttu-id="71a74-272">No se puede obtener un valor devuelto de un método de cliente; sintaxis `int x = Clients.All.add(1,1)` como no funciona.</span><span class="sxs-lookup"><span data-stu-id="71a74-272">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="71a74-273">Puede especificar tipos y matrices complejos para los parámetros.</span><span class="sxs-lookup"><span data-stu-id="71a74-273">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="71a74-274">En el ejemplo siguiente se pasa un tipo complejo al cliente en un parámetro de método.</span><span class="sxs-lookup"><span data-stu-id="71a74-274">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="71a74-275">**Código de servidor que llama a un método cliente mediante un objeto complejo**</span><span class="sxs-lookup"><span data-stu-id="71a74-275">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

<span data-ttu-id="71a74-276">**Código de servidor que define el objeto complejo**</span><span class="sxs-lookup"><span data-stu-id="71a74-276">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

<span data-ttu-id="71a74-277">**Cliente JavaScript mediante proxy generado**</span><span class="sxs-lookup"><span data-stu-id="71a74-277">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="71a74-278">Selección de qué clientes recibirán el RPC</span><span class="sxs-lookup"><span data-stu-id="71a74-278">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="71a74-279">La propiedad Clients devuelve un objeto [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) que proporciona varias opciones para especificar qué clientes recibirán el RPC:</span><span class="sxs-lookup"><span data-stu-id="71a74-279">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="71a74-280">Todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="71a74-280">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="71a74-281">Sólo el cliente que llama.</span><span class="sxs-lookup"><span data-stu-id="71a74-281">Only the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="71a74-282">Todos los clientes excepto el cliente que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="71a74-282">All clients except the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- <span data-ttu-id="71a74-283">Un cliente específico identificado por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="71a74-283">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    <span data-ttu-id="71a74-284">Este ejemplo `addContosoChatMessageToPage` llama al cliente que realiza `Clients.Caller`la llamada y tiene el mismo efecto que usar .</span><span class="sxs-lookup"><span data-stu-id="71a74-284">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="71a74-285">Todos los clientes conectados excepto los clientes especificados, identificados por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="71a74-285">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- <span data-ttu-id="71a74-286">Todos los clientes conectados en un grupo especificado.</span><span class="sxs-lookup"><span data-stu-id="71a74-286">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- <span data-ttu-id="71a74-287">Todos los clientes conectados en un grupo especificado excepto los clientes especificados, identificados por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="71a74-287">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- <span data-ttu-id="71a74-288">Todos los clientes conectados en un grupo especificado excepto el cliente que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="71a74-288">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- <span data-ttu-id="71a74-289">Un usuario específico, identificado por userId.</span><span class="sxs-lookup"><span data-stu-id="71a74-289">A specific user, identified by userId.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    <span data-ttu-id="71a74-290">De forma predeterminada, se `IPrincipal.Identity.Name`trata de , pero esto se puede cambiar [registrando una implementación de IUserIdProvider con el host global.](mapping-users-to-connections.md#IUserIdProvider)</span><span class="sxs-lookup"><span data-stu-id="71a74-290">By default, this is `IPrincipal.Identity.Name`, but this can be changed by [registering an implementation of IUserIdProvider with the global host](mapping-users-to-connections.md#IUserIdProvider).</span></span>
- <span data-ttu-id="71a74-291">Todos los clientes y grupos de una lista de iDE de conexión.</span><span class="sxs-lookup"><span data-stu-id="71a74-291">All clients and groups in a list of connection IDs.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- <span data-ttu-id="71a74-292">Una lista de grupos.</span><span class="sxs-lookup"><span data-stu-id="71a74-292">A list of groups.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- <span data-ttu-id="71a74-293">Un usuario por nombre.</span><span class="sxs-lookup"><span data-stu-id="71a74-293">A user by name.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- <span data-ttu-id="71a74-294">Una lista de nombres de usuario (introducidos en SignalR 2.1).</span><span class="sxs-lookup"><span data-stu-id="71a74-294">A list of user names (introduced in SignalR 2.1).</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="71a74-295">No hay validación en tiempo de compilación para nombres de método</span><span class="sxs-lookup"><span data-stu-id="71a74-295">No compile-time validation for method names</span></span>

<span data-ttu-id="71a74-296">El nombre del método que especifique se interpreta como un objeto dinámico, lo que significa que no hay ninguna validación de IntelliSense o en tiempo de compilación para él.</span><span class="sxs-lookup"><span data-stu-id="71a74-296">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="71a74-297">La expresión se evalúa en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="71a74-297">The expression is evaluated at run time.</span></span> <span data-ttu-id="71a74-298">Cuando se ejecuta la llamada al método, SignalR envía el nombre del método y los valores de parámetro al cliente, y si el cliente tiene un método que coincide con el nombre, se llama a ese método y se le pasan los valores de parámetro.</span><span class="sxs-lookup"><span data-stu-id="71a74-298">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="71a74-299">Si no se encuentra ningún método coincidente en el cliente, no se genera ningún error.</span><span class="sxs-lookup"><span data-stu-id="71a74-299">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="71a74-300">Para obtener información sobre el formato de los datos que SignalR transmite al cliente en segundo plano cuando se llama a un método de cliente, vea [Introducción a SignalR](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="71a74-300">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="71a74-301">Coincidencia de nombre de método que no distingue mayúsculas de minúsculas</span><span class="sxs-lookup"><span data-stu-id="71a74-301">Case-insensitive method name matching</span></span>

<span data-ttu-id="71a74-302">La coincidencia de nombres de método no distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="71a74-302">Method name matching is case-insensitive.</span></span> <span data-ttu-id="71a74-303">Por `Clients.All.addContosoChatMessageToPage` ejemplo, en el `AddContosoChatMessageToPage` `addcontosochatmessagetopage`servidor `addContosoChatMessageToPage` se ejecutará , , o en el cliente.</span><span class="sxs-lookup"><span data-stu-id="71a74-303">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="71a74-304">Ejecución asincrónica</span><span class="sxs-lookup"><span data-stu-id="71a74-304">Asynchronous execution</span></span>

<span data-ttu-id="71a74-305">El método al que se llama se ejecuta de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="71a74-305">The method that you call executes asynchronously.</span></span> <span data-ttu-id="71a74-306">Cualquier código que viene después de una llamada de método a un cliente se ejecutará inmediatamente sin esperar a que SignalR termine de transmitir datos a los clientes a menos que especifique que las líneas de código subsiguientes deben esperar a que se complete el método.</span><span class="sxs-lookup"><span data-stu-id="71a74-306">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="71a74-307">En el ejemplo de código siguiente se muestra cómo ejecutar dos métodos de cliente secuencialmente.</span><span class="sxs-lookup"><span data-stu-id="71a74-307">The following code example shows how to execute two client methods sequentially.</span></span>

<span data-ttu-id="71a74-308">**Uso de Await (.NET 4.5)**</span><span class="sxs-lookup"><span data-stu-id="71a74-308">**Using Await (.NET 4.5)**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="71a74-309">Si usa `await` esperar hasta que finalice un método de cliente antes de que se ejecute la siguiente línea de código, eso no significa que los clientes recibirán realmente el mensaje antes de que se ejecute la siguiente línea de código.</span><span class="sxs-lookup"><span data-stu-id="71a74-309">If you use `await` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="71a74-310">"Completar" de una llamada al método de cliente solo significa que SignalR ha hecho todo lo necesario para enviar el mensaje.</span><span class="sxs-lookup"><span data-stu-id="71a74-310">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="71a74-311">Si necesita verificar que los clientes recibieron el mensaje, debe programar ese mecanismo usted mismo.</span><span class="sxs-lookup"><span data-stu-id="71a74-311">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="71a74-312">Por ejemplo, podría `MessageReceived` codificar un método en `addContosoChatMessageToPage` el concentrador y, `MessageReceived` en el método en el cliente, podría llamar después de realizar el trabajo que necesite hacer en el cliente.</span><span class="sxs-lookup"><span data-stu-id="71a74-312">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="71a74-313">En `MessageReceived` el concentrador puede hacer cualquier trabajo depende de la recepción del cliente real y el procesamiento de la llamada al método original.</span><span class="sxs-lookup"><span data-stu-id="71a74-313">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="71a74-314">Cómo utilizar una variable de cadena como nombre del método</span><span class="sxs-lookup"><span data-stu-id="71a74-314">How to use a string variable as the method name</span></span>

<span data-ttu-id="71a74-315">Si desea invocar un método de cliente mediante una variable `Clients.All` de `Clients.Others` `Clients.Caller`cadena como nombre `IClientProxy` de método, convertir (o , , etc.) a y, a continuación, llame a [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="71a74-315">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="71a74-316">Cómo administrar la pertenencia a grupos desde la clase Hub</span><span class="sxs-lookup"><span data-stu-id="71a74-316">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="71a74-317">Los grupos de SignalR proporcionan un método para transmitir mensajes a subconjuntos especificados de clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="71a74-317">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="71a74-318">Un grupo puede tener cualquier número de clientes y un cliente puede ser miembro de cualquier número de grupos.</span><span class="sxs-lookup"><span data-stu-id="71a74-318">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="71a74-319">Para administrar la pertenencia a grupos, use `Groups` los métodos [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) y [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) proporcionados por la propiedad de la clase Hub.</span><span class="sxs-lookup"><span data-stu-id="71a74-319">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="71a74-320">En el ejemplo `Groups.Add` `Groups.Remove` siguiente se muestran los métodos y utilizados en los métodos De concentrador a los que llama el código de cliente, seguido del código de cliente JavaScript que los llama.</span><span class="sxs-lookup"><span data-stu-id="71a74-320">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="71a74-321">**Server**</span><span class="sxs-lookup"><span data-stu-id="71a74-321">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

<span data-ttu-id="71a74-322">**Cliente JavaScript mediante proxy generado**</span><span class="sxs-lookup"><span data-stu-id="71a74-322">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

<span data-ttu-id="71a74-323">No es posible crear grupos explícitamente.</span><span class="sxs-lookup"><span data-stu-id="71a74-323">You don't have to explicitly create groups.</span></span> <span data-ttu-id="71a74-324">En efecto, un grupo se crea automáticamente la primera `Groups.Add`vez que se especifica su nombre en una llamada a , y se elimina cuando se quita la última conexión de la pertenencia a él.</span><span class="sxs-lookup"><span data-stu-id="71a74-324">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="71a74-325">No hay ninguna API para obtener una lista de pertenencia a grupos o una lista de grupos.</span><span class="sxs-lookup"><span data-stu-id="71a74-325">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="71a74-326">SignalR envía mensajes a clientes y grupos basados en un [modelo pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe)y el servidor no mantiene listas de grupos o pertenencias a grupos.</span><span class="sxs-lookup"><span data-stu-id="71a74-326">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="71a74-327">Esto ayuda a maximizar la escalabilidad, porque cada vez que se agrega un nodo a una granja de servidores web, cualquier estado que SignalR mantiene tiene que propagarse al nuevo nodo.</span><span class="sxs-lookup"><span data-stu-id="71a74-327">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="71a74-328">Ejecución asincrónica de métodos Add y Remove</span><span class="sxs-lookup"><span data-stu-id="71a74-328">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="71a74-329">Los `Groups.Add` `Groups.Remove` métodos y se ejecutan de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="71a74-329">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="71a74-330">Si desea agregar un cliente a un grupo y enviar inmediatamente un mensaje al cliente `Groups.Add` mediante el grupo, debe asegurarse de que el método finaliza primero.</span><span class="sxs-lookup"><span data-stu-id="71a74-330">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="71a74-331">En el ejemplo de código siguiente se muestra cómo hacerlo.</span><span class="sxs-lookup"><span data-stu-id="71a74-331">The following code example shows how to do that.</span></span>

<span data-ttu-id="71a74-332">**Agregar un cliente a un grupo y, a continuación, enviar mensajes a ese cliente**</span><span class="sxs-lookup"><span data-stu-id="71a74-332">**Adding a client to a group and then messaging that client**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="71a74-333">Persistencia de la pertenencia al grupo</span><span class="sxs-lookup"><span data-stu-id="71a74-333">Group membership persistence</span></span>

<span data-ttu-id="71a74-334">SignalR realiza un seguimiento de las conexiones, no de los usuarios, por lo que `Groups.Add` si desea que un usuario esté en el mismo grupo cada vez que el usuario establece una conexión, debe llamar cada vez que el usuario establece una nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="71a74-334">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="71a74-335">Después de una pérdida temporal de conectividad, a veces SignalR puede restaurar la conexión automáticamente.</span><span class="sxs-lookup"><span data-stu-id="71a74-335">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="71a74-336">En ese caso, SignalR está restaurando la misma conexión, no estableciendo una nueva conexión, por lo que la pertenencia al grupo del cliente se restaura automáticamente.</span><span class="sxs-lookup"><span data-stu-id="71a74-336">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="71a74-337">Esto es posible incluso cuando el salto temporal es el resultado de un reinicio o error del servidor, porque el estado de conexión para cada cliente, incluidas las pertenencias a grupos, se realiza de ida y vuelta al cliente.</span><span class="sxs-lookup"><span data-stu-id="71a74-337">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="71a74-338">Si un servidor deja de ser y es reemplazado por un nuevo servidor antes de que se agota el tiempo de espera de la conexión, un cliente puede volver a conectarse automáticamente al nuevo servidor y volver a inscribirse en grupos de los que es miembro.</span><span class="sxs-lookup"><span data-stu-id="71a74-338">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="71a74-339">Cuando una conexión no se puede restaurar automáticamente después de una pérdida de conectividad, o cuando se agota el tiempo de espera de la conexión, o cuando el cliente se desconecta (por ejemplo, cuando un explorador navega a una página nueva), se pierden las pertenencias a grupos.</span><span class="sxs-lookup"><span data-stu-id="71a74-339">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="71a74-340">La próxima vez que el usuario se conecte será una nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="71a74-340">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="71a74-341">Para mantener las pertenencias a grupos cuando el mismo usuario establece una nueva conexión, la aplicación tiene que realizar un seguimiento de las asociaciones entre usuarios y grupos y restaurar las pertenencias a grupos cada vez que un usuario establece una nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="71a74-341">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="71a74-342">Para obtener más información acerca de las conexiones y las reconexiones, vea Cómo controlar los eventos de duración de [la conexión en la clase Hub](#connectionlifetime) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="71a74-342">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="71a74-343">Grupos de un solo usuario</span><span class="sxs-lookup"><span data-stu-id="71a74-343">Single-user groups</span></span>

<span data-ttu-id="71a74-344">Las aplicaciones que utilizan SignalR normalmente tienen que realizar un seguimiento de las asociaciones entre los usuarios y las conexiones para saber qué usuario ha enviado un mensaje y qué usuario(s) deben recibir un mensaje.</span><span class="sxs-lookup"><span data-stu-id="71a74-344">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="71a74-345">Los grupos se utilizan en uno de los dos patrones comúnmente utilizados para hacerlo.</span><span class="sxs-lookup"><span data-stu-id="71a74-345">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="71a74-346">Grupos de un solo usuario.</span><span class="sxs-lookup"><span data-stu-id="71a74-346">Single-user groups.</span></span>

    <span data-ttu-id="71a74-347">Puede especificar el nombre de usuario como nombre de grupo y agregar el identificador de conexión actual al grupo cada vez que el usuario se conecte o vuelva a conectarse.</span><span class="sxs-lookup"><span data-stu-id="71a74-347">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="71a74-348">Para enviar mensajes al usuario que envía al grupo.</span><span class="sxs-lookup"><span data-stu-id="71a74-348">To send messages to the user you send to the group.</span></span> <span data-ttu-id="71a74-349">Una desventaja de este método es que el grupo no le proporciona una manera de averiguar si el usuario está en línea o sin conexión.</span><span class="sxs-lookup"><span data-stu-id="71a74-349">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="71a74-350">Realice un seguimiento de las asociaciones entre los nombres de usuario y los ID de conexión.</span><span class="sxs-lookup"><span data-stu-id="71a74-350">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="71a74-351">Puede almacenar una asociación entre cada nombre de usuario y uno o varios ID de conexión en un diccionario o base de datos, y actualizar los datos almacenados cada vez que el usuario se conecta o se desconecta.</span><span class="sxs-lookup"><span data-stu-id="71a74-351">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="71a74-352">Para enviar mensajes al usuario, especifique los identificadores de conexión.</span><span class="sxs-lookup"><span data-stu-id="71a74-352">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="71a74-353">Una desventaja de este método es que se necesita más memoria.</span><span class="sxs-lookup"><span data-stu-id="71a74-353">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="71a74-354">Cómo controlar los eventos de duración de la conexión en la clase Hub</span><span class="sxs-lookup"><span data-stu-id="71a74-354">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="71a74-355">Las razones típicas para controlar los eventos de duración de la conexión son realizar un seguimiento de si un usuario está conectado o no y realizar un seguimiento de la asociación entre los nombres de usuario y los ID de conexión.</span><span class="sxs-lookup"><span data-stu-id="71a74-355">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="71a74-356">Para ejecutar su propio código cuando los `OnConnected` `OnDisconnected`clientes `OnReconnected` se conectan o desconectan, reemplace los métodos , y virtuales de la clase Hub, como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="71a74-356">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="71a74-357">Cuando OnConnected, OnDisconnected y OnReconnected se llaman</span><span class="sxs-lookup"><span data-stu-id="71a74-357">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="71a74-358">Cada vez que un explorador navega a una nueva página, se debe establecer `OnDisconnected` una nueva `OnConnected` conexión, lo que significa que SignalR ejecutará el método seguido por el método.</span><span class="sxs-lookup"><span data-stu-id="71a74-358">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="71a74-359">SignalR siempre crea un nuevo identificador de conexión cuando se establece una nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="71a74-359">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="71a74-360">Se `OnReconnected` llama al método cuando se ha producido una interrupción temporal en la conectividad de la que SignalR puede recuperarse automáticamente, como cuando un cable se desconecta temporalmente y se vuelve a conectar antes de que se agota el tiempo de espera de la conexión. Se `OnDisconnected` llama al método cuando se desconecta el cliente y SignalR no se puede volver a conectar automáticamente, como cuando un explorador navega a una nueva página.</span><span class="sxs-lookup"><span data-stu-id="71a74-360">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="71a74-361">Por lo tanto, una posible secuencia `OnConnected` `OnReconnected`de `OnDisconnected`eventos para un cliente determinado es , , ; o `OnConnected` `OnDisconnected`, .</span><span class="sxs-lookup"><span data-stu-id="71a74-361">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="71a74-362">No verá la secuencia `OnConnected` `OnDisconnected`, `OnReconnected` , para una conexión determinada.</span><span class="sxs-lookup"><span data-stu-id="71a74-362">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="71a74-363">No `OnDisconnected` se llama al método en algunos escenarios, como cuando un servidor deja de ser o el dominio de aplicación se recicla.</span><span class="sxs-lookup"><span data-stu-id="71a74-363">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="71a74-364">Cuando otro servidor entra en línea o el dominio de la aplicación completa `OnReconnected` su reciclaje, algunos clientes pueden volver a conectarse e desencadenar el evento.</span><span class="sxs-lookup"><span data-stu-id="71a74-364">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="71a74-365">Para obtener más información, consulte Descripción y control de eventos de duración de [conexión en SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="71a74-365">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="71a74-366">Estado de llamada no poblado</span><span class="sxs-lookup"><span data-stu-id="71a74-366">Caller state not populated</span></span>

<span data-ttu-id="71a74-367">Los métodos de controlador de eventos de duración de la conexión `state` se llaman desde el servidor, `Caller` lo que significa que cualquier estado que coloque en el objeto en el cliente no se rellenará en la propiedad del servidor.</span><span class="sxs-lookup"><span data-stu-id="71a74-367">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="71a74-368">Para obtener `state` información sobre `Caller` el objeto y la propiedad, vea Cómo pasar el [estado entre los clientes y la clase Hub](#passstate) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="71a74-368">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="71a74-369">Cómo obtener información sobre el cliente desde la propiedad Context</span><span class="sxs-lookup"><span data-stu-id="71a74-369">How to get information about the client from the Context property</span></span>

<span data-ttu-id="71a74-370">Para obtener información sobre el `Context` cliente, use la propiedad de la clase Hub.</span><span class="sxs-lookup"><span data-stu-id="71a74-370">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="71a74-371">La `Context` propiedad devuelve un objeto [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) que proporciona acceso a la siguiente información:</span><span class="sxs-lookup"><span data-stu-id="71a74-371">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="71a74-372">El identificador de conexión del cliente que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="71a74-372">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    <span data-ttu-id="71a74-373">El identificador de conexión es un GUID asignado por SignalR (no se puede especificar el valor en su propio código).</span><span class="sxs-lookup"><span data-stu-id="71a74-373">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="71a74-374">Hay un identificador de conexión para cada conexión y todos los concentradores usan el mismo identificador de conexión si tiene varios concentradores en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="71a74-374">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="71a74-375">Datos de encabezado HTTP.</span><span class="sxs-lookup"><span data-stu-id="71a74-375">HTTP header data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="71a74-376">También puede obtener encabezados `Context.Headers`HTTP desde .</span><span class="sxs-lookup"><span data-stu-id="71a74-376">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="71a74-377">La razón de varias referencias a `Context.Headers` la misma `Context.Request` cosa es que `Context.Headers` se creó primero, la propiedad se agregó más adelante y se retuvo por compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="71a74-377">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="71a74-378">Datos de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="71a74-378">Query string data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    <span data-ttu-id="71a74-379">También puede obtener datos `Context.QueryString`de cadena de consulta de .</span><span class="sxs-lookup"><span data-stu-id="71a74-379">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="71a74-380">La cadena de consulta que se obtiene en esta propiedad es la que se usó con la solicitud HTTP que estableció la conexión De SignalR.</span><span class="sxs-lookup"><span data-stu-id="71a74-380">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="71a74-381">Puede agregar parámetros de cadena de consulta en el cliente configurando la conexión, que es una manera conveniente de pasar datos sobre el cliente desde el cliente al servidor.</span><span class="sxs-lookup"><span data-stu-id="71a74-381">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="71a74-382">En el ejemplo siguiente se muestra una manera de agregar una cadena de consulta en un cliente JavaScript cuando se usa el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="71a74-382">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    <span data-ttu-id="71a74-383">Para obtener más información acerca de cómo establecer parámetros de cadena de consulta, consulte las guías de API para los clientes [de JavaScript](hubs-api-guide-javascript-client.md) y [.NET.](hubs-api-guide-net-client.md)</span><span class="sxs-lookup"><span data-stu-id="71a74-383">For more information about setting query string parameters, see the API guides for the [JavaScript](hubs-api-guide-javascript-client.md) and [.NET](hubs-api-guide-net-client.md) clients.</span></span>

    <span data-ttu-id="71a74-384">Puede encontrar el método de transporte utilizado para la conexión en los datos de cadena de consulta, junto con algunos otros valores utilizados internamente por SignalR:</span><span class="sxs-lookup"><span data-stu-id="71a74-384">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    <span data-ttu-id="71a74-385">El valor `transportMethod` de será "webSockets", "serverSentEvents", "foreverFrame" o "longPolling".</span><span class="sxs-lookup"><span data-stu-id="71a74-385">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="71a74-386">Tenga en cuenta que si `OnConnected` comprueba este valor en el método de controlador de eventos, en algunos escenarios puede obtener inicialmente un valor de transporte que no es el método de transporte negociado final para la conexión.</span><span class="sxs-lookup"><span data-stu-id="71a74-386">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="71a74-387">En ese caso, el método producirá una excepción y se llamará de nuevo más adelante cuando se establezca el método de transporte final.</span><span class="sxs-lookup"><span data-stu-id="71a74-387">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="71a74-388">Galletas.</span><span class="sxs-lookup"><span data-stu-id="71a74-388">Cookies.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    <span data-ttu-id="71a74-389">También puede obtener `Context.RequestCookies`cookies de .</span><span class="sxs-lookup"><span data-stu-id="71a74-389">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="71a74-390">información del usuario.</span><span class="sxs-lookup"><span data-stu-id="71a74-390">User information.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- <span data-ttu-id="71a74-391">El objeto HttpContext para la solicitud:</span><span class="sxs-lookup"><span data-stu-id="71a74-391">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    <span data-ttu-id="71a74-392">Utilice este método `HttpContext.Current` en lugar `HttpContext` de obtener el objeto para la conexión De SignalR.</span><span class="sxs-lookup"><span data-stu-id="71a74-392">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="71a74-393">Cómo pasar el estado entre los clientes y la clase Hub</span><span class="sxs-lookup"><span data-stu-id="71a74-393">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="71a74-394">El proxy de `state` cliente proporciona un objeto en el que puede almacenar los datos que desea que se transmitan al servidor con cada llamada al método.</span><span class="sxs-lookup"><span data-stu-id="71a74-394">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="71a74-395">En el servidor puede tener `Clients.Caller` acceso a estos datos en la propiedad en los métodos de concentrador a los que llaman los clientes.</span><span class="sxs-lookup"><span data-stu-id="71a74-395">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="71a74-396">La `Clients.Caller` propiedad no se rellena para los `OnConnected` `OnDisconnected`métodos `OnReconnected`de controlador de eventos de duración de conexión , , y .</span><span class="sxs-lookup"><span data-stu-id="71a74-396">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="71a74-397">La creación o `state` actualización de `Clients.Caller` datos en el objeto y la propiedad funciona en ambas direcciones.</span><span class="sxs-lookup"><span data-stu-id="71a74-397">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="71a74-398">Puede actualizar los valores en el servidor y se devuelven al cliente.</span><span class="sxs-lookup"><span data-stu-id="71a74-398">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="71a74-399">En el ejemplo siguiente se muestra el código de cliente de JavaScript que almacena el estado para la transmisión al servidor con cada llamada al método.</span><span class="sxs-lookup"><span data-stu-id="71a74-399">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

<span data-ttu-id="71a74-400">En el ejemplo siguiente se muestra el código equivalente en un cliente .NET.</span><span class="sxs-lookup"><span data-stu-id="71a74-400">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

<span data-ttu-id="71a74-401">En la clase Hub, puede tener `Clients.Caller` acceso a estos datos en la propiedad.</span><span class="sxs-lookup"><span data-stu-id="71a74-401">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="71a74-402">En el ejemplo siguiente se muestra código que recupera el estado al que se hace referencia en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="71a74-402">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="71a74-403">Este mecanismo para conservar el estado no está pensado para grandes `state` cantidades de datos, ya que todo lo que se coloca en la propiedad o `Clients.Caller` se realiza de ida y vuelta con cada invocación de método.</span><span class="sxs-lookup"><span data-stu-id="71a74-403">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="71a74-404">Es útil para elementos más pequeños, como nombres de usuario o contadores.</span><span class="sxs-lookup"><span data-stu-id="71a74-404">It's useful for smaller items such as user names or counters.</span></span>

<span data-ttu-id="71a74-405">En VB.NET o en un concentrador fuertemente tipado, no `Clients.Caller`se puede tener acceso al objeto de estado del llamador a través de ; en su `Clients.CallerState` lugar, el uso (introducido en SignalR 2.1):</span><span class="sxs-lookup"><span data-stu-id="71a74-405">In VB.NET or in a strongly-typed hub, the caller state object can't be accessed through `Clients.Caller`; instead, use `Clients.CallerState` (introduced in SignalR 2.1):</span></span>

<span data-ttu-id="71a74-406">**Uso de CallerState en C #**</span><span class="sxs-lookup"><span data-stu-id="71a74-406">**Using CallerState in C#**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

<span data-ttu-id="71a74-407">**Uso de CallerState en Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="71a74-407">**Using CallerState in Visual Basic**</span></span>

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="71a74-408">Cómo controlar los errores en la clase Hub</span><span class="sxs-lookup"><span data-stu-id="71a74-408">How to handle errors in the Hub class</span></span>

<span data-ttu-id="71a74-409">Para controlar los errores que se producen en los métodos de la clase Hub, primero asegúrese de "observar" cualquier excepción de las operaciones asincrónicas (como invocar métodos de cliente) mediante `await`.</span><span class="sxs-lookup"><span data-stu-id="71a74-409">To handle errors that occur in your Hub class methods, first ensure you "observe" any exceptions from async operations (such as invoking client methods) by using `await`.</span></span> <span data-ttu-id="71a74-410">A continuación, utilice uno o varios de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="71a74-410">Then use one or more of the following methods:</span></span>

- <span data-ttu-id="71a74-411">Ajuste el código del método en bloques try-catch y registre el objeto de excepción.</span><span class="sxs-lookup"><span data-stu-id="71a74-411">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="71a74-412">Para fines de depuración, puede enviar la excepción al cliente, pero por motivos de seguridad no se recomienda enviar información detallada a los clientes en producción.</span><span class="sxs-lookup"><span data-stu-id="71a74-412">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="71a74-413">Cree un módulo de canalización de Hubs que controle el método [OnIncomingError.](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="71a74-413">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="71a74-414">En el ejemplo siguiente se muestra un módulo de canalización que registra errores, seguido de código en Startup.cs que inserta el módulo en la canalización de concentradores.</span><span class="sxs-lookup"><span data-stu-id="71a74-414">The following example shows a pipeline module that logs errors, followed by code in Startup.cs that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- <span data-ttu-id="71a74-415">Utilice `HubException` la clase (introducida en SignalR 2).</span><span class="sxs-lookup"><span data-stu-id="71a74-415">Use the `HubException` class (introduced in SignalR 2).</span></span> <span data-ttu-id="71a74-416">Este error se puede producir desde cualquier invocación de concentrador.</span><span class="sxs-lookup"><span data-stu-id="71a74-416">This error can be thrown from any hub invocation.</span></span> <span data-ttu-id="71a74-417">El `HubError` constructor toma un mensaje de cadena y un objeto para almacenar datos de error adicionales.</span><span class="sxs-lookup"><span data-stu-id="71a74-417">The `HubError` constructor takes a string message, and an object for storing extra error data.</span></span> <span data-ttu-id="71a74-418">SignalR serializará automáticamente la excepción y la enviará al cliente, donde se usará para rechazar o producir un error en la invocación del método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="71a74-418">SignalR will auto-serialize the exception and send it to the client, where it will be used to reject or fail the hub method invocation.</span></span>

    <span data-ttu-id="71a74-419">En los ejemplos de `HubException` código siguientes se muestra cómo iniciar una invocación de concentrador durante una invocación de concentrador y cómo controlar la excepción en clientes JavaScript y .NET.</span><span class="sxs-lookup"><span data-stu-id="71a74-419">The following code samples demonstrate how to throw a `HubException` during a Hub invocation, and how to handle the exception on JavaScript and .NET clients.</span></span>

    <span data-ttu-id="71a74-420">**Código de servidor que muestra la clase HubException**</span><span class="sxs-lookup"><span data-stu-id="71a74-420">**Server code demonstrating the HubException class**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    <span data-ttu-id="71a74-421">**Código de cliente JavaScript que muestra la respuesta a la generación de una excepción HubException en un concentrador**</span><span class="sxs-lookup"><span data-stu-id="71a74-421">**JavaScript client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    <span data-ttu-id="71a74-422">**Código de cliente .NET que muestra la respuesta a la generación de una excepción HubException en un concentrador**</span><span class="sxs-lookup"><span data-stu-id="71a74-422">**.NET client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

<span data-ttu-id="71a74-423">Para obtener más información acerca de los módulos de canalización de concentrador, vea [cómo personalizar la canalización](#hubpipeline) de concentradores más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="71a74-423">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="71a74-424">Cómo habilitar el seguimiento</span><span class="sxs-lookup"><span data-stu-id="71a74-424">How to enable tracing</span></span>

<span data-ttu-id="71a74-425">Para habilitar el seguimiento del lado del servidor, agregue un elemento system.diagnostics al archivo Web.config, como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="71a74-425">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

<span data-ttu-id="71a74-426">Al ejecutar la aplicación en Visual Studio, puede ver los registros en **la** salida ventana.</span><span class="sxs-lookup"><span data-stu-id="71a74-426">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="71a74-427">Cómo llamar a métodos de cliente y administrar grupos desde fuera de la clase Hub</span><span class="sxs-lookup"><span data-stu-id="71a74-427">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="71a74-428">Para llamar a métodos de cliente desde una clase diferente de la clase Hub, obtenga una referencia al objeto de contexto SignalR para el concentrador y úselo para llamar a métodos en el cliente o administrar grupos.</span><span class="sxs-lookup"><span data-stu-id="71a74-428">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="71a74-429">La siguiente `StockTicker` clase de ejemplo obtiene el objeto de contexto, lo almacena en una instancia de la clase, almacena la `updateStockPrice` instancia de clase en una `StockTickerHub`propiedad estática y utiliza el contexto de la instancia de clase singleton para llamar al método en clientes que están conectados a un concentrador denominado .</span><span class="sxs-lookup"><span data-stu-id="71a74-429">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

<span data-ttu-id="71a74-430">Si necesita usar el contexto varias veces en un objeto de larga duración, obtenga la referencia una vez y guárdela en lugar de volver a obtenerla cada vez.</span><span class="sxs-lookup"><span data-stu-id="71a74-430">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="71a74-431">Obtener el contexto una vez garantiza que SignalR envía mensajes a los clientes en la misma secuencia en la que los métodos de concentrador realizan invocaciones de método de cliente.</span><span class="sxs-lookup"><span data-stu-id="71a74-431">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="71a74-432">Para obtener un tutorial que muestra cómo usar el contexto de SignalR para un concentrador, vea Difusión de [servidor con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="71a74-432">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="71a74-433">Llamar a métodos de cliente</span><span class="sxs-lookup"><span data-stu-id="71a74-433">Calling client methods</span></span>

<span data-ttu-id="71a74-434">Puede especificar qué clientes recibirán el RPC, pero tiene menos opciones que cuando llama desde una clase Hub.</span><span class="sxs-lookup"><span data-stu-id="71a74-434">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="71a74-435">La razón de esto es que el contexto no está asociado a una llamada determinada de un `Clients.Others`cliente, por lo que los métodos que requieren conocimiento del identificador de conexión actual, como , o `Clients.Caller`, o `Clients.OthersInGroup`, no están disponibles.</span><span class="sxs-lookup"><span data-stu-id="71a74-435">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="71a74-436">Están disponibles las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="71a74-436">The following options are available:</span></span>

- <span data-ttu-id="71a74-437">Todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="71a74-437">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- <span data-ttu-id="71a74-438">Un cliente específico identificado por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="71a74-438">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- <span data-ttu-id="71a74-439">Todos los clientes conectados excepto los clientes especificados, identificados por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="71a74-439">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- <span data-ttu-id="71a74-440">Todos los clientes conectados en un grupo especificado.</span><span class="sxs-lookup"><span data-stu-id="71a74-440">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- <span data-ttu-id="71a74-441">Todos los clientes conectados en un grupo especificado excepto los clientes especificados, identificados por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="71a74-441">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

<span data-ttu-id="71a74-442">Si llama a la clase que no es de Concentrador desde métodos de la `Clients.Client`clase `Clients.AllExcept`Hub, puede pasar el identificador de conexión actual y usarlo con , , `Clients.Group` o para simular `Clients.Caller`, `Clients.Others`, o `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="71a74-442">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="71a74-443">En el ejemplo `MoveShapeHub` siguiente, la clase pasa `Broadcaster` el identificador `Broadcaster` de `Clients.Others`conexión a la clase para que la clase pueda simular .</span><span class="sxs-lookup"><span data-stu-id="71a74-443">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="71a74-444">Gestión de la pertenencia a grupos</span><span class="sxs-lookup"><span data-stu-id="71a74-444">Managing group membership</span></span>

<span data-ttu-id="71a74-445">Para administrar grupos, tiene las mismas opciones que en una clase Hub.</span><span class="sxs-lookup"><span data-stu-id="71a74-445">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="71a74-446">Agregar un cliente a un grupo</span><span class="sxs-lookup"><span data-stu-id="71a74-446">Add a client to a group</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- <span data-ttu-id="71a74-447">Quitar un cliente de un grupo</span><span class="sxs-lookup"><span data-stu-id="71a74-447">Remove a client from a group</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="71a74-448">Cómo personalizar la canalización de Hubs</span><span class="sxs-lookup"><span data-stu-id="71a74-448">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="71a74-449">SignalR le permite insertar su propio código en la canalización de concentrador.</span><span class="sxs-lookup"><span data-stu-id="71a74-449">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="71a74-450">En el ejemplo siguiente se muestra un módulo de canalización de concentrador personalizado que registra cada llamada al método entrante recibida del cliente y la llamada al método saliente invocada en el cliente:</span><span class="sxs-lookup"><span data-stu-id="71a74-450">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

<span data-ttu-id="71a74-451">El código siguiente en el *archivo Startup.cs* registra el módulo para que se ejecute en la canalización de concentradores:</span><span class="sxs-lookup"><span data-stu-id="71a74-451">The following code in the *Startup.cs* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

<span data-ttu-id="71a74-452">Hay muchos métodos diferentes que puede invalidar.</span><span class="sxs-lookup"><span data-stu-id="71a74-452">There are many different methods that you can override.</span></span> <span data-ttu-id="71a74-453">Para obtener una lista completa, vea [Métodos HubPipelineModule](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="71a74-453">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
