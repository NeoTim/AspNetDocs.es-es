---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: Guía de la API de ASP.NET SignalR Hubs - Cliente .NET (C- ) Microsoft Docs
author: bradygaster
description: Este documento proporciona una introducción al uso de la API de Concentradores para SignalR versión 2 en clientes .NET, como Windows Store (WinRT), WPF, Silverlight y contras...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: d3536f1c15cd7dad7cd660becf0577e5c131f707
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675932"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-c"></a><span data-ttu-id="673c9-103">Guía de la API de ASP.NET SignalR Hubs- Cliente .NET (C-)</span><span class="sxs-lookup"><span data-stu-id="673c9-103">ASP.NET SignalR Hubs API Guide - .NET Client (C#)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="673c9-104">Este documento proporciona una introducción al uso de la API de Hubs para SignalR versión 2 en clientes .NET, como Windows Store (WinRT), WPF, Silverlight y aplicaciones de consola.</span><span class="sxs-lookup"><span data-stu-id="673c9-104">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
>
> <span data-ttu-id="673c9-105">La API de SignalR Hubs le permite realizar llamadas a procedimientos remotos (RPC) desde un servidor a clientes conectados y desde clientes al servidor.</span><span class="sxs-lookup"><span data-stu-id="673c9-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="673c9-106">En el código de servidor, se definen métodos a los que pueden llamar los clientes y se llama a métodos que se ejecutan en el cliente.</span><span class="sxs-lookup"><span data-stu-id="673c9-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="673c9-107">En el código de cliente, se definen métodos a los que se puede llamar desde el servidor y se llama a métodos que se ejecutan en el servidor.</span><span class="sxs-lookup"><span data-stu-id="673c9-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="673c9-108">SignalR se encarga de todas las tuberías de cliente a servidor por usted.</span><span class="sxs-lookup"><span data-stu-id="673c9-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="673c9-109">SignalR también ofrece una API de nivel inferior denominada Conexiones persistentes.</span><span class="sxs-lookup"><span data-stu-id="673c9-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="673c9-110">Para obtener una introducción a SignalR, concentradores y conexiones persistentes, o para un tutorial que muestra cómo crear una aplicación de SignalR completa, vea [SignalR - Introducción](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="673c9-110">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="673c9-111">Versiones de software utilizadas en este tema</span><span class="sxs-lookup"><span data-stu-id="673c9-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="673c9-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="673c9-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="673c9-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="673c9-113">.NET 4.5</span></span>
> - <span data-ttu-id="673c9-114">SignalR versión 2</span><span class="sxs-lookup"><span data-stu-id="673c9-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="673c9-115">Versiones anteriores de este tema</span><span class="sxs-lookup"><span data-stu-id="673c9-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="673c9-116">Para obtener información acerca de las versiones anteriores de SignalR, vea [Versiones anteriores](../older-versions/index.md)de SignalR .</span><span class="sxs-lookup"><span data-stu-id="673c9-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="673c9-117">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="673c9-117">Questions and comments</span></span>
>
> <span data-ttu-id="673c9-118">Por favor, deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="673c9-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="673c9-119">Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el [ASP.NET foro](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) de SignalR o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="673c9-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="673c9-120">Información general</span><span class="sxs-lookup"><span data-stu-id="673c9-120">Overview</span></span>

<span data-ttu-id="673c9-121">Este documento contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="673c9-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="673c9-122">Configuración del cliente</span><span class="sxs-lookup"><span data-stu-id="673c9-122">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="673c9-123">Cómo establecer una conexión</span><span class="sxs-lookup"><span data-stu-id="673c9-123">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="673c9-124">Conexiones entre dominios de clientes de Silverlight</span><span class="sxs-lookup"><span data-stu-id="673c9-124">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="673c9-125">Cómo configurar la conexión</span><span class="sxs-lookup"><span data-stu-id="673c9-125">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="673c9-126">Cómo establecer el número máximo de conexiones simultáneas en los clientes de WPFWPF</span><span class="sxs-lookup"><span data-stu-id="673c9-126">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="673c9-127">Cómo especificar parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="673c9-127">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="673c9-128">Cómo especificar el método de transporte</span><span class="sxs-lookup"><span data-stu-id="673c9-128">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="673c9-129">Cómo especificar encabezados HTTP</span><span class="sxs-lookup"><span data-stu-id="673c9-129">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="673c9-130">Cómo especificar certificados de cliente</span><span class="sxs-lookup"><span data-stu-id="673c9-130">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="673c9-131">Cómo crear el proxy de Hub</span><span class="sxs-lookup"><span data-stu-id="673c9-131">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="673c9-132">Cómo definir métodos en el cliente a los que el servidor puede llamar</span><span class="sxs-lookup"><span data-stu-id="673c9-132">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="673c9-133">Métodos sin parámetros</span><span class="sxs-lookup"><span data-stu-id="673c9-133">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="673c9-134">Métodos con parámetros, especificando tipos de parámetros</span><span class="sxs-lookup"><span data-stu-id="673c9-134">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="673c9-135">Métodos con parámetros, especificando objetos dinámicos para los parámetros</span><span class="sxs-lookup"><span data-stu-id="673c9-135">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="673c9-136">Cómo quitar un controlador</span><span class="sxs-lookup"><span data-stu-id="673c9-136">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="673c9-137">Cómo llamar a métodos de servidor desde el cliente</span><span class="sxs-lookup"><span data-stu-id="673c9-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="673c9-138">Cómo controlar los eventos de duración de la conexión</span><span class="sxs-lookup"><span data-stu-id="673c9-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="673c9-139">Cómo manejar los errores</span><span class="sxs-lookup"><span data-stu-id="673c9-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="673c9-140">Cómo habilitar el registro del lado cliente</span><span class="sxs-lookup"><span data-stu-id="673c9-140">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="673c9-141">EJEMPLOs de código de aplicación de wpf, Silverlight y consola para métodos de cliente a los que el servidor puede llamar</span><span class="sxs-lookup"><span data-stu-id="673c9-141">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="673c9-142">Para ver un ejemplo de proyectos de cliente de .NET, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="673c9-142">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="673c9-143">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) en GitHub.com (WinRT, Silverlight, ejemplos de aplicaciones de consola).</span><span class="sxs-lookup"><span data-stu-id="673c9-143">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="673c9-144">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) en GitHub.com (ejemplo DEWPF).</span><span class="sxs-lookup"><span data-stu-id="673c9-144">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="673c9-145">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) en GitHub.com (ejemplo de aplicación de consola).</span><span class="sxs-lookup"><span data-stu-id="673c9-145">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="673c9-146">Para obtener documentación sobre cómo programar el servidor o los clientes JavaScript, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="673c9-146">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="673c9-147">Guía de la API de SignalR Hubs - Servidor</span><span class="sxs-lookup"><span data-stu-id="673c9-147">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="673c9-148">SignalR Hubs API Guide - Cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="673c9-148">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)

