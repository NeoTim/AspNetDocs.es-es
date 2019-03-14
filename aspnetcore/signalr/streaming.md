---
title: Usar la transmisión por secuencias en ASP.NET Core SignalR
author: bradygaster
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: ade2d6fb6e799d53ff3aaa69c641d0088acdee95
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57036652"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="6152a-102">Usar la transmisión por secuencias en ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="6152a-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="6152a-103">Por [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="6152a-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="6152a-104">ASP.NET Core SignalR es compatible con la transmisión por secuencias los valores devueltos de métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="6152a-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="6152a-105">Esto es útil para escenarios de fragmentos de datos de procedencia con el tiempo.</span><span class="sxs-lookup"><span data-stu-id="6152a-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="6152a-106">Cuando se transmite un valor devuelto al cliente, significará que se ha enviado cada fragmento al cliente en cuanto se convierte en disponible, en lugar de tener que esperar todos los datos estén disponibles.</span><span class="sxs-lookup"><span data-stu-id="6152a-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="6152a-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6152a-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="6152a-108">Configurar el centro</span><span class="sxs-lookup"><span data-stu-id="6152a-108">Set up the hub</span></span>

<span data-ttu-id="6152a-109">Un método de concentrador se convierte automáticamente en un método de concentrador de transmisión por secuencias cuando vuelve una `ChannelReader<T>` o `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="6152a-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="6152a-110">A continuación es un ejemplo que muestra los conceptos básicos de transmisión de datos al cliente.</span><span class="sxs-lookup"><span data-stu-id="6152a-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="6152a-111">Cada vez que se escribe un objeto en el `ChannelReader` ese objeto inmediatamente se envía al cliente.</span><span class="sxs-lookup"><span data-stu-id="6152a-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="6152a-112">Al final, el `ChannelReader` completada para indicar al cliente la secuencia está cerrada.</span><span class="sxs-lookup"><span data-stu-id="6152a-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> * <span data-ttu-id="6152a-113">Escribir en el `ChannelReader` en un subproceso en segundo plano y vuelva el `ChannelReader` tan pronto como sea posible.</span><span class="sxs-lookup"><span data-stu-id="6152a-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="6152a-114">Las demás invocaciones de concentrador se bloqueará hasta que un `ChannelReader` se devuelve.</span><span class="sxs-lookup"><span data-stu-id="6152a-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>
> * <span data-ttu-id="6152a-115">Encapsular la lógica en un `try ... catch` y complete el `Channel` en la captura y fuera de la instrucción catch para asegurarse de que el centro de invocación del método se completó correctamente.</span><span class="sxs-lookup"><span data-stu-id="6152a-115">Wrap your logic in a `try ... catch` and complete the `Channel` in the catch and outside the catch to make sure the hub method invocation is completed properly.</span></span>

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

<span data-ttu-id="6152a-116">En ASP.NET Core 2.2 o posterior, la transmisión por secuencias los métodos de concentrador puede aceptar un `CancellationToken` parámetro que se desencadena cuando el cliente cancela la suscripción de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="6152a-116">In ASP.NET Core 2.2 or later, streaming Hub methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="6152a-117">Use este token para detener la operación del servidor y libere cualquier recurso si el cliente se desconecta antes del final de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="6152a-117">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="6152a-118">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="6152a-118">.NET client</span></span>

<span data-ttu-id="6152a-119">El `StreamAsChannelAsync` método `HubConnection` se utiliza para invocar un método de transmisión por secuencias.</span><span class="sxs-lookup"><span data-stu-id="6152a-119">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="6152a-120">Pase el nombre del método de concentrador y los argumentos definidos en el método de concentrador a `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="6152a-120">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="6152a-121">El parámetro genérico en `StreamAsChannelAsync<T>` especifica el tipo de objetos devueltos por el método de transmisión por secuencias.</span><span class="sxs-lookup"><span data-stu-id="6152a-121">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="6152a-122">Un `ChannelReader<T>` se devuelve desde la invocación de la secuencia y representa el flujo en el cliente.</span><span class="sxs-lookup"><span data-stu-id="6152a-122">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="6152a-123">Para leer datos, un patrón común es para recorrer en bucle `WaitToReadAsync` y llamar a `TryRead` cuando los datos están disponibles.</span><span class="sxs-lookup"><span data-stu-id="6152a-123">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="6152a-124">El bucle finalizará cuando el servidor ha cerrado la secuencia o el token de cancelación pasado a `StreamAsChannelAsync` se cancela.</span><span class="sxs-lookup"><span data-stu-id="6152a-124">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to 
// the server, which will trigger the corresponding token in the Hub method.
var cancellationTokenSource = new CancellationTokenSource();
var channel = await hubConnection.StreamAsChannelAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

```csharp
var channel = await hubConnection
    .StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="6152a-125">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="6152a-125">JavaScript client</span></span>

<span data-ttu-id="6152a-126">Los clientes de JavaScript llamar a métodos de transmisión por secuencias en concentradores mediante `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="6152a-126">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="6152a-127">El `stream` método acepta dos argumentos:</span><span class="sxs-lookup"><span data-stu-id="6152a-127">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="6152a-128">El nombre del método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="6152a-128">The name of the hub method.</span></span> <span data-ttu-id="6152a-129">En el ejemplo siguiente, que es el nombre del método de concentrador `Counter`.</span><span class="sxs-lookup"><span data-stu-id="6152a-129">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="6152a-130">Argumentos definidos en el método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="6152a-130">Arguments defined in the hub method.</span></span> <span data-ttu-id="6152a-131">En el ejemplo siguiente, los argumentos son: un recuento del número de elementos de flujo para recibir y el retraso entre los elementos de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="6152a-131">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="6152a-132">`connection.stream` Devuelve un `IStreamResult` que contiene un `subscribe` método.</span><span class="sxs-lookup"><span data-stu-id="6152a-132">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="6152a-133">Pasar un `IStreamSubscriber` a `subscribe` y establezca el `next`, `error`, y `complete` devoluciones de llamada para recibir notificaciones, la `stream` invocación.</span><span class="sxs-lookup"><span data-stu-id="6152a-133">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="6152a-134">Para finalizar la secuencia desde el cliente, llame a la `dispose` método en el `ISubscription` que se devuelve desde el `subscribe` método.</span><span class="sxs-lookup"><span data-stu-id="6152a-134">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="6152a-135">Para finalizar la secuencia desde el cliente, llame a la `dispose` método en el `ISubscription` que se devuelve desde el `subscribe` método.</span><span class="sxs-lookup"><span data-stu-id="6152a-135">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span> <span data-ttu-id="6152a-136">Llamar a este método hará que el `CancellationToken` parámetro del método de concentrador (si se proporciona uno) se cancele.</span><span class="sxs-lookup"><span data-stu-id="6152a-136">Calling this method will cause the `CancellationToken` parameter of the Hub method (if you provided one) to be canceled.</span></span>

::: moniker-end

## <a name="related-resources"></a><span data-ttu-id="6152a-137">Recursos relacionados</span><span class="sxs-lookup"><span data-stu-id="6152a-137">Related resources</span></span>

* [<span data-ttu-id="6152a-138">Concentradores</span><span class="sxs-lookup"><span data-stu-id="6152a-138">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="6152a-139">Cliente .NET</span><span class="sxs-lookup"><span data-stu-id="6152a-139">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="6152a-140">Cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="6152a-140">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="6152a-141">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="6152a-141">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
