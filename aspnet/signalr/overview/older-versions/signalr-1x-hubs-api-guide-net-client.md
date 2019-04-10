---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: 'Guía de API de ASP.NET SignalR Hubs: cliente .NET (SignalR 1.x) | Microsoft Docs'
author: bradygaster
description: Este documento proporciona una introducción al uso de la API de concentradores de SignalR en clientes. NET, como Windows Store (WinRT), WPF, Silverlight y los contras de la versión 2...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 1551b4533e05a6cd7dcc29e4c6bc17e854889ee8
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59402250"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a><span data-ttu-id="7e3af-103">Guía de API de ASP.NET SignalR Hubs: cliente .NET (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="7e3af-103">ASP.NET SignalR Hubs API Guide - .NET Client (SignalR 1.x)</span></span>

<span data-ttu-id="7e3af-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7e3af-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="7e3af-105">Este documento proporciona una introducción al uso de la API de concentradores de SignalR versión 2 en los clientes. NET, como aplicaciones de consola, WPF, Silverlight y Windows Store (WinRT).</span><span class="sxs-lookup"><span data-stu-id="7e3af-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="7e3af-106">La API de concentradores de SignalR permite realizar llamadas a procedimiento remoto (RPC) de un servidor a los clientes conectados y de clientes en el servidor.</span><span class="sxs-lookup"><span data-stu-id="7e3af-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="7e3af-107">En el código de servidor, definir los métodos que se pueden llamar a los clientes y llamar a métodos que se ejecutan en el cliente.</span><span class="sxs-lookup"><span data-stu-id="7e3af-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="7e3af-108">En el código de cliente, definir los métodos que pueden llamarse desde el servidor y llamar a métodos que se ejecutan en el servidor.</span><span class="sxs-lookup"><span data-stu-id="7e3af-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="7e3af-109">SignalR se ocupa de todos los mecanismos de cliente a servidor para usted.</span><span class="sxs-lookup"><span data-stu-id="7e3af-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="7e3af-110">SignalR también ofrece una API de nivel inferior denominada conexiones persistentes.</span><span class="sxs-lookup"><span data-stu-id="7e3af-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="7e3af-111">Para obtener una introducción a SignalR, concentradores y conexiones persistentes, o para ver un tutorial que muestra cómo crear una aplicación de SignalR completa, consulte [SignalR - Introducción a](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="7e3af-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="7e3af-112">Información general</span><span class="sxs-lookup"><span data-stu-id="7e3af-112">Overview</span></span>