<span data-ttu-id="673c9-149">Los vínculos a temas de Referencia de API se vinculan a la versión 4.5 de la API.</span><span class="sxs-lookup"><span data-stu-id="673c9-149">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="673c9-150">Si usa .NET 4, consulte [la versión de .NET 4 de los temas de la API.](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="673c9-150">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="673c9-151">Configuración del cliente</span><span class="sxs-lookup"><span data-stu-id="673c9-151">Client setup</span></span>

<span data-ttu-id="673c9-152">Instale el paquete NuGet [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) (no el paquete [Microsoft.AspNet.SignalR).](http://nuget.org/packages/microsoft.aspnet.signalr)</span><span class="sxs-lookup"><span data-stu-id="673c9-152">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="673c9-153">Este paquete admite WinRT, Silverlight, WPF, aplicación de consola y clientes de Windows Phone, tanto para .NET 4 como para .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="673c9-153">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="673c9-154">Si la versión de SignalR que tiene en el cliente es diferente de la versión que tiene en el servidor, SignalR a menudo puede adaptarse a la diferencia.</span><span class="sxs-lookup"><span data-stu-id="673c9-154">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="673c9-155">Por ejemplo, un servidor que ejecute SignalR versión 2 admitirá clientes que tengan 1.1.x instalado, así como clientes que tengan instalada la versión 2.</span><span class="sxs-lookup"><span data-stu-id="673c9-155">For example, a server running SignalR version 2 will support clients that have 1.1.x installed as well as clients that have version 2 installed.</span></span> <span data-ttu-id="673c9-156">Si la diferencia entre la versión en el servidor y la versión en el cliente es `InvalidOperationException` demasiado grande, o si el cliente es más reciente que el servidor, SignalR produce una excepción cuando el cliente intenta establecer una conexión.</span><span class="sxs-lookup"><span data-stu-id="673c9-156">If the difference between the version on the server and the version on the client is too great, or if the client is newer than the server, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="673c9-157">El mensaje de`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`error es " ".</span><span class="sxs-lookup"><span data-stu-id="673c9-157">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="673c9-158">Cómo establecer una conexión</span><span class="sxs-lookup"><span data-stu-id="673c9-158">How to establish a connection</span></span>

<span data-ttu-id="673c9-159">Antes de poder establecer una conexión, `HubConnection` debe crear un objeto y crear un proxy.</span><span class="sxs-lookup"><span data-stu-id="673c9-159">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="673c9-160">Para establecer la conexión, llame al `Start` método en el `HubConnection` objeto.</span><span class="sxs-lookup"><span data-stu-id="673c9-160">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,5)]

