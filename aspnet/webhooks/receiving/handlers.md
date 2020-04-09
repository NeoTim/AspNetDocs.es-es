---
uid: webhooks/receiving/handlers
title: ASP.NET controladores de WebHooks Microsoft Docs
author: rick-anderson
description: Cómo controlar las solicitudes en ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: ff12dd8df167eca17ecbd9f03a71807d44af2b30
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675210"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="33372-103">ASP.NET webHooks controladores</span><span class="sxs-lookup"><span data-stu-id="33372-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="33372-104">Una vez que webHooks solicita ha sido validado por un receptor de WebHook, está listo para ser procesado por el código de usuario.</span><span class="sxs-lookup"><span data-stu-id="33372-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="33372-105">Aquí es donde entran *los controladores.*</span><span class="sxs-lookup"><span data-stu-id="33372-105">This is where *handlers* come in.</span></span> <span data-ttu-id="33372-106">Los controladores derivan de la [interfaz IWebHookHandler,](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) pero normalmente usan la clase [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) en lugar de derivar directamente de la interfaz.</span><span class="sxs-lookup"><span data-stu-id="33372-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="33372-107">Uno o varios controladores pueden procesar una solicitud de WebHook.</span><span class="sxs-lookup"><span data-stu-id="33372-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="33372-108">Los controladores se llaman en orden basado en su respectiva propiedad *Order* que va de menor a mayor donde Order es un entero simple (sugerido que está entre 1 y 100):</span><span class="sxs-lookup"><span data-stu-id="33372-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Diagrama de propiedades de orden del controlador de WebHook](_static/Handlers.png)

<span data-ttu-id="33372-110">Un controlador puede establecer opcionalmente el *Response* propiedad en el WebHookHandlerContext que llevará el procesamiento a detenerse y la respuesta que se enviará de vuelta como la respuesta HTTP a la WebHook.</span><span class="sxs-lookup"><span data-stu-id="33372-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="33372-111">En el caso anterior, el controlador C no se llamará porque tiene un orden más alto que B y B establece la respuesta.</span><span class="sxs-lookup"><span data-stu-id="33372-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="33372-112">La configuración de la respuesta normalmente solo es relevante para WebHooks, donde la respuesta puede llevar información a la API de origen.</span><span class="sxs-lookup"><span data-stu-id="33372-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="33372-113">Este es, por ejemplo, el caso de Slack WebHooks donde la respuesta se devuelve al canal de donde procede WebHook.</span><span class="sxs-lookup"><span data-stu-id="33372-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="33372-114">Los controladores pueden establecer la propiedad Receiver si solo desean recibir WebHooks de ese receptor concreto.</span><span class="sxs-lookup"><span data-stu-id="33372-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="33372-115">Si no establecen el receptor, se les llama a todos ellos.</span><span class="sxs-lookup"><span data-stu-id="33372-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="33372-116">Otro uso común de una respuesta es usar una respuesta *410 Gone* para indicar que el WebHook ya no está activo y no se deben enviar más solicitudes.</span><span class="sxs-lookup"><span data-stu-id="33372-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="33372-117">De forma predeterminada, todos los receptores de WebHook llamarán a un controlador.</span><span class="sxs-lookup"><span data-stu-id="33372-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="33372-118">Sin embargo, si la propiedad *Receiver* se establece en el nombre de un controlador, ese controlador solo recibirá solicitudes WebHook de ese receptor.</span><span class="sxs-lookup"><span data-stu-id="33372-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="33372-119">Procesamiento de un WebHook</span><span class="sxs-lookup"><span data-stu-id="33372-119">Processing a WebHook</span></span>

<span data-ttu-id="33372-120">Cuando se llama a un controlador, obtiene un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) que contiene información sobre la solicitud WebHook.</span><span class="sxs-lookup"><span data-stu-id="33372-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="33372-121">Los datos, normalmente el cuerpo de la solicitud HTTP, están disponibles en la propiedad *Data.*</span><span class="sxs-lookup"><span data-stu-id="33372-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="33372-122">El tipo de los datos suele ser JSON o HTML, pero es posible convertir a un tipo más específico si se desea.</span><span class="sxs-lookup"><span data-stu-id="33372-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="33372-123">Por ejemplo, los WebHooks personalizados generados por ASP.NET WebHooks se pueden convertir al tipo [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="33372-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a><span data-ttu-id="33372-124">Procesamiento en cola</span><span class="sxs-lookup"><span data-stu-id="33372-124">Queued Processing</span></span>

<span data-ttu-id="33372-125">La mayoría de los remitentes de WebHook volverán a enviar un WebHook si no se genera una respuesta en un puñado de segundos.</span><span class="sxs-lookup"><span data-stu-id="33372-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="33372-126">Esto significa que el controlador debe completar el procesamiento dentro de ese período de tiempo para no volver a llamarlo.</span><span class="sxs-lookup"><span data-stu-id="33372-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="33372-127">Si el procesamiento tarda más o se controla mejor por separado, se puede usar [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) para enviar la solicitud WebHook a una cola, por ejemplo [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="33372-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="33372-128">Aquí se proporciona un esquema de una implementación [de WebHookQueueHandler:](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)</span><span class="sxs-lookup"><span data-stu-id="33372-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
