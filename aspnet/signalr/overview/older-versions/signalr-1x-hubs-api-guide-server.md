---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: Guía de la API de ASP.NET Signalr hubs-servidor (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: En este documento se proporciona una introducción a la programación del lado del servidor de ASP.NET Signalr hubs API para Signalr versión 1,1, con ejemplos de código que se muestran...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: d9cd3fad36c0300d96c6dbdc61291ef119da2327
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468133"
---
# <a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a><span data-ttu-id="c62d3-103">Guía de la API de ASP.NET Signalr hubs-servidor (Signalr 1. x)</span><span class="sxs-lookup"><span data-stu-id="c62d3-103">ASP.NET SignalR Hubs API Guide - Server (SignalR 1.x)</span></span>

<span data-ttu-id="c62d3-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c62d3-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="c62d3-105">En este documento se proporciona una introducción a la programación del lado del servidor de ASP.NET Signalr hubs API para Signalr versión 1,1, con ejemplos de código que muestran opciones comunes.</span><span class="sxs-lookup"><span data-stu-id="c62d3-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 1.1, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="c62d3-106">Signalr hubs API le permite realizar llamadas a procedimiento remoto (RPC) desde un servidor a clientes conectados y desde clientes al servidor.</span><span class="sxs-lookup"><span data-stu-id="c62d3-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="c62d3-107">En el código del servidor, se definen los métodos a los que pueden llamar los clientes y se llama a los métodos que se ejecutan en el cliente.</span><span class="sxs-lookup"><span data-stu-id="c62d3-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="c62d3-108">En el código de cliente, se definen los métodos a los que se puede llamar desde el servidor y se llama a los métodos que se ejecutan en el servidor.</span><span class="sxs-lookup"><span data-stu-id="c62d3-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="c62d3-109">Signalr se encarga de todas las conexiones de cliente a servidor.</span><span class="sxs-lookup"><span data-stu-id="c62d3-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="c62d3-110">Signalr también ofrece una API de nivel inferior denominada conexiones persistentes.</span><span class="sxs-lookup"><span data-stu-id="c62d3-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="c62d3-111">Para obtener una introducción a Signalr, hubs y conexiones persistentes, o para ver un tutorial que muestra cómo crear una aplicación Signalr completa, consulte [signalr-introducción](index.md).</span><span class="sxs-lookup"><span data-stu-id="c62d3-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](index.md).</span></span>

## <a name="overview"></a><span data-ttu-id="c62d3-112">Información general</span><span class="sxs-lookup"><span data-stu-id="c62d3-112">Overview</span></span>