> [!NOTE]
> <span data-ttu-id="673c9-161">Para los clientes de JavaScript debe registrar `Start` al menos un controlador de eventos antes de llamar al método para establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="673c9-161">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="673c9-162">Esto no es necesario para los clientes .NET.</span><span class="sxs-lookup"><span data-stu-id="673c9-162">This is not necessary for .NET clients.</span></span> <span data-ttu-id="673c9-163">Para los clientes JavaScript, el código proxy generado crea automáticamente servidores proxy para todos los concentradores que existen en el servidor y el registro de un controlador es la forma en que se indica qué concentradores piensa usar el cliente.</span><span class="sxs-lookup"><span data-stu-id="673c9-163">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="673c9-164">Pero para un cliente de .NET se crean servidores proxy de concentrador manualmente, por lo que SignalR supone que va a usar cualquier concentrador para el que cree un proxy.</span><span class="sxs-lookup"><span data-stu-id="673c9-164">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>

<span data-ttu-id="673c9-165">El código de ejemplo utiliza la dirección URL predeterminada "/signalr" para conectarse al servicio SignalR.</span><span class="sxs-lookup"><span data-stu-id="673c9-165">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="673c9-166">Para obtener información sobre cómo especificar una dirección URL base diferente, consulte ASP.NET Guía de la API de [SignalR Hubs - Servidor - La dirección URL de /signalr](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="673c9-166">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="673c9-167">El `Start` método se ejecuta de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="673c9-167">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="673c9-168">Para asegurarse de que las líneas de código posteriores `await` no se ejecutan hasta después de establecer la conexión, utilice en un método asincrónico ASP.NET 4.5 o `.Wait()` en un método sincrónico.</span><span class="sxs-lookup"><span data-stu-id="673c9-168">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="673c9-169">No usar `.Wait()` en un cliente WinRT.</span><span class="sxs-lookup"><span data-stu-id="673c9-169">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="673c9-170">Conexiones entre dominios de clientes de Silverlight</span><span class="sxs-lookup"><span data-stu-id="673c9-170">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="673c9-171">Para obtener información sobre cómo habilitar conexiones entre dominios desde clientes de Silverlight, vea [Hacer que un servicio esté disponible en todos los límites de dominio](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="673c9-171">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="673c9-172">Cómo configurar la conexión</span><span class="sxs-lookup"><span data-stu-id="673c9-172">How to configure the connection</span></span>

<span data-ttu-id="673c9-173">Antes de establecer una conexión, puede especificar cualquiera de las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="673c9-173">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="673c9-174">Límite de conexiones simultáneas.</span><span class="sxs-lookup"><span data-stu-id="673c9-174">Concurrent connections limit.</span></span>
- <span data-ttu-id="673c9-175">Parámetros de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="673c9-175">Query string parameters.</span></span>
- <span data-ttu-id="673c9-176">El método de transporte.</span><span class="sxs-lookup"><span data-stu-id="673c9-176">The transport method.</span></span>
- <span data-ttu-id="673c9-177">Encabezados HTTP.</span><span class="sxs-lookup"><span data-stu-id="673c9-177">HTTP headers.</span></span>
- <span data-ttu-id="673c9-178">Certificados de cliente.</span><span class="sxs-lookup"><span data-stu-id="673c9-178">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="673c9-179">Cómo establecer el número máximo de conexiones simultáneas en los clientes de WPFWPF</span><span class="sxs-lookup"><span data-stu-id="673c9-179">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="673c9-180">En los clientes de WPFWPF, es posible que tenga que aumentar el número máximo de conexiones simultáneas a partir de su valor predeterminado de 2.</span><span class="sxs-lookup"><span data-stu-id="673c9-180">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="673c9-181">El valor recomendado es 10.</span><span class="sxs-lookup"><span data-stu-id="673c9-181">The recommended value is 10.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=5)]