<span data-ttu-id="7e3af-113">Este documento contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="7e3af-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="7e3af-114">Programa de instalación de cliente</span><span class="sxs-lookup"><span data-stu-id="7e3af-114">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="7e3af-115">Cómo establecer una conexión</span><span class="sxs-lookup"><span data-stu-id="7e3af-115">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="7e3af-116">Conexiones entre dominios desde clientes de Silverlight</span><span class="sxs-lookup"><span data-stu-id="7e3af-116">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="7e3af-117">Cómo configurar la conexión</span><span class="sxs-lookup"><span data-stu-id="7e3af-117">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="7e3af-118">Cómo establecer el número máximo de conexiones simultáneas en los clientes WPF</span><span class="sxs-lookup"><span data-stu-id="7e3af-118">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="7e3af-119">Cómo especificar parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="7e3af-119">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="7e3af-120">Cómo especificar el modo de transporte</span><span class="sxs-lookup"><span data-stu-id="7e3af-120">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="7e3af-121">Cómo especificar los encabezados HTTP</span><span class="sxs-lookup"><span data-stu-id="7e3af-121">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="7e3af-122">Cómo especificar los certificados de cliente</span><span class="sxs-lookup"><span data-stu-id="7e3af-122">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="7e3af-123">Cómo crear al proxy de concentrador</span><span class="sxs-lookup"><span data-stu-id="7e3af-123">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="7e3af-124">Cómo definir métodos en el cliente que el servidor puede llamar a</span><span class="sxs-lookup"><span data-stu-id="7e3af-124">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="7e3af-125">Métodos sin parámetros</span><span class="sxs-lookup"><span data-stu-id="7e3af-125">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="7e3af-126">Métodos con parámetros, que se especifiquen tipos de parámetros</span><span class="sxs-lookup"><span data-stu-id="7e3af-126">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="7e3af-127">Métodos con parámetros, que se especifiquen objetos dinámicos para los parámetros</span><span class="sxs-lookup"><span data-stu-id="7e3af-127">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="7e3af-128">Cómo quitar un controlador</span><span class="sxs-lookup"><span data-stu-id="7e3af-128">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="7e3af-129">Cómo llamar a métodos del servidor desde el cliente</span><span class="sxs-lookup"><span data-stu-id="7e3af-129">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="7e3af-130">Cómo controlar eventos de duración de la conexión</span><span class="sxs-lookup"><span data-stu-id="7e3af-130">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="7e3af-131">Cómo controlar los errores</span><span class="sxs-lookup"><span data-stu-id="7e3af-131">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="7e3af-132">Cómo habilitar el registro del lado cliente</span><span class="sxs-lookup"><span data-stu-id="7e3af-132">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="7e3af-133">Ejemplos de código de aplicación de consola para que el servidor puede llamar a métodos del cliente, Silverlight y WPF</span><span class="sxs-lookup"><span data-stu-id="7e3af-133">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="7e3af-134">Para un proyecto de cliente .NET de ejemplo, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="7e3af-134">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="7e3af-135">[Gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) en GitHub.com (ejemplos de aplicación de WinRT, Silverlight, consola).</span><span class="sxs-lookup"><span data-stu-id="7e3af-135">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="7e3af-136">[DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) en GitHub.com (ejemplo de WPF).</span><span class="sxs-lookup"><span data-stu-id="7e3af-136">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="7e3af-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) en GitHub.com (ejemplo de aplicación de consola).</span><span class="sxs-lookup"><span data-stu-id="7e3af-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="7e3af-138">Para obtener documentación sobre cómo programar el servidor o los clientes de JavaScript, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="7e3af-138">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="7e3af-139">Guía de la API SignalR Hubs - servidor</span><span class="sxs-lookup"><span data-stu-id="7e3af-139">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="7e3af-140">Guía de API de concentradores de SignalR: cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="7e3af-140">SignalR Hubs API Guide - JavaScript Client</span></span>](../guide-to-the-api/hubs-api-guide-javascript-client.md)