<span data-ttu-id="c62d3-113">Este documento contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="c62d3-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="c62d3-114">Cómo registrar la ruta de Signalr y configurar las opciones de Signalr</span><span class="sxs-lookup"><span data-stu-id="c62d3-114">How to register the SignalR route and configure SignalR options</span></span>](#route)

    - [<span data-ttu-id="c62d3-115">La dirección URL de/signalr</span><span class="sxs-lookup"><span data-stu-id="c62d3-115">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="c62d3-116">Configuración de las opciones de Signalr</span><span class="sxs-lookup"><span data-stu-id="c62d3-116">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="c62d3-117">Cómo crear y usar clases de Hub</span><span class="sxs-lookup"><span data-stu-id="c62d3-117">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="c62d3-118">Duración del objeto de concentrador</span><span class="sxs-lookup"><span data-stu-id="c62d3-118">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="c62d3-119">Grafía Camel de nombres de concentrador en clientes de JavaScript</span><span class="sxs-lookup"><span data-stu-id="c62d3-119">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="c62d3-120">Varios centros</span><span class="sxs-lookup"><span data-stu-id="c62d3-120">Multiple Hubs</span></span>](#multiplehubs)
- [<span data-ttu-id="c62d3-121">Cómo definir métodos en la clase de concentrador a los que los clientes pueden llamar</span><span class="sxs-lookup"><span data-stu-id="c62d3-121">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="c62d3-122">Grafía Camel de nombres de métodos en clientes de JavaScript</span><span class="sxs-lookup"><span data-stu-id="c62d3-122">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="c62d3-123">Cuándo ejecutar de forma asincrónica</span><span class="sxs-lookup"><span data-stu-id="c62d3-123">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="c62d3-124">Definir sobrecargas</span><span class="sxs-lookup"><span data-stu-id="c62d3-124">Defining overloads</span></span>](#overloads)
- [<span data-ttu-id="c62d3-125">Cómo llamar a métodos de cliente desde la clase hub</span><span class="sxs-lookup"><span data-stu-id="c62d3-125">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="c62d3-126">Selección de los clientes que recibirán la RPC</span><span class="sxs-lookup"><span data-stu-id="c62d3-126">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="c62d3-127">No hay validación en tiempo de compilación para los nombres de método</span><span class="sxs-lookup"><span data-stu-id="c62d3-127">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="c62d3-128">Coincidencia de nombres de método que no distinguen mayúsculas de minúsculas</span><span class="sxs-lookup"><span data-stu-id="c62d3-128">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="c62d3-129">Ejecución asincrónica</span><span class="sxs-lookup"><span data-stu-id="c62d3-129">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="c62d3-130">Cómo administrar la pertenencia a grupos desde la clase hub</span><span class="sxs-lookup"><span data-stu-id="c62d3-130">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="c62d3-131">Ejecución asincrónica de métodos Add y Remove</span><span class="sxs-lookup"><span data-stu-id="c62d3-131">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="c62d3-132">Persistencia de pertenencia a grupos</span><span class="sxs-lookup"><span data-stu-id="c62d3-132">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="c62d3-133">Grupos de usuario único</span><span class="sxs-lookup"><span data-stu-id="c62d3-133">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="c62d3-134">Cómo controlar los eventos de duración de la conexión en la clase hub</span><span class="sxs-lookup"><span data-stu-id="c62d3-134">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="c62d3-135">Cuando se llama a alconnected, OnDisconnection y OnReconnected</span><span class="sxs-lookup"><span data-stu-id="c62d3-135">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="c62d3-136">Estado del autor de la llamada no rellenado</span><span class="sxs-lookup"><span data-stu-id="c62d3-136">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="c62d3-137">Cómo obtener información sobre el cliente desde la propiedad de contexto</span><span class="sxs-lookup"><span data-stu-id="c62d3-137">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="c62d3-138">Cómo pasar el estado entre los clientes y la clase de concentrador</span><span class="sxs-lookup"><span data-stu-id="c62d3-138">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="c62d3-139">Cómo controlar errores en la clase hub</span><span class="sxs-lookup"><span data-stu-id="c62d3-139">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="c62d3-140">Cómo llamar a métodos de cliente y administrar grupos desde fuera de la clase hub</span><span class="sxs-lookup"><span data-stu-id="c62d3-140">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="c62d3-141">Llamar a métodos de cliente</span><span class="sxs-lookup"><span data-stu-id="c62d3-141">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="c62d3-142">Administrar la pertenencia a grupos</span><span class="sxs-lookup"><span data-stu-id="c62d3-142">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="c62d3-143">Cómo habilitar el seguimiento</span><span class="sxs-lookup"><span data-stu-id="c62d3-143">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="c62d3-144">Personalización de la canalización de hubs</span><span class="sxs-lookup"><span data-stu-id="c62d3-144">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="c62d3-145">Para obtener documentación sobre cómo programar clientes, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="c62d3-145">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="c62d3-146">Guía de API de signalr hubs: cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="c62d3-146">SignalR Hubs API Guide - JavaScript Client</span></span>](index.md)
- [<span data-ttu-id="c62d3-147">Guía de la API de signalr hubs: cliente .NET</span><span class="sxs-lookup"><span data-stu-id="c62d3-147">SignalR Hubs API Guide - .NET Client</span></span>](index.md)

<span data-ttu-id="c62d3-148">Los vínculos a los temas de referencia de la API se redirigen a la versión .NET 4,5 de la API.</span><span class="sxs-lookup"><span data-stu-id="c62d3-148">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="c62d3-149">Si usa .NET 4, vea [la versión .net 4 de los temas de la API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="c62d3-149">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a><span data-ttu-id="c62d3-150">Cómo registrar la ruta de Signalr y configurar las opciones de Signalr</span><span class="sxs-lookup"><span data-stu-id="c62d3-150">How to register the SignalR route and configure SignalR options</span></span>

<span data-ttu-id="c62d3-151">Para definir la ruta que los clientes usarán para conectarse al centro, llame al método [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) cuando se inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c62d3-151">To define the route that clients will use to connect to your Hub, call the [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="c62d3-152">`MapHubs` es un [método de extensión](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) para la clase `System.Web.Routing.RouteCollection`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-152">`MapHubs` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `System.Web.Routing.RouteCollection` class.</span></span> <span data-ttu-id="c62d3-153">En el ejemplo siguiente se muestra cómo definir la ruta de los concentradores de Signalr en el archivo *global. asax* .</span><span class="sxs-lookup"><span data-stu-id="c62d3-153">The following example shows how to define the SignalR Hubs route in the *Global.asax* file.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

<span data-ttu-id="c62d3-154">Si va a agregar la funcionalidad de Signalr a una aplicación de ASP.NET MVC, asegúrese de que la ruta de Signalr se agrega antes que las demás rutas.</span><span class="sxs-lookup"><span data-stu-id="c62d3-154">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="c62d3-155">Para obtener más información, vea [Tutorial: introducción con signalr y MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="c62d3-155">For more information, see [Tutorial: Getting Started with SignalR and MVC 4](index.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="c62d3-156">La dirección URL de/signalr</span><span class="sxs-lookup"><span data-stu-id="c62d3-156">The /signalr URL</span></span>

<span data-ttu-id="c62d3-157">De forma predeterminada, la dirección URL de ruta que los clientes usarán para conectarse al centro es "/signalr".</span><span class="sxs-lookup"><span data-stu-id="c62d3-157">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="c62d3-158">(No confunda esta dirección URL con la dirección URL "/signalr/hubs", que es para el archivo JavaScript generado automáticamente.</span><span class="sxs-lookup"><span data-stu-id="c62d3-158">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="c62d3-159">Para obtener más información sobre el proxy generado, consulte [Guía de API de signalr hubs: cliente JavaScript: el proxy generado y lo que hace para usted](index.md).</span><span class="sxs-lookup"><span data-stu-id="c62d3-159">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).)</span></span>

<span data-ttu-id="c62d3-160">Puede haber circunstancias extraordinarias que hagan que esta dirección URL base no sea utilizable para Signalr. por ejemplo, si tiene una carpeta en el proyecto denominada *signalr* y no desea cambiar el nombre.</span><span class="sxs-lookup"><span data-stu-id="c62d3-160">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="c62d3-161">En ese caso, puede cambiar la dirección URL base, como se muestra en los ejemplos siguientes (reemplace "/signalr" en el código de ejemplo por la dirección URL que desee).</span><span class="sxs-lookup"><span data-stu-id="c62d3-161">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="c62d3-162">**Código de servidor que especifica la dirección URL**</span><span class="sxs-lookup"><span data-stu-id="c62d3-162">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

<span data-ttu-id="c62d3-163">**Código de cliente de JavaScript que especifica la dirección URL (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="c62d3-163">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="c62d3-164">**Código de cliente de JavaScript que especifica la dirección URL (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="c62d3-164">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

<span data-ttu-id="c62d3-165">**Código de cliente .NET que especifica la dirección URL**</span><span class="sxs-lookup"><span data-stu-id="c62d3-165">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="c62d3-166">Configuración de las opciones de Signalr</span><span class="sxs-lookup"><span data-stu-id="c62d3-166">Configuring SignalR Options</span></span>

<span data-ttu-id="c62d3-167">Las sobrecargas del método `MapHubs` permiten especificar una dirección URL personalizada, una resolución de dependencia personalizada y las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="c62d3-167">Overloads of the `MapHubs` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="c62d3-168">Habilitar llamadas entre dominios desde clientes del explorador.</span><span class="sxs-lookup"><span data-stu-id="c62d3-168">Enable cross-domain calls from browser clients.</span></span>

    <span data-ttu-id="c62d3-169">Normalmente, si el explorador carga una página desde `http://contoso.com`, la conexión de Signalr está en el mismo dominio, en `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-169">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="c62d3-170">Si la página de `http://contoso.com` establece una conexión con `http://fabrikam.com/signalr`, es una conexión entre dominios.</span><span class="sxs-lookup"><span data-stu-id="c62d3-170">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="c62d3-171">Por motivos de seguridad, las conexiones entre dominios están deshabilitadas de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c62d3-171">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="c62d3-172">Para obtener más información, consulte Guía de la [API de ASP.net signalr hubs: cliente JavaScript: Cómo establecer una conexión entre dominios](index.md).</span><span class="sxs-lookup"><span data-stu-id="c62d3-172">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](index.md).</span></span>
- <span data-ttu-id="c62d3-173">Habilitar mensajes de error detallados.</span><span class="sxs-lookup"><span data-stu-id="c62d3-173">Enable detailed error messages.</span></span>

    <span data-ttu-id="c62d3-174">Cuando se producen errores, el comportamiento predeterminado de Signalr es enviar a los clientes un mensaje de notificación sin detalles sobre lo que ha sucedido.</span><span class="sxs-lookup"><span data-stu-id="c62d3-174">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="c62d3-175">No se recomienda enviar información de error detallada a los clientes en producción, ya que es posible que los usuarios malintencionados puedan usar la información en ataques contra la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c62d3-175">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="c62d3-176">Para solucionar problemas, puede usar esta opción para habilitar temporalmente informes de errores más informativos.</span><span class="sxs-lookup"><span data-stu-id="c62d3-176">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="c62d3-177">Deshabilite los archivos de proxy de JavaScript generados automáticamente.</span><span class="sxs-lookup"><span data-stu-id="c62d3-177">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="c62d3-178">De forma predeterminada, se genera un archivo JavaScript con servidores proxy para las clases de centro como respuesta a la dirección URL "/signalr/hubs".</span><span class="sxs-lookup"><span data-stu-id="c62d3-178">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="c62d3-179">Si no desea usar los servidores proxy de JavaScript, o si desea generar este archivo manualmente y hacer referencia a un archivo físico en sus clientes, puede usar esta opción para deshabilitar la generación de proxy.</span><span class="sxs-lookup"><span data-stu-id="c62d3-179">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="c62d3-180">Para obtener más información, consulte [Guía de API de signalr hubs: cliente JavaScript: Cómo crear un archivo físico para el proxy generado por signalr](index.md).</span><span class="sxs-lookup"><span data-stu-id="c62d3-180">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](index.md).</span></span>

<span data-ttu-id="c62d3-181">En el ejemplo siguiente se muestra cómo especificar la dirección URL de conexión de Signalr y estas opciones en una llamada al método `MapHubs`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-181">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapHubs` method.</span></span> <span data-ttu-id="c62d3-182">Para especificar una dirección URL personalizada, reemplace "/signalr" en el ejemplo por la dirección URL que desea usar.</span><span class="sxs-lookup"><span data-stu-id="c62d3-182">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="c62d3-183">Cómo crear y usar clases de Hub</span><span class="sxs-lookup"><span data-stu-id="c62d3-183">How to create and use Hub classes</span></span>

<span data-ttu-id="c62d3-184">Para crear un concentrador, cree una clase que derive de [Microsoft. Aspnet. signalr. Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="c62d3-184">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="c62d3-185">En el ejemplo siguiente se muestra una clase de concentrador simple para una aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="c62d3-185">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

<span data-ttu-id="c62d3-186">En este ejemplo, un cliente conectado puede llamar al método `NewContosoChatMessage` y, cuando lo hace, los datos recibidos se difunden a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="c62d3-186">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="c62d3-187">Duración del objeto de concentrador</span><span class="sxs-lookup"><span data-stu-id="c62d3-187">Hub object lifetime</span></span>

<span data-ttu-id="c62d3-188">No se crea una instancia de la clase hub ni se llama a sus métodos desde su propio código en el servidor; todo lo que hace la canalización de hubs de Signalr.</span><span class="sxs-lookup"><span data-stu-id="c62d3-188">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="c62d3-189">Signalr crea una instancia nueva de la clase hub cada vez que necesita controlar una operación de concentrador, como cuando un cliente se conecta, desconecta o realiza una llamada al método en el servidor.</span><span class="sxs-lookup"><span data-stu-id="c62d3-189">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="c62d3-190">Dado que las instancias de la clase hub son transitorias, no puede usarlas para mantener el estado de una llamada de método a la siguiente.</span><span class="sxs-lookup"><span data-stu-id="c62d3-190">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="c62d3-191">Cada vez que el servidor recibe una llamada al método desde un cliente, una nueva instancia de la clase del centro procesa el mensaje.</span><span class="sxs-lookup"><span data-stu-id="c62d3-191">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="c62d3-192">Para mantener el estado a través de varias conexiones y llamadas a métodos, use algún otro método, como una base de datos, una variable estática en la clase hub o una clase diferente que no se derive de `Hub`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-192">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="c62d3-193">Si conserva los datos en memoria, mediante un método como una variable estática en la clase hub, los datos se perderán cuando se recicle el dominio de aplicación.</span><span class="sxs-lookup"><span data-stu-id="c62d3-193">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="c62d3-194">Si desea enviar mensajes a los clientes desde su propio código que se ejecuta fuera de la clase de concentrador, no puede crear una instancia de la clase de concentrador, pero puede hacerlo obteniendo una referencia al objeto de contexto de Signalr para la clase de centro.</span><span class="sxs-lookup"><span data-stu-id="c62d3-194">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="c62d3-195">Para obtener más información, consulte [llamar a métodos de cliente y administrar grupos desde fuera de la clase de concentrador](#callfromoutsidehub) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="c62d3-195">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="c62d3-196">Grafía Camel de nombres de concentrador en clientes de JavaScript</span><span class="sxs-lookup"><span data-stu-id="c62d3-196">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="c62d3-197">De forma predeterminada, los clientes de JavaScript hacen referencia a los concentradores mediante una versión con grafía Camel del nombre de clase.</span><span class="sxs-lookup"><span data-stu-id="c62d3-197">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="c62d3-198">Signalr realiza este cambio automáticamente para que el código JavaScript pueda ajustarse a las convenciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c62d3-198">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="c62d3-199">El ejemplo anterior se denominaría `contosoChatHub` en el código de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c62d3-199">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="c62d3-200">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="c62d3-200">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

<span data-ttu-id="c62d3-201">**Cliente JavaScript que usa el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="c62d3-201">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

<span data-ttu-id="c62d3-202">Si desea especificar un nombre diferente para que lo usen los clientes, agregue el atributo `HubName`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-202">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="c62d3-203">Cuando se usa un atributo `HubName`, no se cambia el nombre a mayúsculas y minúsculas Camel en los clientes de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c62d3-203">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="c62d3-204">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="c62d3-204">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

<span data-ttu-id="c62d3-205">**Cliente JavaScript que usa el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="c62d3-205">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="c62d3-206">Varios centros</span><span class="sxs-lookup"><span data-stu-id="c62d3-206">Multiple Hubs</span></span>

<span data-ttu-id="c62d3-207">Puede definir varias clases de concentrador en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="c62d3-207">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="c62d3-208">Al hacerlo, la conexión se comparte pero los grupos son independientes:</span><span class="sxs-lookup"><span data-stu-id="c62d3-208">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="c62d3-209">Todos los clientes usarán la misma dirección URL para establecer una conexión Signalr con su servicio ("/signalr" o su dirección URL personalizada si especificó una) y esa conexión se utiliza para todos los concentradores definidos por el servicio.</span><span class="sxs-lookup"><span data-stu-id="c62d3-209">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="c62d3-210">No hay ninguna diferencia de rendimiento para varios concentradores en comparación con la definición de todas las funciones del concentrador en una sola clase.</span><span class="sxs-lookup"><span data-stu-id="c62d3-210">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="c62d3-211">Todos los concentradores obtienen la misma información de solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="c62d3-211">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="c62d3-212">Dado que todos los concentradores comparten la misma conexión, la única información de la solicitud HTTP que obtiene el servidor es lo que se incluye en la solicitud HTTP original que establece la conexión de Signalr.</span><span class="sxs-lookup"><span data-stu-id="c62d3-212">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="c62d3-213">Si usa la solicitud de conexión para pasar información desde el cliente al servidor mediante la especificación de una cadena de consulta, no puede proporcionar cadenas de consulta diferentes a diferentes concentradores.</span><span class="sxs-lookup"><span data-stu-id="c62d3-213">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="c62d3-214">Todos los concentradores recibirán la misma información.</span><span class="sxs-lookup"><span data-stu-id="c62d3-214">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="c62d3-215">El archivo de proxies de JavaScript generado contendrá servidores proxy para todos los concentradores en un archivo.</span><span class="sxs-lookup"><span data-stu-id="c62d3-215">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="c62d3-216">Para obtener información sobre los servidores proxy de JavaScript, consulte [Guía de API de signalr hubs: cliente JavaScript: el proxy generado y lo que hace para usted](index.md).</span><span class="sxs-lookup"><span data-stu-id="c62d3-216">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).</span></span>
- <span data-ttu-id="c62d3-217">Los grupos se definen dentro de los concentradores.</span><span class="sxs-lookup"><span data-stu-id="c62d3-217">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="c62d3-218">En Signalr puede definir grupos con nombre para difundirlos a subconjuntos de clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="c62d3-218">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="c62d3-219">Los grupos se mantienen por separado para cada concentrador.</span><span class="sxs-lookup"><span data-stu-id="c62d3-219">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="c62d3-220">Por ejemplo, un grupo denominado "administradores" incluiría un conjunto de clientes para la clase `ContosoChatHub` y el mismo nombre de grupo haría referencia a un conjunto de clientes diferente para la clase `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-220">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="c62d3-221">Cómo definir métodos en la clase de concentrador a los que los clientes pueden llamar</span><span class="sxs-lookup"><span data-stu-id="c62d3-221">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="c62d3-222">Para exponer un método en el concentrador al que desea llamar desde el cliente, declare un método público, tal como se muestra en los ejemplos siguientes.</span><span class="sxs-lookup"><span data-stu-id="c62d3-222">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="c62d3-223">Puede especificar un tipo de valor devuelto y parámetros, incluidos tipos complejos y matrices, como haría en cualquier C# método.</span><span class="sxs-lookup"><span data-stu-id="c62d3-223">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="c62d3-224">Los datos que se reciben en parámetros o se devuelven al autor de la llamada se comunican entre el cliente y el servidor mediante JSON, y Signalr controla el enlace de objetos complejos y matrices de objetos de forma automática.</span><span class="sxs-lookup"><span data-stu-id="c62d3-224">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="c62d3-225">Grafía Camel de nombres de métodos en clientes de JavaScript</span><span class="sxs-lookup"><span data-stu-id="c62d3-225">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="c62d3-226">De forma predeterminada, los clientes de JavaScript hacen referencia a métodos de concentrador mediante una versión con mayúsculas y minúsculas Camel del nombre del método.</span><span class="sxs-lookup"><span data-stu-id="c62d3-226">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="c62d3-227">Signalr realiza este cambio automáticamente para que el código JavaScript pueda ajustarse a las convenciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c62d3-227">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="c62d3-228">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="c62d3-228">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="c62d3-229">**Cliente JavaScript que usa el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="c62d3-229">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="c62d3-230">Si desea especificar un nombre diferente para que lo usen los clientes, agregue el atributo `HubMethodName`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-230">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="c62d3-231">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="c62d3-231">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="c62d3-232">**Cliente JavaScript que usa el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="c62d3-232">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="c62d3-233">Cuándo ejecutar de forma asincrónica</span><span class="sxs-lookup"><span data-stu-id="c62d3-233">When to execute asynchronously</span></span>

<span data-ttu-id="c62d3-234">Si el método va a ser de ejecución prolongada o tiene que hacer un trabajo que implique la espera, como una búsqueda en la base de datos o una llamada de servicio Web, haga que el método del concentrador sea asincrónico devolviendo una [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (en lugar de `void` Return) o una [tarea&lt;t&gt;](https://msdn.microsoft.com/library/dd321424.aspx) objeto (en lugar de `T` tipo de valor devuelto).</span><span class="sxs-lookup"><span data-stu-id="c62d3-234">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="c62d3-235">Cuando se devuelve un objeto de `Task` desde el método, Signalr espera a que se complete el `Task` y, a continuación, devuelve el resultado desencapsulado al cliente, por lo que no hay ninguna diferencia en la forma de codificar la llamada al método en el cliente.</span><span class="sxs-lookup"><span data-stu-id="c62d3-235">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="c62d3-236">Al convertir un método de concentrador en asincrónico se evita el bloqueo de la conexión cuando se usa el transporte de WebSocket.</span><span class="sxs-lookup"><span data-stu-id="c62d3-236">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="c62d3-237">Cuando un método de concentrador se ejecuta sincrónicamente y el transporte es WebSocket, las siguientes invocaciones de métodos en el centro desde el mismo cliente se bloquean hasta que se completa el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="c62d3-237">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="c62d3-238">En el ejemplo siguiente se muestra el mismo método codificado para ejecutarse de forma sincrónica o asincrónica, seguido del código de cliente de JavaScript que funciona para llamar a cualquier versión.</span><span class="sxs-lookup"><span data-stu-id="c62d3-238">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="c62d3-239">**Sincrónica**</span><span class="sxs-lookup"><span data-stu-id="c62d3-239">**Synchronous**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="c62d3-240">**Asincrónico-ASP.NET 4,5**</span><span class="sxs-lookup"><span data-stu-id="c62d3-240">**Asynchronous - ASP.NET 4.5**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="c62d3-241">**Cliente JavaScript que usa el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="c62d3-241">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="c62d3-242">Para obtener más información sobre cómo usar métodos asincrónicos en ASP.NET 4,5, vea [uso de métodos asincrónicos en ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="c62d3-242">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="c62d3-243">Definir sobrecargas</span><span class="sxs-lookup"><span data-stu-id="c62d3-243">Defining Overloads</span></span>

<span data-ttu-id="c62d3-244">Si desea definir sobrecargas para un método, el número de parámetros de cada sobrecarga debe ser diferente.</span><span class="sxs-lookup"><span data-stu-id="c62d3-244">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="c62d3-245">Si diferencia una sobrecarga simplemente especificando distintos tipos de parámetro, la clase del concentrador se compilará, pero el servicio Signalr producirá una excepción en tiempo de ejecución cuando los clientes intenten llamar a una de las sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="c62d3-245">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="c62d3-246">Cómo llamar a métodos de cliente desde la clase hub</span><span class="sxs-lookup"><span data-stu-id="c62d3-246">How to call client methods from the Hub class</span></span>

<span data-ttu-id="c62d3-247">Para llamar a métodos de cliente desde el servidor, use la propiedad `Clients` en un método de la clase hub.</span><span class="sxs-lookup"><span data-stu-id="c62d3-247">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="c62d3-248">En el ejemplo siguiente se muestra el código de servidor que llama a `addNewMessageToPage` en todos los clientes conectados y el código de cliente que define el método en un cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c62d3-248">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="c62d3-249">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="c62d3-249">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

<span data-ttu-id="c62d3-250">**Cliente JavaScript que usa el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="c62d3-250">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

<span data-ttu-id="c62d3-251">No se puede obtener un valor devuelto de un método de cliente; la sintaxis como `int x = Clients.All.add(1,1)` no funciona.</span><span class="sxs-lookup"><span data-stu-id="c62d3-251">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="c62d3-252">Puede especificar tipos complejos y matrices para los parámetros.</span><span class="sxs-lookup"><span data-stu-id="c62d3-252">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="c62d3-253">En el ejemplo siguiente se pasa un tipo complejo al cliente en un parámetro de método.</span><span class="sxs-lookup"><span data-stu-id="c62d3-253">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="c62d3-254">**Código de servidor que llama a un método de cliente mediante un objeto complejo**</span><span class="sxs-lookup"><span data-stu-id="c62d3-254">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

<span data-ttu-id="c62d3-255">**Código de servidor que define el objeto complejo**</span><span class="sxs-lookup"><span data-stu-id="c62d3-255">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

<span data-ttu-id="c62d3-256">**Cliente JavaScript que usa el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="c62d3-256">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="c62d3-257">Selección de los clientes que recibirán la RPC</span><span class="sxs-lookup"><span data-stu-id="c62d3-257">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="c62d3-258">La propiedad clients devuelve un objeto [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) que proporciona varias opciones para especificar qué clientes recibirán la RPC:</span><span class="sxs-lookup"><span data-stu-id="c62d3-258">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="c62d3-259">Todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="c62d3-259">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- <span data-ttu-id="c62d3-260">Solo el cliente que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="c62d3-260">Only the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="c62d3-261">Todos los clientes, excepto el cliente que llama.</span><span class="sxs-lookup"><span data-stu-id="c62d3-261">All clients except the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="c62d3-262">Un cliente específico identificado por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="c62d3-262">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    <span data-ttu-id="c62d3-263">En este ejemplo se llama a `addContosoChatMessageToPage` en el cliente que llama y tiene el mismo efecto que el uso de `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-263">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="c62d3-264">Todos los clientes conectados, excepto los clientes especificados, identificados por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="c62d3-264">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- <span data-ttu-id="c62d3-265">Todos los clientes conectados en un grupo especificado.</span><span class="sxs-lookup"><span data-stu-id="c62d3-265">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- <span data-ttu-id="c62d3-266">Todos los clientes conectados en un grupo especificado, excepto los clientes especificados, identificados por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="c62d3-266">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- <span data-ttu-id="c62d3-267">Todos los clientes conectados en un grupo especificado, excepto el cliente que llama.</span><span class="sxs-lookup"><span data-stu-id="c62d3-267">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="c62d3-268">No hay validación en tiempo de compilación para los nombres de método</span><span class="sxs-lookup"><span data-stu-id="c62d3-268">No compile-time validation for method names</span></span>

<span data-ttu-id="c62d3-269">El nombre de método que especifique se interpreta como un objeto dinámico, lo que significa que no hay ninguna validación de IntelliSense o de tiempo de compilación para él.</span><span class="sxs-lookup"><span data-stu-id="c62d3-269">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="c62d3-270">La expresión se evalúa en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="c62d3-270">The expression is evaluated at run time.</span></span> <span data-ttu-id="c62d3-271">Cuando se ejecuta la llamada al método, Signalr envía el nombre del método y los valores de parámetro al cliente, y si el cliente tiene un método que coincide con el nombre, se llama a ese método y se le pasan los valores de parámetro.</span><span class="sxs-lookup"><span data-stu-id="c62d3-271">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="c62d3-272">Si no se encuentra ningún método coincidente en el cliente, no se produce ningún error.</span><span class="sxs-lookup"><span data-stu-id="c62d3-272">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="c62d3-273">Para obtener información sobre el formato de los datos que Signalr transmite al cliente en segundo plano cuando se llama a un método de cliente, consulte [Introducción a signalr](index.md).</span><span class="sxs-lookup"><span data-stu-id="c62d3-273">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](index.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="c62d3-274">Coincidencia de nombres de método que no distinguen mayúsculas de minúsculas</span><span class="sxs-lookup"><span data-stu-id="c62d3-274">Case-insensitive method name matching</span></span>

<span data-ttu-id="c62d3-275">La coincidencia de nombres de método no distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="c62d3-275">Method name matching is case-insensitive.</span></span> <span data-ttu-id="c62d3-276">Por ejemplo, `Clients.All.addContosoChatMessageToPage` en el servidor ejecutará `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`o `addContosoChatMessageToPage` en el cliente.</span><span class="sxs-lookup"><span data-stu-id="c62d3-276">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="c62d3-277">Ejecución asincrónica</span><span class="sxs-lookup"><span data-stu-id="c62d3-277">Asynchronous execution</span></span>

<span data-ttu-id="c62d3-278">El método que se llama se ejecuta de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="c62d3-278">The method that you call executes asynchronously.</span></span> <span data-ttu-id="c62d3-279">Cualquier código que venga después de una llamada de método a un cliente se ejecutará inmediatamente sin esperar a que Signalr termine de transmitir datos a los clientes a menos que especifique que las siguientes líneas de código deben esperar a que se complete el método.</span><span class="sxs-lookup"><span data-stu-id="c62d3-279">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="c62d3-280">En los siguientes ejemplos de código se muestra cómo ejecutar dos métodos de cliente secuencialmente, uno con código que funciona en .NET 4,5 y otro con código que funciona en .NET 4.</span><span class="sxs-lookup"><span data-stu-id="c62d3-280">The following code examples show how to execute two client methods sequentially, one using code that works in .NET 4.5, and one using code that works in .NET 4.</span></span>

<span data-ttu-id="c62d3-281">**Ejemplo de .NET 4,5**</span><span class="sxs-lookup"><span data-stu-id="c62d3-281">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

<span data-ttu-id="c62d3-282">**Ejemplo de .NET 4**</span><span class="sxs-lookup"><span data-stu-id="c62d3-282">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

<span data-ttu-id="c62d3-283">Si usa `await` o `ContinueWith` para esperar a que un método de cliente finalice antes de que se ejecute la siguiente línea de código, eso no significa que los clientes reciban realmente el mensaje antes de que se ejecute la siguiente línea de código.</span><span class="sxs-lookup"><span data-stu-id="c62d3-283">If you use `await` or `ContinueWith` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="c62d3-284">La "finalización" de una llamada al método de cliente solo significa que Signalr ha hecho todo lo necesario para enviar el mensaje.</span><span class="sxs-lookup"><span data-stu-id="c62d3-284">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="c62d3-285">Si necesita comprobar que los clientes recibieron el mensaje, debe programar ese mecanismo.</span><span class="sxs-lookup"><span data-stu-id="c62d3-285">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="c62d3-286">Por ejemplo, puede codificar un método de `MessageReceived` en el concentrador y, en el método de `addContosoChatMessageToPage` en el cliente, podría llamar a `MessageReceived` después de hacer todo el trabajo que necesita hacer en el cliente.</span><span class="sxs-lookup"><span data-stu-id="c62d3-286">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="c62d3-287">En `MessageReceived` en el concentrador puede realizar cualquier trabajo que dependa de la recepción y el procesamiento reales del cliente de la llamada al método original.</span><span class="sxs-lookup"><span data-stu-id="c62d3-287">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="c62d3-288">Cómo usar una variable de cadena como nombre del método</span><span class="sxs-lookup"><span data-stu-id="c62d3-288">How to use a string variable as the method name</span></span>

<span data-ttu-id="c62d3-289">Si desea invocar un método de cliente utilizando una variable de cadena como nombre del método, convierta `Clients.All` (o `Clients.Others`, `Clients.Caller`, etc.) a `IClientProxy` y, a continuación, llame a [Invoke (MethodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="c62d3-289">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="c62d3-290">Cómo administrar la pertenencia a grupos desde la clase hub</span><span class="sxs-lookup"><span data-stu-id="c62d3-290">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="c62d3-291">Los grupos de Signalr proporcionan un método para difundir mensajes a subconjuntos específicos de clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="c62d3-291">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="c62d3-292">Un grupo puede tener cualquier número de clientes y un cliente puede ser miembro de cualquier número de grupos.</span><span class="sxs-lookup"><span data-stu-id="c62d3-292">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="c62d3-293">Para administrar la pertenencia a grupos, use los métodos [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) y [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) proporcionados por la propiedad `Groups` de la clase hub.</span><span class="sxs-lookup"><span data-stu-id="c62d3-293">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="c62d3-294">En el ejemplo siguiente se muestran los métodos `Groups.Add` y `Groups.Remove` usados en métodos de concentrador a los que llama el código de cliente, seguidos del código de cliente de JavaScript que los llama.</span><span class="sxs-lookup"><span data-stu-id="c62d3-294">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="c62d3-295">**Servidor**</span><span class="sxs-lookup"><span data-stu-id="c62d3-295">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

<span data-ttu-id="c62d3-296">**Cliente JavaScript que usa el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="c62d3-296">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

<span data-ttu-id="c62d3-297">No tiene que crear grupos explícitamente.</span><span class="sxs-lookup"><span data-stu-id="c62d3-297">You don't have to explicitly create groups.</span></span> <span data-ttu-id="c62d3-298">En efecto, se crea automáticamente un grupo la primera vez que se especifica su nombre en una llamada a `Groups.Add`y se elimina cuando se quita la última conexión de la pertenencia a él.</span><span class="sxs-lookup"><span data-stu-id="c62d3-298">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="c62d3-299">No hay ninguna API para obtener una lista de pertenencia a grupos o una lista de grupos.</span><span class="sxs-lookup"><span data-stu-id="c62d3-299">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="c62d3-300">Signalr envía mensajes a clientes y grupos basados en un [modelo pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), y el servidor no mantiene listas de grupos o pertenencias a grupos.</span><span class="sxs-lookup"><span data-stu-id="c62d3-300">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="c62d3-301">Esto ayuda a maximizar la escalabilidad, ya que cada vez que se agrega un nodo a una granja de servidores Web, cualquier Estado que mantenga Signalr debe propagarse al nuevo nodo.</span><span class="sxs-lookup"><span data-stu-id="c62d3-301">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="c62d3-302">Ejecución asincrónica de métodos Add y Remove</span><span class="sxs-lookup"><span data-stu-id="c62d3-302">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="c62d3-303">Los métodos `Groups.Add` y `Groups.Remove` se ejecutan de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="c62d3-303">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="c62d3-304">Si desea agregar un cliente a un grupo y enviar inmediatamente un mensaje al cliente mediante el grupo, debe asegurarse de que el método `Groups.Add` finaliza primero.</span><span class="sxs-lookup"><span data-stu-id="c62d3-304">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="c62d3-305">En los siguientes ejemplos de código se muestra cómo hacerlo, uno mediante código que funciona en .NET 4,5 y otro con código que funciona en .NET 4.</span><span class="sxs-lookup"><span data-stu-id="c62d3-305">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4</span></span>

<span data-ttu-id="c62d3-306">**Ejemplo de .NET 4,5**</span><span class="sxs-lookup"><span data-stu-id="c62d3-306">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="c62d3-307">**Ejemplo de .NET 4**</span><span class="sxs-lookup"><span data-stu-id="c62d3-307">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="c62d3-308">Persistencia de pertenencia a grupos</span><span class="sxs-lookup"><span data-stu-id="c62d3-308">Group membership persistence</span></span>

<span data-ttu-id="c62d3-309">Signalr realiza un seguimiento de las conexiones, no de los usuarios, por lo que si desea que un usuario esté en el mismo grupo cada vez que el usuario establece una conexión, debe llamar a `Groups.Add` cada vez que el usuario establezca una nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="c62d3-309">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="c62d3-310">Después de una pérdida temporal de conectividad, a veces Signalr puede restaurar la conexión automáticamente.</span><span class="sxs-lookup"><span data-stu-id="c62d3-310">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="c62d3-311">En ese caso, Signalr está restaurando la misma conexión, sin establecer una conexión nueva, por lo que la pertenencia al grupo del cliente se restaura automáticamente.</span><span class="sxs-lookup"><span data-stu-id="c62d3-311">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="c62d3-312">Esto es posible incluso cuando la interrupción temporal es el resultado de un reinicio o un error del servidor, ya que el estado de conexión de cada cliente, incluida la pertenencia a grupos, es de ida y vuelta al cliente.</span><span class="sxs-lookup"><span data-stu-id="c62d3-312">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="c62d3-313">Si un servidor deja de funcionar y se reemplaza por un nuevo servidor antes de que se agote el tiempo de espera de la conexión, un cliente puede volver a conectarse automáticamente al nuevo servidor y volver a inscribirse en los grupos de los que es miembro.</span><span class="sxs-lookup"><span data-stu-id="c62d3-313">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="c62d3-314">Cuando una conexión no se puede restaurar automáticamente después de una pérdida de conectividad, o cuando se agota el tiempo de espera de la conexión o cuando el cliente se desconecta (por ejemplo, cuando un explorador navega a una nueva página), se pierden las pertenencias a grupos.</span><span class="sxs-lookup"><span data-stu-id="c62d3-314">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="c62d3-315">La próxima vez que el usuario se conecte será una nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="c62d3-315">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="c62d3-316">Para mantener la pertenencia a grupos cuando el mismo usuario establece una nueva conexión, la aplicación tiene que realizar el seguimiento de las asociaciones entre usuarios y grupos, y restaurar la pertenencia a grupos cada vez que un usuario establece una nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="c62d3-316">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="c62d3-317">Para obtener más información acerca de las conexiones y las reconexiones, vea [Cómo controlar los eventos de duración de la conexión en la clase de concentrador](#connectionlifetime) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="c62d3-317">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="c62d3-318">Grupos de usuario único</span><span class="sxs-lookup"><span data-stu-id="c62d3-318">Single-user groups</span></span>

<span data-ttu-id="c62d3-319">Las aplicaciones que usan Signalr normalmente tienen que realizar un seguimiento de las asociaciones entre los usuarios y las conexiones para saber qué usuario ha enviado un mensaje y qué usuarios deben recibir un mensaje.</span><span class="sxs-lookup"><span data-stu-id="c62d3-319">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="c62d3-320">Los grupos se usan en uno de los dos patrones de uso frecuente para hacerlo.</span><span class="sxs-lookup"><span data-stu-id="c62d3-320">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="c62d3-321">Grupos de usuario único.</span><span class="sxs-lookup"><span data-stu-id="c62d3-321">Single-user groups.</span></span>

    <span data-ttu-id="c62d3-322">Puede especificar el nombre de usuario como el nombre del grupo y agregar el identificador de conexión actual al grupo cada vez que el usuario se conecta o se vuelve a conectar.</span><span class="sxs-lookup"><span data-stu-id="c62d3-322">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="c62d3-323">Para enviar mensajes al usuario que envía al grupo.</span><span class="sxs-lookup"><span data-stu-id="c62d3-323">To send messages to the user you send to the group.</span></span> <span data-ttu-id="c62d3-324">Un inconveniente de este método es que el grupo no proporciona una manera de averiguar si el usuario está en línea o sin conexión.</span><span class="sxs-lookup"><span data-stu-id="c62d3-324">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="c62d3-325">Realizar un seguimiento de las asociaciones entre los nombres de usuario y los identificadores de conexión.</span><span class="sxs-lookup"><span data-stu-id="c62d3-325">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="c62d3-326">Puede almacenar una asociación entre cada nombre de usuario y uno o varios identificadores de conexión de un diccionario o una base de datos, y actualizar los datos almacenados cada vez que el usuario se conecta o desconecta.</span><span class="sxs-lookup"><span data-stu-id="c62d3-326">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="c62d3-327">Para enviar mensajes al usuario, especifique los identificadores de conexión.</span><span class="sxs-lookup"><span data-stu-id="c62d3-327">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="c62d3-328">Una desventaja de este método es que toma más memoria.</span><span class="sxs-lookup"><span data-stu-id="c62d3-328">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="c62d3-329">Cómo controlar los eventos de duración de la conexión en la clase hub</span><span class="sxs-lookup"><span data-stu-id="c62d3-329">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="c62d3-330">Los motivos típicos para controlar los eventos de duración de la conexión son realizar un seguimiento de si un usuario está conectado o no, y realizar un seguimiento de la asociación entre los nombres de usuario y los identificadores de conexión.</span><span class="sxs-lookup"><span data-stu-id="c62d3-330">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="c62d3-331">Para ejecutar su propio código cuando los clientes se conectan o desconectan, invalide los métodos virtuales `OnConnected`, `OnDisconnected`y `OnReconnected` de la clase hub, tal y como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="c62d3-331">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="c62d3-332">Cuando se llama a alconnected, OnDisconnection y OnReconnected</span><span class="sxs-lookup"><span data-stu-id="c62d3-332">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="c62d3-333">Cada vez que un explorador navega a una nueva página, debe establecerse una nueva conexión, lo que significa que Signalr ejecutará el método `OnDisconnected` seguido del método `OnConnected`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-333">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="c62d3-334">Signalr siempre crea un nuevo identificador de conexión cuando se establece una nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="c62d3-334">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="c62d3-335">Se llama al método `OnReconnected` cuando se ha producido una interrupción temporal en la conectividad que Signalr puede recuperar automáticamente de, por ejemplo, cuando un cable se desconecta temporalmente y se vuelve a conectar antes de que se agote el tiempo de espera de la conexión. Se llama al método `OnDisconnected` cuando el cliente está desconectado y Signalr no puede volver a conectarse automáticamente, como cuando un explorador navega a una nueva página.</span><span class="sxs-lookup"><span data-stu-id="c62d3-335">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="c62d3-336">Por lo tanto, una posible secuencia de eventos para un cliente determinado es `OnConnected`, `OnReconnected``OnDisconnected`; o `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-336">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="c62d3-337">No verá la secuencia `OnConnected`, `OnDisconnected``OnReconnected` para una conexión determinada.</span><span class="sxs-lookup"><span data-stu-id="c62d3-337">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="c62d3-338">No se llama al método `OnDisconnected` en algunos escenarios, como cuando un servidor deja de funcionar o se recicla el dominio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c62d3-338">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="c62d3-339">Cuando otro servidor entra en línea o el dominio de la aplicación completa su reciclaje, algunos clientes pueden volver a conectarse y activar el evento `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-339">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="c62d3-340">Para obtener más información, consulte [Descripción y control de eventos de duración de conexión en signalr](index.md).</span><span class="sxs-lookup"><span data-stu-id="c62d3-340">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="c62d3-341">Estado del autor de la llamada no rellenado</span><span class="sxs-lookup"><span data-stu-id="c62d3-341">Caller state not populated</span></span>

<span data-ttu-id="c62d3-342">Se llama a los métodos de controlador de eventos de duración de conexión desde el servidor, lo que significa que cualquier Estado que se coloque en el objeto de `state` en el cliente no se rellenará en la propiedad `Caller` del servidor.</span><span class="sxs-lookup"><span data-stu-id="c62d3-342">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="c62d3-343">Para obtener información sobre el objeto `state` y la propiedad `Caller`, vea [Cómo pasar el estado entre clientes y la clase de concentrador](#passstate) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="c62d3-343">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="c62d3-344">Cómo obtener información sobre el cliente desde la propiedad de contexto</span><span class="sxs-lookup"><span data-stu-id="c62d3-344">How to get information about the client from the Context property</span></span>

<span data-ttu-id="c62d3-345">Para obtener información sobre el cliente, use la propiedad `Context` de la clase hub.</span><span class="sxs-lookup"><span data-stu-id="c62d3-345">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="c62d3-346">La propiedad `Context` devuelve un objeto [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) que proporciona acceso a la siguiente información:</span><span class="sxs-lookup"><span data-stu-id="c62d3-346">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="c62d3-347">El identificador de conexión del cliente que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="c62d3-347">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    <span data-ttu-id="c62d3-348">El identificador de conexión es un GUID que se asigna mediante Signalr (no puede especificar el valor en su propio código).</span><span class="sxs-lookup"><span data-stu-id="c62d3-348">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="c62d3-349">Hay un identificador de conexión para cada conexión y todos los concentradores usan el mismo identificador de conexión si tiene varios centros en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c62d3-349">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="c62d3-350">Datos de encabezado HTTP.</span><span class="sxs-lookup"><span data-stu-id="c62d3-350">HTTP header data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    <span data-ttu-id="c62d3-351">También puede obtener encabezados HTTP de `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-351">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="c62d3-352">La razón para varias referencias a lo mismo es que `Context.Headers` se creó en primer lugar, la propiedad `Context.Request` se agregó más tarde y `Context.Headers` se conserva por compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="c62d3-352">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="c62d3-353">Datos de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="c62d3-353">Query string data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    <span data-ttu-id="c62d3-354">También puede obtener datos de cadena de consulta de `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-354">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="c62d3-355">La cadena de consulta que se obtiene en esta propiedad es la que se usó con la solicitud HTTP que estableció la conexión de Signalr.</span><span class="sxs-lookup"><span data-stu-id="c62d3-355">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="c62d3-356">Puede agregar parámetros de cadena de consulta en el cliente mediante la configuración de la conexión, que es una manera cómoda de pasar datos sobre el cliente desde el cliente al servidor.</span><span class="sxs-lookup"><span data-stu-id="c62d3-356">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="c62d3-357">En el ejemplo siguiente se muestra una manera de agregar una cadena de consulta en un cliente de JavaScript cuando se usa el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="c62d3-357">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    <span data-ttu-id="c62d3-358">Para obtener más información sobre la configuración de parámetros de cadena de consulta, vea las guías de API de los clientes de [JavaScript](index.md) y [.net](index.md) .</span><span class="sxs-lookup"><span data-stu-id="c62d3-358">For more information about setting query string parameters, see the API guides for the [JavaScript](index.md) and [.NET](index.md) clients.</span></span>

    <span data-ttu-id="c62d3-359">Puede encontrar el método de transporte que se usa para la conexión en los datos de la cadena de consulta, junto con otros valores utilizados internamente por Signalr:</span><span class="sxs-lookup"><span data-stu-id="c62d3-359">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    <span data-ttu-id="c62d3-360">El valor de `transportMethod` será "WebSockets", "serverSentEvents", "foreverFrame" o "longPolling".</span><span class="sxs-lookup"><span data-stu-id="c62d3-360">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="c62d3-361">Tenga en cuenta que si comprueba este valor en el método de control de eventos `OnConnected`, en algunos escenarios podría obtener inicialmente un valor de transporte que no sea el método de transporte negociado final para la conexión.</span><span class="sxs-lookup"><span data-stu-id="c62d3-361">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="c62d3-362">En ese caso, el método producirá una excepción y se llamará de nuevo más tarde cuando se establezca el método de transporte final.</span><span class="sxs-lookup"><span data-stu-id="c62d3-362">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="c62d3-363">Propias.</span><span class="sxs-lookup"><span data-stu-id="c62d3-363">Cookies.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="c62d3-364">También puede obtener cookies de `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-364">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="c62d3-365">información del usuario.</span><span class="sxs-lookup"><span data-stu-id="c62d3-365">User information.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- <span data-ttu-id="c62d3-366">El objeto HttpContext de la solicitud:</span><span class="sxs-lookup"><span data-stu-id="c62d3-366">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    <span data-ttu-id="c62d3-367">Utilice este método en lugar de obtener `HttpContext.Current` para obtener el objeto de `HttpContext` para la conexión de Signalr.</span><span class="sxs-lookup"><span data-stu-id="c62d3-367">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="c62d3-368">Cómo pasar el estado entre los clientes y la clase de concentrador</span><span class="sxs-lookup"><span data-stu-id="c62d3-368">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="c62d3-369">El proxy de cliente proporciona un `state` objeto en el que puede almacenar los datos que desea que se transmitan al servidor con cada llamada al método.</span><span class="sxs-lookup"><span data-stu-id="c62d3-369">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="c62d3-370">En el servidor, puede tener acceso a estos datos en la propiedad `Clients.Caller` en métodos de concentrador a los que llaman los clientes.</span><span class="sxs-lookup"><span data-stu-id="c62d3-370">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="c62d3-371">La propiedad `Clients.Caller` no se rellena para los métodos de controlador de eventos de duración de conexión `OnConnected`, `OnDisconnected`y `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-371">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="c62d3-372">La creación o la actualización de datos en el objeto `state` y la propiedad `Clients.Caller` funciona en ambas direcciones.</span><span class="sxs-lookup"><span data-stu-id="c62d3-372">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="c62d3-373">Puede actualizar los valores en el servidor y devolverlos al cliente.</span><span class="sxs-lookup"><span data-stu-id="c62d3-373">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="c62d3-374">En el ejemplo siguiente se muestra el código de cliente de JavaScript que almacena el estado de la transmisión al servidor con cada llamada al método.</span><span class="sxs-lookup"><span data-stu-id="c62d3-374">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

<span data-ttu-id="c62d3-375">En el ejemplo siguiente se muestra el código equivalente en un cliente .NET.</span><span class="sxs-lookup"><span data-stu-id="c62d3-375">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

<span data-ttu-id="c62d3-376">En la clase hub, puede tener acceso a estos datos en la propiedad `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-376">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="c62d3-377">En el ejemplo siguiente se muestra código que recupera el estado al que se hace referencia en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="c62d3-377">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="c62d3-378">Este mecanismo para conservar el estado no está pensado para grandes cantidades de datos, ya que todo lo que se coloca en la propiedad `state` o `Clients.Caller` se recorre en ida y vuelta con cada invocación de método.</span><span class="sxs-lookup"><span data-stu-id="c62d3-378">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="c62d3-379">Resulta útil para los elementos más pequeños, como los nombres de usuario o los contadores.</span><span class="sxs-lookup"><span data-stu-id="c62d3-379">It's useful for smaller items such as user names or counters.</span></span>

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="c62d3-380">Cómo controlar errores en la clase hub</span><span class="sxs-lookup"><span data-stu-id="c62d3-380">How to handle errors in the Hub class</span></span>

<span data-ttu-id="c62d3-381">Para controlar los errores que se producen en los métodos de clase de concentrador, use uno de los métodos siguientes o ambos:</span><span class="sxs-lookup"><span data-stu-id="c62d3-381">To handle errors that occur in your Hub class methods, use one or both of the following methods:</span></span>

- <span data-ttu-id="c62d3-382">Ajuste el código del método en los bloques try-catch y registre el objeto de excepción.</span><span class="sxs-lookup"><span data-stu-id="c62d3-382">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="c62d3-383">Con fines de depuración, puede enviar la excepción al cliente, pero no se recomienda por motivos de seguridad que envíen información detallada a los clientes de producción.</span><span class="sxs-lookup"><span data-stu-id="c62d3-383">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="c62d3-384">Cree un módulo de canalización de hubs que controle el método [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) .</span><span class="sxs-lookup"><span data-stu-id="c62d3-384">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="c62d3-385">En el ejemplo siguiente se muestra un módulo de canalización que registra errores, seguido del código de global. asax que inserta el módulo en la canalización hubs.</span><span class="sxs-lookup"><span data-stu-id="c62d3-385">The following example shows a pipeline module that logs errors, followed by code in Global.asax that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

<span data-ttu-id="c62d3-386">Para obtener más información acerca de los módulos de canalización de concentrador, consulte [Personalización de la canalización de hubs](#hubpipeline) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="c62d3-386">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="c62d3-387">Cómo habilitar el seguimiento</span><span class="sxs-lookup"><span data-stu-id="c62d3-387">How to enable tracing</span></span>

<span data-ttu-id="c62d3-388">Para habilitar el seguimiento del lado servidor, agregue un elemento System. Diagnostics al archivo Web. config, tal como se muestra en este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c62d3-388">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

<span data-ttu-id="c62d3-389">Al ejecutar la aplicación en Visual Studio, puede ver los registros en la ventana **resultados** .</span><span class="sxs-lookup"><span data-stu-id="c62d3-389">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="c62d3-390">Cómo llamar a métodos de cliente y administrar grupos desde fuera de la clase hub</span><span class="sxs-lookup"><span data-stu-id="c62d3-390">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="c62d3-391">Para llamar a métodos de cliente desde una clase diferente de la clase de concentrador, obtenga una referencia al objeto de contexto de Signalr para el concentrador y úselo para llamar a métodos en el cliente o administrar grupos.</span><span class="sxs-lookup"><span data-stu-id="c62d3-391">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="c62d3-392">En el siguiente ejemplo `StockTicker` clase se obtiene el objeto de contexto, se almacena en una instancia de la clase, se almacena la instancia de la clase en una propiedad estática y se usa el contexto de la instancia de la clase singleton para llamar al método `updateStockPrice` en los clientes que están conectados a un concentrador denominado `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-392">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

<span data-ttu-id="c62d3-393">Si necesita usar el contexto varias veces en un objeto de larga duración, obtenga la referencia una vez y guárdela en lugar de volver a obtenerla cada vez.</span><span class="sxs-lookup"><span data-stu-id="c62d3-393">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="c62d3-394">Obtener el contexto una vez garantiza que Signalr envía mensajes a los clientes en la misma secuencia en la que los métodos del concentrador realizan las invocaciones de métodos de cliente.</span><span class="sxs-lookup"><span data-stu-id="c62d3-394">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="c62d3-395">Para ver un tutorial que muestra cómo usar el contexto de Signalr para un concentrador, consulte [difusión del servidor con ASP.net signalr](index.md).</span><span class="sxs-lookup"><span data-stu-id="c62d3-395">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](index.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="c62d3-396">Llamar a métodos de cliente</span><span class="sxs-lookup"><span data-stu-id="c62d3-396">Calling client methods</span></span>

<span data-ttu-id="c62d3-397">Puede especificar qué clientes recibirán la RPC, pero tiene menos opciones que al llamar desde una clase de concentrador.</span><span class="sxs-lookup"><span data-stu-id="c62d3-397">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="c62d3-398">La razón es que el contexto no está asociado a una llamada determinada de un cliente, por lo que no están disponibles los métodos que requieren conocimientos del identificador de conexión actual, como `Clients.Others`, o `Clients.Caller`o `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-398">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="c62d3-399">Están disponibles las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="c62d3-399">The following options are available:</span></span>

- <span data-ttu-id="c62d3-400">Todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="c62d3-400">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- <span data-ttu-id="c62d3-401">Un cliente específico identificado por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="c62d3-401">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- <span data-ttu-id="c62d3-402">Todos los clientes conectados, excepto los clientes especificados, identificados por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="c62d3-402">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- <span data-ttu-id="c62d3-403">Todos los clientes conectados en un grupo especificado.</span><span class="sxs-lookup"><span data-stu-id="c62d3-403">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- <span data-ttu-id="c62d3-404">Todos los clientes conectados en un grupo especificado excepto los clientes especificados, identificados por el identificador de conexión.</span><span class="sxs-lookup"><span data-stu-id="c62d3-404">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

<span data-ttu-id="c62d3-405">Si está llamando a la clase que no es de Hub desde los métodos de la clase hub, puede pasar el identificador de conexión actual y usarlo con `Clients.Client`, `Clients.AllExcept`o `Clients.Group` para simular `Clients.Caller`, `Clients.Others`o `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-405">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="c62d3-406">En el ejemplo siguiente, la clase `MoveShapeHub` pasa el identificador de conexión a la clase `Broadcaster` para que la clase `Broadcaster` pueda simular `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="c62d3-406">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="c62d3-407">Administrar la pertenencia a grupos</span><span class="sxs-lookup"><span data-stu-id="c62d3-407">Managing group membership</span></span>

<span data-ttu-id="c62d3-408">Para administrar grupos, tiene las mismas opciones que en una clase de Hub.</span><span class="sxs-lookup"><span data-stu-id="c62d3-408">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="c62d3-409">Agregar un cliente a un grupo</span><span class="sxs-lookup"><span data-stu-id="c62d3-409">Add a client to a group</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- <span data-ttu-id="c62d3-410">Quitar un cliente de un grupo</span><span class="sxs-lookup"><span data-stu-id="c62d3-410">Remove a client from a group</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="c62d3-411">Personalización de la canalización de hubs</span><span class="sxs-lookup"><span data-stu-id="c62d3-411">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="c62d3-412">Signalr le permite insertar su propio código en la canalización del concentrador.</span><span class="sxs-lookup"><span data-stu-id="c62d3-412">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="c62d3-413">En el ejemplo siguiente se muestra un módulo de canalización de concentrador personalizado que registra cada llamada de método entrante recibida del cliente y la llamada al método de salida invocada en el cliente:</span><span class="sxs-lookup"><span data-stu-id="c62d3-413">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

<span data-ttu-id="c62d3-414">El siguiente código del archivo *global. asax* registra el módulo para ejecutarse en la canalización del concentrador:</span><span class="sxs-lookup"><span data-stu-id="c62d3-414">The following code in the *Global.asax* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

<span data-ttu-id="c62d3-415">Hay muchos métodos diferentes que puede invalidar.</span><span class="sxs-lookup"><span data-stu-id="c62d3-415">There are many different methods that you can override.</span></span> <span data-ttu-id="c62d3-416">Para obtener una lista completa, consulte [métodos de HubPipelineModule](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="c62d3-416">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