<span data-ttu-id="673c9-182">Para obtener más información, vea [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="673c9-182">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="673c9-183">Cómo especificar parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="673c9-183">How to specify query string parameters</span></span>

<span data-ttu-id="673c9-184">Si desea enviar datos al servidor cuando el cliente se conecta, puede agregar parámetros de cadena de consulta al objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="673c9-184">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="673c9-185">En el ejemplo siguiente se muestra cómo establecer un parámetro de cadena de consulta en el código de cliente.</span><span class="sxs-lookup"><span data-stu-id="673c9-185">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="673c9-186">En el ejemplo siguiente se muestra cómo leer un parámetro de cadena de consulta en el código del servidor.</span><span class="sxs-lookup"><span data-stu-id="673c9-186">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="673c9-187">Cómo especificar el método de transporte</span><span class="sxs-lookup"><span data-stu-id="673c9-187">How to specify the transport method</span></span>

<span data-ttu-id="673c9-188">Como parte del proceso de conexión, un cliente de SignalR normalmente negocia con el servidor para determinar el mejor transporte que es compatible con el servidor y el cliente.</span><span class="sxs-lookup"><span data-stu-id="673c9-188">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="673c9-189">Si ya sabe qué transporte desea utilizar, puede omitir este proceso de negociación.</span><span class="sxs-lookup"><span data-stu-id="673c9-189">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="673c9-190">Para especificar el método de transporte, pase un objeto de transporte al método Start.</span><span class="sxs-lookup"><span data-stu-id="673c9-190">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="673c9-191">En el ejemplo siguiente se muestra cómo especificar el método de transporte en el código de cliente.</span><span class="sxs-lookup"><span data-stu-id="673c9-191">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=5)]