<span data-ttu-id="7e3af-141">Son vínculos a temas de referencia de API a la versión 4.5 de .NET de la API.</span><span class="sxs-lookup"><span data-stu-id="7e3af-141">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="7e3af-142">Si usa .NET 4, consulte [la versión 4 de .NET de los temas de la API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="7e3af-142">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="7e3af-143">Programa de instalación de cliente</span><span class="sxs-lookup"><span data-stu-id="7e3af-143">Client setup</span></span>

<span data-ttu-id="7e3af-144">Instalar el [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) paquete NuGet (no el [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) paquete).</span><span class="sxs-lookup"><span data-stu-id="7e3af-144">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="7e3af-145">Este paquete es compatible con WinRT, WPF, Silverlight, aplicación de consola y los clientes de Windows Phone, para .NET 4 y .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="7e3af-145">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="7e3af-146">Si la versión de SignalR que existen en el cliente es diferente de la versión que tiene en el servidor, SignalR a menudo es capaz de adaptarse a la diferencia.</span><span class="sxs-lookup"><span data-stu-id="7e3af-146">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="7e3af-147">Por ejemplo, cuando se lanza la versión 2.0 de SignalR e instala en el servidor, el servidor será compatible con los clientes que tienen instalado, así como los clientes que tienen instalado 2.0 de 1.1.</span><span class="sxs-lookup"><span data-stu-id="7e3af-147">For example, when SignalR version 2.0 is released and you install that on the server, the server will support clients that have 1.1.x installed as well as clients that have 2.0 installed.</span></span> <span data-ttu-id="7e3af-148">Si la diferencia entre la versión en el servidor y la versión en el cliente es demasiado grande, SignalR genera un `InvalidOperationException` excepción cuando el cliente intenta establecer una conexión.</span><span class="sxs-lookup"><span data-stu-id="7e3af-148">If the difference between the version on the server and the version on the client is too great, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="7e3af-149">El mensaje de error es "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="7e3af-149">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="7e3af-150">Cómo establecer una conexión</span><span class="sxs-lookup"><span data-stu-id="7e3af-150">How to establish a connection</span></span>

<span data-ttu-id="7e3af-151">Antes de establecer una conexión, debe crear un `HubConnection` de objetos y crear un proxy.</span><span class="sxs-lookup"><span data-stu-id="7e3af-151">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="7e3af-152">Para establecer la conexión, llame a la `Start` método en el `HubConnection` objeto.</span><span class="sxs-lookup"><span data-stu-id="7e3af-152">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="7e3af-153">Para los clientes de JavaScript tiene que registrar al menos un controlador de eventos antes de llamar a la `Start` método para establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="7e3af-153">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="7e3af-154">Esto no es necesario para los clientes. NET.</span><span class="sxs-lookup"><span data-stu-id="7e3af-154">This is not necessary for .NET clients.</span></span> <span data-ttu-id="7e3af-155">Para los clientes de JavaScript, el código proxy generado automáticamente crea objetos proxy para todos los centros que existen en el servidor y registrar un controlador es el modo de indicar qué Hubs el cliente desea usar.</span><span class="sxs-lookup"><span data-stu-id="7e3af-155">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="7e3af-156">Pero para que un cliente .NET crear a servidores proxy de concentrador manualmente, por lo que SignalR se da por supuesto que va a usar cualquier Hub que ha creado a un proxy para.</span><span class="sxs-lookup"><span data-stu-id="7e3af-156">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="7e3af-157">El código de ejemplo usa el valor predeterminado "/ signalr" dirección URL para conectarse a su servicio SignalR.</span><span class="sxs-lookup"><span data-stu-id="7e3af-157">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="7e3af-158">Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [Guía de la API de ASP.NET SignalR Hubs - Server - la dirección URL de /signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="7e3af-158">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="7e3af-159">El `Start` método se ejecuta de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="7e3af-159">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="7e3af-160">Para asegurarse de que no se ejecutan las siguientes líneas de código hasta una vez establecida la conexión, use `await` en un método asincrónico de ASP.NET 4.5 o `.Wait()` en un método sincrónico.</span><span class="sxs-lookup"><span data-stu-id="7e3af-160">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="7e3af-161">No use `.Wait()` en un cliente de WinRT.</span><span class="sxs-lookup"><span data-stu-id="7e3af-161">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<span data-ttu-id="7e3af-162">La clase `HubConnection` es segura para la ejecución de subprocesos.</span><span class="sxs-lookup"><span data-stu-id="7e3af-162">The `HubConnection` class is thread-safe.</span></span>

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="7e3af-163">Conexiones entre dominios desde clientes de Silverlight</span><span class="sxs-lookup"><span data-stu-id="7e3af-163">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="7e3af-164">Para obtener información acerca de cómo habilitar las conexiones entre dominios desde clientes de Silverlight, vea [hacer que un servicio disponible a través de los límites del dominio](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="7e3af-164">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="7e3af-165">Cómo configurar la conexión</span><span class="sxs-lookup"><span data-stu-id="7e3af-165">How to configure the connection</span></span>

<span data-ttu-id="7e3af-166">Antes de establecer una conexión, puede especificar cualquiera de las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="7e3af-166">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="7e3af-167">Límite de conexiones simultáneas.</span><span class="sxs-lookup"><span data-stu-id="7e3af-167">Concurrent connections limit.</span></span>
- <span data-ttu-id="7e3af-168">Parámetros de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="7e3af-168">Query string parameters.</span></span>
- <span data-ttu-id="7e3af-169">El método de transporte.</span><span class="sxs-lookup"><span data-stu-id="7e3af-169">The transport method.</span></span>
- <span data-ttu-id="7e3af-170">Encabezados HTTP.</span><span class="sxs-lookup"><span data-stu-id="7e3af-170">HTTP headers.</span></span>
- <span data-ttu-id="7e3af-171">Certificados de cliente.</span><span class="sxs-lookup"><span data-stu-id="7e3af-171">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="7e3af-172">Cómo establecer el número máximo de conexiones simultáneas en los clientes WPF</span><span class="sxs-lookup"><span data-stu-id="7e3af-172">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="7e3af-173">En los clientes WPF, es posible que deba aumentar el número máximo de conexiones simultáneas de su valor predeterminado de 2.</span><span class="sxs-lookup"><span data-stu-id="7e3af-173">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="7e3af-174">El valor recomendado es 10.</span><span class="sxs-lookup"><span data-stu-id="7e3af-174">The recommended value is 10.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="7e3af-175">Para obtener más información, consulte [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="7e3af-175">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="7e3af-176">Cómo especificar parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="7e3af-176">How to specify query string parameters</span></span>

<span data-ttu-id="7e3af-177">Si desea enviar datos al servidor cuando el cliente se conecta, puede agregar parámetros de cadena de consulta para el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="7e3af-177">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="7e3af-178">El ejemplo siguiente muestra cómo establecer un parámetro de cadena de consulta en código de cliente.</span><span class="sxs-lookup"><span data-stu-id="7e3af-178">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="7e3af-179">El ejemplo siguiente muestra cómo leer un parámetro de cadena de consulta en código del servidor.</span><span class="sxs-lookup"><span data-stu-id="7e3af-179">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="7e3af-180">Cómo especificar el modo de transporte</span><span class="sxs-lookup"><span data-stu-id="7e3af-180">How to specify the transport method</span></span>

<span data-ttu-id="7e3af-181">Como parte del proceso de conexión, un cliente de SignalR normalmente negocia con el servidor para determinar el mejor transporte que se admite por servidor y cliente.</span><span class="sxs-lookup"><span data-stu-id="7e3af-181">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="7e3af-182">Si ya conoce el tipo de transporte que desea usar, puede omitir este proceso de negociación.</span><span class="sxs-lookup"><span data-stu-id="7e3af-182">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="7e3af-183">Para especificar el método de transporte, pasar un objeto de transporte al método Start.</span><span class="sxs-lookup"><span data-stu-id="7e3af-183">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="7e3af-184">El ejemplo siguiente muestra cómo especificar el modo de transporte en el código de cliente.</span><span class="sxs-lookup"><span data-stu-id="7e3af-184">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="7e3af-185">El [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) espacio de nombres incluye las siguientes clases que se pueden utilizar para especificar el transporte.</span><span class="sxs-lookup"><span data-stu-id="7e3af-185">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- [<span data-ttu-id="7e3af-186">LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="7e3af-186">LongPollingTransport</span></span>](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [<span data-ttu-id="7e3af-187">ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="7e3af-187">ServerSentEventsTransport</span></span>](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- <span data-ttu-id="7e3af-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponible solo cuando el servidor y cliente usan .NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="7e3af-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="7e3af-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (elige automáticamente el mejor transporte es compatible con el cliente y el servidor.</span><span class="sxs-lookup"><span data-stu-id="7e3af-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="7e3af-190">Se trata el transporte predeterminado.</span><span class="sxs-lookup"><span data-stu-id="7e3af-190">This is the default transport.</span></span> <span data-ttu-id="7e3af-191">Esto en para pasar el `Start` método tiene el mismo efecto que no pasa nada.)</span><span class="sxs-lookup"><span data-stu-id="7e3af-191">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="7e3af-192">El transporte ForeverFrame no se incluye en esta lista, porque se utiliza únicamente por los exploradores.</span><span class="sxs-lookup"><span data-stu-id="7e3af-192">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="7e3af-193">Para obtener información sobre cómo comprobar el modo de transporte en el código de servidor, consulte [Guía de la API de ASP.NET SignalR Hubs - Server - obtener información sobre el cliente de la propiedad de contexto](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="7e3af-193">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="7e3af-194">Para obtener más información acerca de los transportes y reservas, consulte [Introducción a SignalR - transportes y reservas](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="7e3af-194">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="7e3af-195">Cómo especificar los encabezados HTTP</span><span class="sxs-lookup"><span data-stu-id="7e3af-195">How to specify HTTP headers</span></span>

<span data-ttu-id="7e3af-196">Para establecer encabezados HTTP, utilice el `Headers` propiedad del objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="7e3af-196">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="7e3af-197">El ejemplo siguiente muestra cómo agregar un encabezado HTTP.</span><span class="sxs-lookup"><span data-stu-id="7e3af-197">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="7e3af-198">Cómo especificar los certificados de cliente</span><span class="sxs-lookup"><span data-stu-id="7e3af-198">How to specify client certificates</span></span>

<span data-ttu-id="7e3af-199">Para agregar certificados de cliente, use el `AddClientCertificate` método en el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="7e3af-199">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="7e3af-200">Cómo crear al proxy de concentrador</span><span class="sxs-lookup"><span data-stu-id="7e3af-200">How to create the Hub proxy</span></span>

<span data-ttu-id="7e3af-201">Con el fin de definir métodos en el cliente que se puede llamar un concentrador del servidor y para invocar métodos en un centro en el servidor, crear un proxy para el concentrador mediante una llamada a `CreateHubProxy` en el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="7e3af-201">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="7e3af-202">La cadena se pasa a `CreateHubProxy` es el nombre de la clase Hub o el nombre especificado por el `HubName` atributo si se usó en el servidor.</span><span class="sxs-lookup"><span data-stu-id="7e3af-202">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="7e3af-203">Nombre de la coincidencia distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="7e3af-203">Name matching is case-insensitive.</span></span>

**<span data-ttu-id="7e3af-204">Clase de Hub en servidor</span><span class="sxs-lookup"><span data-stu-id="7e3af-204">Hub class on server</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**<span data-ttu-id="7e3af-205">Crear el proxy de cliente para la clase Hub</span><span class="sxs-lookup"><span data-stu-id="7e3af-205">Create client proxy for the Hub class</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="7e3af-206">Si decorar la clase de Hub con un `HubName` atributo, use ese nombre.</span><span class="sxs-lookup"><span data-stu-id="7e3af-206">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

**<span data-ttu-id="7e3af-207">Clase de Hub en servidor</span><span class="sxs-lookup"><span data-stu-id="7e3af-207">Hub class on server</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

**<span data-ttu-id="7e3af-208">Crear el proxy de cliente para la clase Hub</span><span class="sxs-lookup"><span data-stu-id="7e3af-208">Create client proxy for the Hub class</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="7e3af-209">El objeto proxy es segura para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="7e3af-209">The proxy object is thread-safe.</span></span> <span data-ttu-id="7e3af-210">De hecho, si se llama a `HubConnection.CreateHubProxy` varias veces con el mismo `hubName`, obtendrá la misma caché `IHubProxy` objeto.</span><span class="sxs-lookup"><span data-stu-id="7e3af-210">In fact, if you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="7e3af-211">Cómo definir métodos en el cliente que el servidor puede llamar a</span><span class="sxs-lookup"><span data-stu-id="7e3af-211">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="7e3af-212">Para definir un método que puede llamar el servidor, utilice el proxy `On` método para registrar un controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="7e3af-212">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="7e3af-213">Coincidencia de nombres de método distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="7e3af-213">Method name matching is case-insensitive.</span></span> <span data-ttu-id="7e3af-214">Por ejemplo, `Clients.All.UpdateStockPrice` se ejecutará en el servidor `updateStockPrice`, `updatestockprice`, o `UpdateStockPrice` en el cliente.</span><span class="sxs-lookup"><span data-stu-id="7e3af-214">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="7e3af-215">Plataformas de cliente diferentes tienen requisitos diferentes para cómo escribir código del método para actualizar la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="7e3af-215">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="7e3af-216">Los ejemplos mostrados son para los clientes de WinRT (Windows Store,. NET).</span><span class="sxs-lookup"><span data-stu-id="7e3af-216">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="7e3af-217">Se proporcionan ejemplos de aplicación de consola, WPF y Silverlight en [una sección independiente más adelante en este tema](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="7e3af-217">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="7e3af-218">Métodos sin parámetros</span><span class="sxs-lookup"><span data-stu-id="7e3af-218">Methods without parameters</span></span>

<span data-ttu-id="7e3af-219">Si el método que se está administrando no tiene parámetros, use la sobrecarga no genérica de la `On` método:</span><span class="sxs-lookup"><span data-stu-id="7e3af-219">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

**<span data-ttu-id="7e3af-220">Código de servidor, llamar al método de cliente sin parámetros</span><span class="sxs-lookup"><span data-stu-id="7e3af-220">Server code calling client method without parameters</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**<span data-ttu-id="7e3af-221">Código de cliente de WinRT para el método se llama desde el servidor sin parámetros ([ver ejemplos WPF y Silverlight más adelante en este tema](#wpfsl))</span><span class="sxs-lookup"><span data-stu-id="7e3af-221">WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="7e3af-222">Métodos con parámetros, especificando los tipos de parámetro</span><span class="sxs-lookup"><span data-stu-id="7e3af-222">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="7e3af-223">Si el método que está administrando tiene parámetros, especifique los tipos de los parámetros como los tipos genéricos de la `On` método.</span><span class="sxs-lookup"><span data-stu-id="7e3af-223">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="7e3af-224">Hay sobrecargas genéricas de los `On` método para permitirle especificar parámetros de hasta 8 (4 en Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="7e3af-224">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="7e3af-225">En el ejemplo siguiente, se envía un parámetro a la `UpdateStockPrice` método.</span><span class="sxs-lookup"><span data-stu-id="7e3af-225">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

**<span data-ttu-id="7e3af-226">Llamar al método de cliente con un parámetro de código de servidor</span><span class="sxs-lookup"><span data-stu-id="7e3af-226">Server code calling client method with a parameter</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**<span data-ttu-id="7e3af-227">La clase de acción utilizada para el parámetro</span><span class="sxs-lookup"><span data-stu-id="7e3af-227">The Stock class used for the parameter</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

**<span data-ttu-id="7e3af-228">Código de cliente de WinRT para un método se llama desde el servidor con un parámetro ([ver ejemplos WPF y Silverlight más adelante en este tema](#wpfsl))</span><span class="sxs-lookup"><span data-stu-id="7e3af-228">WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="7e3af-229">Métodos con parámetros, que se especifiquen objetos dinámicos para los parámetros</span><span class="sxs-lookup"><span data-stu-id="7e3af-229">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="7e3af-230">Como alternativa a especificar parámetros como tipos genéricos de la `On` método, puede especificar parámetros como objetos dinámicos:</span><span class="sxs-lookup"><span data-stu-id="7e3af-230">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

**<span data-ttu-id="7e3af-231">Llamar al método de cliente con un parámetro de código de servidor</span><span class="sxs-lookup"><span data-stu-id="7e3af-231">Server code calling client method with a parameter</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**<span data-ttu-id="7e3af-232">La clase de acción utilizada para el parámetro</span><span class="sxs-lookup"><span data-stu-id="7e3af-232">The Stock class used for the parameter</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

**<span data-ttu-id="7e3af-233">Código de cliente de WinRT para un método se llama desde el servidor con un parámetro, con un objeto dinámico para el parámetro ([ver ejemplos WPF y Silverlight más adelante en este tema](#wpfsl))</span><span class="sxs-lookup"><span data-stu-id="7e3af-233">WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="7e3af-234">Cómo quitar un controlador</span><span class="sxs-lookup"><span data-stu-id="7e3af-234">How to remove a handler</span></span>

<span data-ttu-id="7e3af-235">Para quitar un controlador, llame a su `Dispose` método.</span><span class="sxs-lookup"><span data-stu-id="7e3af-235">To remove a handler, call its `Dispose` method.</span></span>

**<span data-ttu-id="7e3af-236">Código de cliente para un método llamado desde servidor</span><span class="sxs-lookup"><span data-stu-id="7e3af-236">Client code for a method called from server</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**<span data-ttu-id="7e3af-237">Al quitar el controlador de código de cliente</span><span class="sxs-lookup"><span data-stu-id="7e3af-237">Client code to remove the handler</span></span>**

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="7e3af-238">Cómo llamar a métodos del servidor desde el cliente</span><span class="sxs-lookup"><span data-stu-id="7e3af-238">How to call server methods from the client</span></span>

<span data-ttu-id="7e3af-239">Para llamar a un método en el servidor, use el `Invoke` método en el proxy de concentrador.</span><span class="sxs-lookup"><span data-stu-id="7e3af-239">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="7e3af-240">Si el método de servidor no tiene ningún valor devuelto, use la sobrecarga no genérica de la `Invoke` método.</span><span class="sxs-lookup"><span data-stu-id="7e3af-240">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

**<span data-ttu-id="7e3af-241">Código de servidor para un método que no tiene ningún valor devuelto</span><span class="sxs-lookup"><span data-stu-id="7e3af-241">Server code for a method that has no return value</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**<span data-ttu-id="7e3af-242">Código de cliente llama a un método que no tiene ningún valor devuelto</span><span class="sxs-lookup"><span data-stu-id="7e3af-242">Client code calling a method that has no return value</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="7e3af-243">Si el método de servidor tiene un valor devuelto, especifique el tipo de valor devuelto como el tipo genérico de la `Invoke` método.</span><span class="sxs-lookup"><span data-stu-id="7e3af-243">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

**<span data-ttu-id="7e3af-244">Código de servidor para un método que tiene un valor devuelto y toma un parámetro de tipo complejo</span><span class="sxs-lookup"><span data-stu-id="7e3af-244">Server code for a method that has a return value and takes a complex type parameter</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**<span data-ttu-id="7e3af-245">La clase de Stock utilizada para el parámetro y valor devuelto</span><span class="sxs-lookup"><span data-stu-id="7e3af-245">The Stock class used for the parameter and return value</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

**<span data-ttu-id="7e3af-246">Código de cliente llama a un método que tiene un valor devuelto y toma un parámetro de tipo complejo, en un método asincrónico de ASP.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="7e3af-246">Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**<span data-ttu-id="7e3af-247">Código de cliente llama a un método que tiene un valor devuelto y toma un parámetro de tipo complejo, en un método sincrónico</span><span class="sxs-lookup"><span data-stu-id="7e3af-247">Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="7e3af-248">El `Invoke` método se ejecuta de forma asincrónica y devuelve un `Task` objeto.</span><span class="sxs-lookup"><span data-stu-id="7e3af-248">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="7e3af-249">Si no se especifica `await` o `.Wait()`, la siguiente línea de código se ejecutará antes de que el método que invoca ha terminado de ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="7e3af-249">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="7e3af-250">Cómo controlar eventos de duración de la conexión</span><span class="sxs-lookup"><span data-stu-id="7e3af-250">How to handle connection lifetime events</span></span>

<span data-ttu-id="7e3af-251">SignalR proporciona la siguiente conexión de eventos de duración que puede controlar:</span><span class="sxs-lookup"><span data-stu-id="7e3af-251">SignalR provides the following connection lifetime events that you can handle:</span></span>

- `Received`<span data-ttu-id="7e3af-252">: Se genera cuando se recibe ningún dato en la conexión.</span><span class="sxs-lookup"><span data-stu-id="7e3af-252">: Raised when any data is received on the connection.</span></span> <span data-ttu-id="7e3af-253">Proporciona los datos recibidos.</span><span class="sxs-lookup"><span data-stu-id="7e3af-253">Provides the received data.</span></span>
- `ConnectionSlow`<span data-ttu-id="7e3af-254">: Se genera cuando el cliente detecta una conexión lenta o eliminación con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="7e3af-254">: Raised when the client detects a slow or frequently dropping connection.</span></span>
- `Reconnecting`<span data-ttu-id="7e3af-255">: Se genera cuando comienza a volver a conectar el transporte subyacente.</span><span class="sxs-lookup"><span data-stu-id="7e3af-255">: Raised when the underlying transport begins reconnecting.</span></span>
- `Reconnected`<span data-ttu-id="7e3af-256">: Se genera cuando se ha vuelto a conectar el transporte subyacente.</span><span class="sxs-lookup"><span data-stu-id="7e3af-256">: Raised when the underlying transport has reconnected.</span></span>
- `StateChanged`<span data-ttu-id="7e3af-257">: Se genera cuando cambia el estado de conexión.</span><span class="sxs-lookup"><span data-stu-id="7e3af-257">: Raised when the connection state changes.</span></span> <span data-ttu-id="7e3af-258">Proporciona el estado antiguo y el estado nueva.</span><span class="sxs-lookup"><span data-stu-id="7e3af-258">Provides the old state and the new state.</span></span> <span data-ttu-id="7e3af-259">Para obtener información acerca de la conexión que vea los valores de estado [enumeración ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="7e3af-259">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- `Closed`<span data-ttu-id="7e3af-260">: Se genera cuando se ha desconectado la conexión.</span><span class="sxs-lookup"><span data-stu-id="7e3af-260">: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="7e3af-261">Por ejemplo, si desea mostrar los mensajes de advertencia de errores que no son irrecuperables pero causará problemas de conexión intermitentes, como lentitud o frecuentes quitar de la conexión, controlar el `ConnectionSlow` eventos.</span><span class="sxs-lookup"><span data-stu-id="7e3af-261">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="7e3af-262">Para obtener más información, consulte [comprensión y control de eventos de duración de conexión en SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="7e3af-262">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="7e3af-263">Cómo controlar los errores</span><span class="sxs-lookup"><span data-stu-id="7e3af-263">How to handle errors</span></span>

<span data-ttu-id="7e3af-264">Si no habilita explícitamente los mensajes de error detallados en el servidor, el objeto de excepción que SignalR devuelve después de un error contiene información mínima sobre el error.</span><span class="sxs-lookup"><span data-stu-id="7e3af-264">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="7e3af-265">Por ejemplo, si una llamada a `newContosoChatMessage` se produce un error, el mensaje de error en el objeto de error contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" enviar mensajes de error detallados a los clientes de producción no se recomienda por motivos de seguridad, pero si desea permitir que los mensajes de error detallados para solucionar problemas, use el código siguiente en el servidor.</span><span class="sxs-lookup"><span data-stu-id="7e3af-265">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="7e3af-266">Para controlar los errores que genera SignalR, puede agregar un controlador para el `Error` evento en el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="7e3af-266">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="7e3af-267">Para controlar los errores de las invocaciones de método, encapsule el código en un bloque try-catch.</span><span class="sxs-lookup"><span data-stu-id="7e3af-267">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="7e3af-268">Cómo habilitar el registro del lado cliente</span><span class="sxs-lookup"><span data-stu-id="7e3af-268">How to enable client-side logging</span></span>

<span data-ttu-id="7e3af-269">Para habilitar el registro del lado cliente, establezca el `TraceLevel` y `TraceWriter` las propiedades del objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="7e3af-269">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="7e3af-270">Ejemplos de código de aplicación de consola para que el servidor puede llamar a métodos del cliente, Silverlight y WPF</span><span class="sxs-lookup"><span data-stu-id="7e3af-270">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="7e3af-271">Los ejemplos de código mostrados anteriormente para definir métodos de cliente que el servidor puede llamar a aplican a los clientes de WinRT.</span><span class="sxs-lookup"><span data-stu-id="7e3af-271">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="7e3af-272">Los ejemplos siguientes muestran el código equivalente para WPF, Silverlight y los clientes de aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="7e3af-272">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="7e3af-273">Métodos sin parámetros</span><span class="sxs-lookup"><span data-stu-id="7e3af-273">Methods without parameters</span></span>

**<span data-ttu-id="7e3af-274">Código de cliente WPF para el método que se llama desde el servidor sin parámetros</span><span class="sxs-lookup"><span data-stu-id="7e3af-274">WPF client code for method called from server without parameters</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**<span data-ttu-id="7e3af-275">Código de cliente de Silverlight para el método que se llama desde el servidor sin parámetros</span><span class="sxs-lookup"><span data-stu-id="7e3af-275">Silverlight client code for method called from server without parameters</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**<span data-ttu-id="7e3af-276">Código de cliente de aplicación de consola para el método se llama desde el servidor sin parámetros</span><span class="sxs-lookup"><span data-stu-id="7e3af-276">Console application client code for method called from server without parameters</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="7e3af-277">Métodos con parámetros, especificando los tipos de parámetro</span><span class="sxs-lookup"><span data-stu-id="7e3af-277">Methods with parameters, specifying the parameter types</span></span>

**<span data-ttu-id="7e3af-278">Código de cliente WPF para un método llamado desde el servidor con un parámetro</span><span class="sxs-lookup"><span data-stu-id="7e3af-278">WPF client code for a method called from server with a parameter</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**<span data-ttu-id="7e3af-279">Código de cliente de Silverlight para un método llamado desde el servidor con un parámetro</span><span class="sxs-lookup"><span data-stu-id="7e3af-279">Silverlight client code for a method called from server with a parameter</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**<span data-ttu-id="7e3af-280">Código de cliente de aplicación de consola para un método se llama desde el servidor con un parámetro</span><span class="sxs-lookup"><span data-stu-id="7e3af-280">Console application client code for a method called from server with a parameter</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="7e3af-281">Métodos con parámetros, que se especifiquen objetos dinámicos para los parámetros</span><span class="sxs-lookup"><span data-stu-id="7e3af-281">Methods with parameters, specifying dynamic objects for the parameters</span></span>

**<span data-ttu-id="7e3af-282">Código de cliente WPF para un método llamado desde el servidor con un parámetro, con un objeto dinámico para el parámetro</span><span class="sxs-lookup"><span data-stu-id="7e3af-282">WPF client code for a method called from server with a parameter, using a dynamic object for the parameter</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**<span data-ttu-id="7e3af-283">Código de cliente de Silverlight para un método llamado desde el servidor con un parámetro, con un objeto dinámico para el parámetro</span><span class="sxs-lookup"><span data-stu-id="7e3af-283">Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**<span data-ttu-id="7e3af-284">Código de cliente de aplicación de consola para un método se llama desde el servidor con un parámetro, con un objeto dinámico para el parámetro</span><span class="sxs-lookup"><span data-stu-id="7e3af-284">Console application client code for a method called from server with a parameter, using a dynamic object for the parameter</span></span>**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
