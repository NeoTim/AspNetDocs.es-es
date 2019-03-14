---
title: Interfaz web abierta para .NET (OWIN) con ASP.NET Core
author: ardalis
description: Descubra la manera en que ASP.NET Core es compatible con la interfaz web abierta para .NET (OWIN), que permite que las aplicaciones web se desacoplen de servidores web.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/18/2018
uid: fundamentals/owin
ms.openlocfilehash: 51982c7ebc4f66c2b0b73bf425d9ecbd0bf37826
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57065062"
---
# <a name="open-web-interface-for-net-owin-with-aspnet-core"></a><span data-ttu-id="3f3e4-103">Interfaz web abierta para .NET (OWIN) con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f3e4-103">Open Web Interface for .NET (OWIN) with ASP.NET Core</span></span>

<span data-ttu-id="3f3e4-104">Por [Steve Smith](https://ardalis.com/) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-104">By [Steve Smith](https://ardalis.com/) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3f3e4-105">ASP.NET Core es compatible con la interfaz web abierta para .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="3f3e4-105">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="3f3e4-106">OWIN permite que las aplicaciones web se desacoplen de los servidores web.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-106">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="3f3e4-107">Define una manera estándar para usar software intermedio en una canalización a fin de controlar las solicitudes y las respuestas asociadas.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-107">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="3f3e4-108">El software intermedio y las aplicaciones de ASP.NET Core pueden interoperar con aplicaciones, servidores y software intermedio basados en OWIN.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-108">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="3f3e4-109">OWIN proporciona una capa de desacoplamiento que permite que dos marcos de trabajo con modelos de objetos dispares se usen juntos.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-109">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="3f3e4-110">El paquete `Microsoft.AspNetCore.Owin` proporciona dos implementaciones del adaptador:</span><span class="sxs-lookup"><span data-stu-id="3f3e4-110">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>

* <span data-ttu-id="3f3e4-111">De ASP.NET Core a OWIN</span><span class="sxs-lookup"><span data-stu-id="3f3e4-111">ASP.NET Core to OWIN</span></span> 
* <span data-ttu-id="3f3e4-112">De OWIN a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f3e4-112">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="3f3e4-113">Esto permite que ASP.NET Core se hospede sobre un servidor/host compatible con OWIN, o bien que otros componentes compatibles con OWIN se ejecuten sobre ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-113">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="3f3e4-114">El uso de estos adaptadores conlleva un costo de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-114">Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="3f3e4-115">Las aplicaciones que solo usan componentes de ASP.NET Core no deben usar el paquete o adaptadores de `Microsoft.AspNetCore.Owin`.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-115">Apps using only ASP.NET Core components shouldn't use the `Microsoft.AspNetCore.Owin` package or adapters.</span></span>

<span data-ttu-id="3f3e4-116">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3f3e4-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-core-pipeline"></a><span data-ttu-id="3f3e4-117">Ejecución de software intermedio de OWIN en la canalización de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f3e4-117">Running OWIN middleware in the ASP.NET Core pipeline</span></span>

<span data-ttu-id="3f3e4-118">La compatibilidad con OWIN de ASP.NET Core se implementa como parte del paquete `Microsoft.AspNetCore.Owin`.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-118">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="3f3e4-119">Puede importar compatibilidad con OWIN en el proyecto mediante la instalación de este paquete.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-119">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="3f3e4-120">El software intermedio de OWIN cumple la [especificación de OWIN](http://owin.org/spec/spec/owin-1.0.0.html), que requiere una interfaz `Func<IDictionary<string, object>, Task>` y el establecimiento de determinadas claves (como `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="3f3e4-120">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="3f3e4-121">En el siguiente software intermedio simple de OWIN se muestra "Hello World":</span><span class="sxs-lookup"><span data-stu-id="3f3e4-121">The following simple OWIN middleware displays "Hello World":</span></span>

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

<span data-ttu-id="3f3e4-122">La firma de ejemplo devuelve un valor `Task` y acepta un valor `IDictionary<string, object>`, según los requisitos de OWIN.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-122">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="3f3e4-123">En el código siguiente se muestra cómo agregar el software intermedio `OwinHello` (mostrado arriba) a la canalización ASP.NET Core con el método de extensión `UseOwin`.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-123">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET Core pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="3f3e4-124">Puede configurar la realización de otras acciones en la canalización de OWIN.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-124">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="3f3e4-125">Los encabezados de respuesta solo deben modificarse antes de la primera escritura en la secuencia de respuesta.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-125">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="3f3e4-126">No se recomienda la realización de varias llamadas a `UseOwin` por motivos de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-126">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="3f3e4-127">Los componentes de OWIN funcionarán mejor si se agrupan.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-127">OWIN components will operate best if grouped together.</span></span>

```csharp
app.UseOwin(pipeline =>
{
    pipeline(async (next) =>
    {
        // do something before
        await OwinHello(new OwinEnvironment(HttpContext));
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-core-hosting-on-an-owin-based-server"></a><span data-ttu-id="3f3e4-128">Uso de hospedaje de ASP.NET Core en un servidor basado en OWIN</span><span class="sxs-lookup"><span data-stu-id="3f3e4-128">Using ASP.NET Core Hosting on an OWIN-based server</span></span>

<span data-ttu-id="3f3e4-129">Los servidores basados en OWIN pueden hospedar aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-129">OWIN-based servers can host ASP.NET Core apps.</span></span> <span data-ttu-id="3f3e4-130">Un servidor de este tipo es [Nowin](https://github.com/Bobris/Nowin), un servidor web de OWIN de .NET.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-130">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="3f3e4-131">En el ejemplo de este artículo, se ha incluido un proyecto que hace referencia a Nowin y lo usa para crear una interfaz `IServer` capaz de autohospedar ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-131">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="3f3e4-132">`IServer` es una interfaz que requiere una propiedad `Features` y un método `Start`.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-132">`IServer` is an interface that requires a `Features` property and a `Start` method.</span></span>

<span data-ttu-id="3f3e4-133">`Start` se encarga de configurar e iniciar el servidor, lo que en este caso se lleva a cabo mediante una serie de llamadas API fluidas que establecen direcciones analizadas desde IServerAddressesFeature.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-133">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="3f3e4-134">Tenga en cuenta que la configuración fluida de la variable `_builder` especifica que las solicitudes se controlarán mediante el valor `appFunc` definido anteriormente en el método.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-134">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="3f3e4-135">Se llama a este valor `Func` en cada solicitud para procesar las solicitudes entrantes.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-135">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="3f3e4-136">Agregaremos también una extensión `IWebHostBuilder` para que sea fácil de agregar y configurar el servidor Nowin.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-136">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

<span data-ttu-id="3f3e4-137">Después de hacer estas implementaciones, invoque la extensión en *Program.cs* para ejecutar una aplicación ASP.NET a través de este servidor personalizado:</span><span class="sxs-lookup"><span data-stu-id="3f3e4-137">With this in place, invoke the extension in *Program.cs* to run an ASP.NET Core app using this custom server:</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}
```

<span data-ttu-id="3f3e4-138">Obtenga más información sobre los [servidores de ASP.NET](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="3f3e4-138">Learn more about [ASP.NET Core Servers](xref:fundamentals/servers/index).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="3f3e4-139">Ejecución de ASP.NET Core en un servidor basado en OWIN y uso de su compatibilidad con WebSockets</span><span class="sxs-lookup"><span data-stu-id="3f3e4-139">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="3f3e4-140">Otro ejemplo de la manera en que ASP.NET Core puede aprovechar las características de los servidores basados en OWIN es el acceso a funciones como WebSockets.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-140">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="3f3e4-141">El servidor web de OWIN de .NET usado en el ejemplo anterior es compatible con WebSockets integrado, cuyas ventajas puede aprovechar la aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-141">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="3f3e4-142">En el ejemplo siguiente se muestra una aplicación web simple que admite WebSockets y devuelve todo lo que se envía al servidor a través de WebSockets.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-142">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

<span data-ttu-id="3f3e4-143">Este [ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) se ha configurado con el mismo `NowinServer` que el anterior; la única diferencia es la manera en que la aplicación está configurada en su método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-143">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="3f3e4-144">La aplicación se muestra mediante una prueba que usa un [cliente WebSocket simple](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en):</span><span class="sxs-lookup"><span data-stu-id="3f3e4-144">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Cliente de prueba de WebSocket](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="3f3e4-146">Entorno de OWIN</span><span class="sxs-lookup"><span data-stu-id="3f3e4-146">OWIN environment</span></span>

<span data-ttu-id="3f3e4-147">Puede construir un entorno de OWIN por medio de `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-147">You can construct an OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="3f3e4-148">Claves de OWIN</span><span class="sxs-lookup"><span data-stu-id="3f3e4-148">OWIN keys</span></span>

<span data-ttu-id="3f3e4-149">OWIN depende de un objeto `IDictionary<string,object>` para comunicar información mediante un intercambio de solicitud/respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-149">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="3f3e4-150">ASP.NET Core implementa las claves que se enumeran a continuación.</span><span class="sxs-lookup"><span data-stu-id="3f3e4-150">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="3f3e4-151">Vea las [extensiones de la especificación principal](http://owin.org/#spec) y las [directrices principales y claves comunes de OWIN](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="3f3e4-151">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="3f3e4-152">Datos de solicitud (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-152">Request data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="3f3e4-153">Key</span><span class="sxs-lookup"><span data-stu-id="3f3e4-153">Key</span></span>               | <span data-ttu-id="3f3e4-154">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-154">Value (type)</span></span> | <span data-ttu-id="3f3e4-155">Descripción</span><span class="sxs-lookup"><span data-stu-id="3f3e4-155">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="3f3e4-156">owin.RequestScheme</span><span class="sxs-lookup"><span data-stu-id="3f3e4-156">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="3f3e4-157">owin.RequestMethod</span><span class="sxs-lookup"><span data-stu-id="3f3e4-157">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="3f3e4-158">owin.RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="3f3e4-158">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="3f3e4-159">owin.RequestPath</span><span class="sxs-lookup"><span data-stu-id="3f3e4-159">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="3f3e4-160">owin.RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="3f3e4-160">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="3f3e4-161">owin.RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="3f3e4-161">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="3f3e4-162">owin.RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="3f3e4-162">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="3f3e4-163">owin.RequestBody</span><span class="sxs-lookup"><span data-stu-id="3f3e4-163">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="3f3e4-164">Datos de solicitud (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-164">Request data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="3f3e4-165">Key</span><span class="sxs-lookup"><span data-stu-id="3f3e4-165">Key</span></span>               | <span data-ttu-id="3f3e4-166">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-166">Value (type)</span></span> | <span data-ttu-id="3f3e4-167">Descripción</span><span class="sxs-lookup"><span data-stu-id="3f3e4-167">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="3f3e4-168">owin.RequestId</span><span class="sxs-lookup"><span data-stu-id="3f3e4-168">owin.RequestId</span></span> | `String` | <span data-ttu-id="3f3e4-169">Optional</span><span class="sxs-lookup"><span data-stu-id="3f3e4-169">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="3f3e4-170">Datos de respuesta (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-170">Response data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="3f3e4-171">Key</span><span class="sxs-lookup"><span data-stu-id="3f3e4-171">Key</span></span>               | <span data-ttu-id="3f3e4-172">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-172">Value (type)</span></span> | <span data-ttu-id="3f3e4-173">Descripción</span><span class="sxs-lookup"><span data-stu-id="3f3e4-173">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="3f3e4-174">owin.ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="3f3e4-174">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="3f3e4-175">Optional</span><span class="sxs-lookup"><span data-stu-id="3f3e4-175">Optional</span></span> |
| <span data-ttu-id="3f3e4-176">owin.ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="3f3e4-176">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="3f3e4-177">Optional</span><span class="sxs-lookup"><span data-stu-id="3f3e4-177">Optional</span></span> |
| <span data-ttu-id="3f3e4-178">owin.ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="3f3e4-178">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="3f3e4-179">owin.ResponseBody</span><span class="sxs-lookup"><span data-stu-id="3f3e4-179">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="3f3e4-180">Otros datos (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-180">Other data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="3f3e4-181">Key</span><span class="sxs-lookup"><span data-stu-id="3f3e4-181">Key</span></span>               | <span data-ttu-id="3f3e4-182">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-182">Value (type)</span></span> | <span data-ttu-id="3f3e4-183">Descripción</span><span class="sxs-lookup"><span data-stu-id="3f3e4-183">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="3f3e4-184">owin.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="3f3e4-184">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="3f3e4-185">owin.Version</span><span class="sxs-lookup"><span data-stu-id="3f3e4-185">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="3f3e4-186">Claves comunes</span><span class="sxs-lookup"><span data-stu-id="3f3e4-186">Common keys</span></span>

| <span data-ttu-id="3f3e4-187">Key</span><span class="sxs-lookup"><span data-stu-id="3f3e4-187">Key</span></span>               | <span data-ttu-id="3f3e4-188">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-188">Value (type)</span></span> | <span data-ttu-id="3f3e4-189">Descripción</span><span class="sxs-lookup"><span data-stu-id="3f3e4-189">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="3f3e4-190">ssl.ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="3f3e4-190">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="3f3e4-191">ssl.LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="3f3e4-191">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="3f3e4-192">server.RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="3f3e4-192">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="3f3e4-193">server.RemotePort</span><span class="sxs-lookup"><span data-stu-id="3f3e4-193">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="3f3e4-194">server.LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="3f3e4-194">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="3f3e4-195">server.LocalPort</span><span class="sxs-lookup"><span data-stu-id="3f3e4-195">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="3f3e4-196">server.IsLocal</span><span class="sxs-lookup"><span data-stu-id="3f3e4-196">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="3f3e4-197">server.OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="3f3e4-197">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="3f3e4-198">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="3f3e4-198">SendFiles v0.3.0</span></span>

| <span data-ttu-id="3f3e4-199">Key</span><span class="sxs-lookup"><span data-stu-id="3f3e4-199">Key</span></span>               | <span data-ttu-id="3f3e4-200">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-200">Value (type)</span></span> | <span data-ttu-id="3f3e4-201">Descripción</span><span class="sxs-lookup"><span data-stu-id="3f3e4-201">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="3f3e4-202">sendfile.SendAsync</span><span class="sxs-lookup"><span data-stu-id="3f3e4-202">sendfile.SendAsync</span></span> | <span data-ttu-id="3f3e4-203">Vea [Delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) (Signatura de delegado)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-203">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="3f3e4-204">Por solicitud</span><span class="sxs-lookup"><span data-stu-id="3f3e4-204">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="3f3e4-205">Opaque v0.3.0</span><span class="sxs-lookup"><span data-stu-id="3f3e4-205">Opaque v0.3.0</span></span>

| <span data-ttu-id="3f3e4-206">Key</span><span class="sxs-lookup"><span data-stu-id="3f3e4-206">Key</span></span>               | <span data-ttu-id="3f3e4-207">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-207">Value (type)</span></span> | <span data-ttu-id="3f3e4-208">Descripción</span><span class="sxs-lookup"><span data-stu-id="3f3e4-208">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="3f3e4-209">opaque.Version</span><span class="sxs-lookup"><span data-stu-id="3f3e4-209">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="3f3e4-210">opaque.Upgrade</span><span class="sxs-lookup"><span data-stu-id="3f3e4-210">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="3f3e4-211">Vea [Delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) (Signatura de delegado)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-211">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="3f3e4-212">opaque.Stream</span><span class="sxs-lookup"><span data-stu-id="3f3e4-212">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="3f3e4-213">opaque.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="3f3e4-213">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="3f3e4-214">WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="3f3e4-214">WebSocket v0.3.0</span></span>

| <span data-ttu-id="3f3e4-215">Key</span><span class="sxs-lookup"><span data-stu-id="3f3e4-215">Key</span></span>               | <span data-ttu-id="3f3e4-216">Valor (tipo)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-216">Value (type)</span></span> | <span data-ttu-id="3f3e4-217">Descripción</span><span class="sxs-lookup"><span data-stu-id="3f3e4-217">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="3f3e4-218">websocket.Version</span><span class="sxs-lookup"><span data-stu-id="3f3e4-218">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="3f3e4-219">websocket.Accept</span><span class="sxs-lookup"><span data-stu-id="3f3e4-219">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="3f3e4-220">Vea [Delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) (Signatura de delegado)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-220">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="3f3e4-221">websocket.AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="3f3e4-221">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="3f3e4-222">Sin especificaciones</span><span class="sxs-lookup"><span data-stu-id="3f3e4-222">Non-spec</span></span> |
| <span data-ttu-id="3f3e4-223">websocket.SubProtocol</span><span class="sxs-lookup"><span data-stu-id="3f3e4-223">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="3f3e4-224">Vea la [sección 4.2.2 de RFC6455](https://tools.ietf.org/html/rfc6455#section-4.2.2), paso 5.5</span><span class="sxs-lookup"><span data-stu-id="3f3e4-224">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="3f3e4-225">websocket.SendAsync</span><span class="sxs-lookup"><span data-stu-id="3f3e4-225">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="3f3e4-226">Vea [Delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) (Signatura de delegado)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-226">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="3f3e4-227">websocket.ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="3f3e4-227">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="3f3e4-228">Vea [Delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) (Signatura de delegado)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-228">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="3f3e4-229">websocket.CloseAsync</span><span class="sxs-lookup"><span data-stu-id="3f3e4-229">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="3f3e4-230">Vea [Delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) (Signatura de delegado)</span><span class="sxs-lookup"><span data-stu-id="3f3e4-230">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="3f3e4-231">websocket.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="3f3e4-231">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="3f3e4-232">websocket.ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="3f3e4-232">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="3f3e4-233">Optional</span><span class="sxs-lookup"><span data-stu-id="3f3e4-233">Optional</span></span> |
| <span data-ttu-id="3f3e4-234">websocket.ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="3f3e4-234">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="3f3e4-235">Optional</span><span class="sxs-lookup"><span data-stu-id="3f3e4-235">Optional</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="3f3e4-236">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3f3e4-236">Additional resources</span></span>

* [<span data-ttu-id="3f3e4-237">Middleware</span><span class="sxs-lookup"><span data-stu-id="3f3e4-237">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="3f3e4-238">Servidores</span><span class="sxs-lookup"><span data-stu-id="3f3e4-238">Servers</span></span>](xref:fundamentals/servers/index)