<span data-ttu-id="673c9-192">El [espacio de nombres Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) incluye las siguientes clases que puede usar para especificar el transporte.</span><span class="sxs-lookup"><span data-stu-id="673c9-192">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="673c9-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="673c9-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="673c9-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="673c9-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="673c9-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponible solo cuando el servidor y el cliente usan .NET 4.5.)</span><span class="sxs-lookup"><span data-stu-id="673c9-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="673c9-196">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (elige automáticamente el mejor transporte que admite tanto el cliente como el servidor.</span><span class="sxs-lookup"><span data-stu-id="673c9-196">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="673c9-197">Este es el transporte predeterminado.</span><span class="sxs-lookup"><span data-stu-id="673c9-197">This is the default transport.</span></span> <span data-ttu-id="673c9-198">Pasar esto al `Start` método tiene el mismo efecto que no pasar nada.)</span><span class="sxs-lookup"><span data-stu-id="673c9-198">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="673c9-199">El transporte ForeverFrame no se incluye en esta lista porque solo lo utilizan los exploradores.</span><span class="sxs-lookup"><span data-stu-id="673c9-199">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="673c9-200">Para obtener información acerca de cómo comprobar el método de transporte en el código del servidor, consulte ASP.NET Guía de la API de [SignalR Hubs - Servidor - Cómo obtener información sobre el cliente desde la propiedad Context](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="673c9-200">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="673c9-201">Para obtener más información acerca de los transportes y las reservaciones, vea [Introducción a SignalR - Transportes y reserva .](../getting-started/introduction-to-signalr.md#transports)</span><span class="sxs-lookup"><span data-stu-id="673c9-201">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="673c9-202">Cómo especificar encabezados HTTP</span><span class="sxs-lookup"><span data-stu-id="673c9-202">How to specify HTTP headers</span></span>

<span data-ttu-id="673c9-203">Para establecer encabezados HTTP, utilice la `Headers` propiedad del objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="673c9-203">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="673c9-204">En el ejemplo siguiente se muestra cómo agregar un encabezado HTTP.</span><span class="sxs-lookup"><span data-stu-id="673c9-204">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="673c9-205">Cómo especificar certificados de cliente</span><span class="sxs-lookup"><span data-stu-id="673c9-205">How to specify client certificates</span></span>

<span data-ttu-id="673c9-206">Para agregar certificados de `AddClientCertificate` cliente, utilice el método en el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="673c9-206">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="673c9-207">Cómo crear el proxy de Hub</span><span class="sxs-lookup"><span data-stu-id="673c9-207">How to create the Hub proxy</span></span>

<span data-ttu-id="673c9-208">Para definir métodos en el cliente que un concentrador puede llamar desde el servidor e invocar métodos `CreateHubProxy` en un concentrador en el servidor, cree un proxy para el concentrador llamando al objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="673c9-208">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="673c9-209">La cadena a `CreateHubProxy` la que se pasa es el nombre de `HubName` la clase Hub o el nombre especificado por el atributo si se usó uno en el servidor.</span><span class="sxs-lookup"><span data-stu-id="673c9-209">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="673c9-210">La coincidencia de nombres no distingue mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="673c9-210">Name matching is case-insensitive.</span></span>

<span data-ttu-id="673c9-211">**Clase Hub en servidor**</span><span class="sxs-lookup"><span data-stu-id="673c9-211">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="673c9-212">**Crear proxy de cliente para la clase Hub**</span><span class="sxs-lookup"><span data-stu-id="673c9-212">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=3)]

<span data-ttu-id="673c9-213">Si decora sacada `HubName` la clase Hub con un atributo, use ese nombre.</span><span class="sxs-lookup"><span data-stu-id="673c9-213">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="673c9-214">**Clase Hub en servidor**</span><span class="sxs-lookup"><span data-stu-id="673c9-214">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="673c9-215">**Crear proxy de cliente para la clase Hub**</span><span class="sxs-lookup"><span data-stu-id="673c9-215">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=3)]

<span data-ttu-id="673c9-216">Si llama `HubConnection.CreateHubProxy` varias veces `hubName`con el mismo `IHubProxy` , obtendrá el mismo objeto almacenado en caché.</span><span class="sxs-lookup"><span data-stu-id="673c9-216">If you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="673c9-217">Cómo definir métodos en el cliente a los que el servidor puede llamar</span><span class="sxs-lookup"><span data-stu-id="673c9-217">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="673c9-218">Para definir un método al que el servidor `On` puede llamar, use el método del proxy para registrar un controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="673c9-218">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="673c9-219">La coincidencia de nombres de método no distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="673c9-219">Method name matching is case-insensitive.</span></span> <span data-ttu-id="673c9-220">Por `Clients.All.UpdateStockPrice` ejemplo, en el `updateStockPrice` `updatestockprice`servidor `UpdateStockPrice` se ejecutará , , o en el cliente.</span><span class="sxs-lookup"><span data-stu-id="673c9-220">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="673c9-221">Diferentes plataformas de cliente tienen requisitos diferentes para escribir código de método para actualizar la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="673c9-221">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="673c9-222">Los ejemplos que se muestran son para clientes de WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="673c9-222">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="673c9-223">WPFWPF, Silverlight y ejemplos de aplicaciones de consola se proporcionan en [una sección independiente más adelante en este tema.](#wpfsl)</span><span class="sxs-lookup"><span data-stu-id="673c9-223">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="673c9-224">Métodos sin parámetros</span><span class="sxs-lookup"><span data-stu-id="673c9-224">Methods without parameters</span></span>

<span data-ttu-id="673c9-225">Si el método que está controlando no tiene parámetros, utilice la sobrecarga no genérica del `On` método:</span><span class="sxs-lookup"><span data-stu-id="673c9-225">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="673c9-226">**Código de servidor que llama al método cliente sin parámetros**</span><span class="sxs-lookup"><span data-stu-id="673c9-226">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="673c9-227">**Código de cliente WinRT para el método al que se llama desde el servidor sin parámetros[(vea ejemplos de WPF y Silverlight más adelante en este tema)](#wpfsl)**</span><span class="sxs-lookup"><span data-stu-id="673c9-227">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="673c9-228">Métodos con parámetros, especificando los tipos de parámetros</span><span class="sxs-lookup"><span data-stu-id="673c9-228">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="673c9-229">Si el método que está controlando tiene parámetros, especifique `On` los tipos de los parámetros como los tipos genéricos del método.</span><span class="sxs-lookup"><span data-stu-id="673c9-229">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="673c9-230">Hay sobrecargas genéricas `On` del método para permitirle especificar hasta 8 parámetros (4 en Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="673c9-230">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="673c9-231">En el ejemplo siguiente, se `UpdateStockPrice` envía un parámetro al método.</span><span class="sxs-lookup"><span data-stu-id="673c9-231">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="673c9-232">**Método de cliente de llamada de código de servidor con un parámetro**</span><span class="sxs-lookup"><span data-stu-id="673c9-232">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="673c9-233">**La clase Stock utilizada para el parámetro**</span><span class="sxs-lookup"><span data-stu-id="673c9-233">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="673c9-234">**Código de cliente WinRT para un método al que se llama desde el servidor con un parámetro[(vea WPF y ejemplos de Silverlight más adelante en este tema)](#wpfsl)**</span><span class="sxs-lookup"><span data-stu-id="673c9-234">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="673c9-235">Métodos con parámetros, especificando objetos dinámicos para los parámetros</span><span class="sxs-lookup"><span data-stu-id="673c9-235">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="673c9-236">Como alternativa a la especificación de `On` parámetros como tipos genéricos del método, puede especificar parámetros como objetos dinámicos:</span><span class="sxs-lookup"><span data-stu-id="673c9-236">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="673c9-237">**Método de cliente de llamada de código de servidor con un parámetro**</span><span class="sxs-lookup"><span data-stu-id="673c9-237">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="673c9-238">**La clase Stock utilizada para el parámetro**</span><span class="sxs-lookup"><span data-stu-id="673c9-238">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="673c9-239">**Código de cliente WinRT para un método al que se llama desde el servidor con un parámetro, mediante un objeto dinámico para el parámetro[(consulte ejemplos de WPFwpf y Silverlight más adelante en este tema)](#wpfsl)**</span><span class="sxs-lookup"><span data-stu-id="673c9-239">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="673c9-240">Cómo quitar un controlador</span><span class="sxs-lookup"><span data-stu-id="673c9-240">How to remove a handler</span></span>

<span data-ttu-id="673c9-241">Para quitar un controlador, llame a su `Dispose` método.</span><span class="sxs-lookup"><span data-stu-id="673c9-241">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="673c9-242">**Código de cliente para un método llamado desde el servidor**</span><span class="sxs-lookup"><span data-stu-id="673c9-242">**Client code for a method called from server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="673c9-243">**Código de cliente para quitar el controlador**</span><span class="sxs-lookup"><span data-stu-id="673c9-243">**Client code to remove the handler**</span></span>

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="673c9-244">Cómo llamar a métodos de servidor desde el cliente</span><span class="sxs-lookup"><span data-stu-id="673c9-244">How to call server methods from the client</span></span>

<span data-ttu-id="673c9-245">Para llamar a un método `Invoke` en el servidor, utilice el método en el proxy de concentrador.</span><span class="sxs-lookup"><span data-stu-id="673c9-245">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="673c9-246">Si el método de servidor no tiene ningún valor `Invoke` devuelto, utilice la sobrecarga no genérica del método.</span><span class="sxs-lookup"><span data-stu-id="673c9-246">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="673c9-247">**Código de servidor para un método que no tiene ningún valor devuelto**</span><span class="sxs-lookup"><span data-stu-id="673c9-247">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="673c9-248">**Código de cliente que llama a un método que no tiene ningún valor devuelto**</span><span class="sxs-lookup"><span data-stu-id="673c9-248">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="673c9-249">Si el método de servidor tiene un valor devuelto, `Invoke` especifique el tipo de valor devuelto como el tipo genérico del método.</span><span class="sxs-lookup"><span data-stu-id="673c9-249">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="673c9-250">**Código de servidor para un método que tiene un valor devuelto y toma un parámetro de tipo complejo**</span><span class="sxs-lookup"><span data-stu-id="673c9-250">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="673c9-251">**La clase Stock utilizada para el parámetro y el valor devuelto**</span><span class="sxs-lookup"><span data-stu-id="673c9-251">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="673c9-252">**Código de cliente que llama a un método que tiene un valor devuelto y toma un parámetro de tipo complejo, en un método asincrónico ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="673c9-252">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="673c9-253">**Código de cliente que llama a un método que tiene un valor devuelto y toma un parámetro de tipo complejo, en un método sincrónico**</span><span class="sxs-lookup"><span data-stu-id="673c9-253">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="673c9-254">El `Invoke` método se ejecuta de `Task` forma asincrónica y devuelve un objeto.</span><span class="sxs-lookup"><span data-stu-id="673c9-254">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="673c9-255">Si no especifica `await` o `.Wait()`, la siguiente línea de código se ejecutará antes de que el método que invoque haya terminado de ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="673c9-255">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="673c9-256">Cómo controlar los eventos de duración de la conexión</span><span class="sxs-lookup"><span data-stu-id="673c9-256">How to handle connection lifetime events</span></span>

<span data-ttu-id="673c9-257">SignalR proporciona los siguientes eventos de duración de conexión que puede controlar:</span><span class="sxs-lookup"><span data-stu-id="673c9-257">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="673c9-258">`Received`: Se genera cuando se reciben datos en la conexión.</span><span class="sxs-lookup"><span data-stu-id="673c9-258">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="673c9-259">Proporciona los datos recibidos.</span><span class="sxs-lookup"><span data-stu-id="673c9-259">Provides the received data.</span></span>
- <span data-ttu-id="673c9-260">`ConnectionSlow`: Se genera cuando el cliente detecta una conexión lenta o que se cae con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="673c9-260">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="673c9-261">`Reconnecting`: Se genera cuando el transporte subyacente comienza a volver a conectarse.</span><span class="sxs-lookup"><span data-stu-id="673c9-261">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="673c9-262">`Reconnected`: Se genera cuando el transporte subyacente se ha vuelto a conectar.</span><span class="sxs-lookup"><span data-stu-id="673c9-262">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="673c9-263">`StateChanged`: se genera cuando cambia el estado de conexión.</span><span class="sxs-lookup"><span data-stu-id="673c9-263">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="673c9-264">Proporciona el estado antiguo y el nuevo estado.</span><span class="sxs-lookup"><span data-stu-id="673c9-264">Provides the old state and the new state.</span></span> <span data-ttu-id="673c9-265">Para obtener información acerca de los valores de estado de conexión, vea [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="673c9-265">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="673c9-266">`Closed`: Se genera cuando la conexión se ha desconectado.</span><span class="sxs-lookup"><span data-stu-id="673c9-266">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="673c9-267">Por ejemplo, si desea mostrar mensajes de advertencia para errores que no son fatales pero causan problemas `ConnectionSlow` de conexión intermitentes, como lentitud o caída frecuente de la conexión, controle el evento.</span><span class="sxs-lookup"><span data-stu-id="673c9-267">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="673c9-268">Para obtener más información, consulte Descripción y control de eventos de duración de [conexión en SignalR](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="673c9-268">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="673c9-269">Cómo manejar los errores</span><span class="sxs-lookup"><span data-stu-id="673c9-269">How to handle errors</span></span>

<span data-ttu-id="673c9-270">Si no habilita explícitamente los mensajes de error detallados en el servidor, el objeto de excepción que SignalR devuelve después de un error contiene información mínima sobre el error.</span><span class="sxs-lookup"><span data-stu-id="673c9-270">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="673c9-271">Por ejemplo, si `newContosoChatMessage` se produce un error en una`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`llamada, el mensaje de error del objeto de error contiene " " Enviar mensajes de error detallados a los clientes en producción no se recomienda por motivos de seguridad, pero si desea habilitar mensajes de error detallados para solucionar problemas, utilice el código siguiente en el servidor.</span><span class="sxs-lookup"><span data-stu-id="673c9-271">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="673c9-272">Para controlar los errores que SignalR genera, `Error` puede agregar un controlador para el evento en el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="673c9-272">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="673c9-273">Para controlar los errores de invocaciones de método, ajuste el código en un bloque try-catch.</span><span class="sxs-lookup"><span data-stu-id="673c9-273">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="673c9-274">Cómo habilitar el registro del lado cliente</span><span class="sxs-lookup"><span data-stu-id="673c9-274">How to enable client-side logging</span></span>

<span data-ttu-id="673c9-275">Para habilitar el registro del `TraceLevel` `TraceWriter` lado cliente, establezca las propiedades y en el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="673c9-275">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=3-4)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="673c9-276">EJEMPLOs de código de aplicación de wpf, Silverlight y consola para métodos de cliente a los que el servidor puede llamar</span><span class="sxs-lookup"><span data-stu-id="673c9-276">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="673c9-277">Los ejemplos de código mostrados anteriormente para definir métodos de cliente a los que el servidor puede llamar se aplican a los clientes de WinRT.</span><span class="sxs-lookup"><span data-stu-id="673c9-277">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="673c9-278">En los ejemplos siguientes se muestra el código equivalente para WPFWPF, Silverlight y clientes de aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="673c9-278">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="673c9-279">Métodos sin parámetros</span><span class="sxs-lookup"><span data-stu-id="673c9-279">Methods without parameters</span></span>

<span data-ttu-id="673c9-280">**Código de cliente WPF para el método llamado desde el servidor sin parámetros**</span><span class="sxs-lookup"><span data-stu-id="673c9-280">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="673c9-281">**Código de cliente Silverlight para el método llamado desde el servidor sin parámetros**</span><span class="sxs-lookup"><span data-stu-id="673c9-281">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="673c9-282">**Código de cliente de aplicación de consola para el método llamado desde el servidor sin parámetros**</span><span class="sxs-lookup"><span data-stu-id="673c9-282">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="673c9-283">Métodos con parámetros, especificando los tipos de parámetros</span><span class="sxs-lookup"><span data-stu-id="673c9-283">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="673c9-284">**Código de cliente WPF para un método al que se llama desde el servidor con un parámetro**</span><span class="sxs-lookup"><span data-stu-id="673c9-284">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="673c9-285">**Código de cliente Silverlight para un método llamado desde el servidor con un parámetro**</span><span class="sxs-lookup"><span data-stu-id="673c9-285">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="673c9-286">**Código de cliente de aplicación de consola para un método al que se llama desde el servidor con un parámetro**</span><span class="sxs-lookup"><span data-stu-id="673c9-286">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="673c9-287">Métodos con parámetros, especificando objetos dinámicos para los parámetros</span><span class="sxs-lookup"><span data-stu-id="673c9-287">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="673c9-288">**Código de cliente WPF para un método al que se llama desde el servidor con un parámetro, mediante un objeto dinámico para el parámetro**</span><span class="sxs-lookup"><span data-stu-id="673c9-288">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="673c9-289">**Código de cliente Silverlight para un método al que se llama desde el servidor con un parámetro, utilizando un objeto dinámico para el parámetro**</span><span class="sxs-lookup"><span data-stu-id="673c9-289">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="673c9-290">**Código de cliente de aplicación de consola para un método al que se llama desde el servidor con un parámetro, utilizando un objeto dinámico para el parámetro**</span><span class="sxs-lookup"><span data-stu-id="673c9-290">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
