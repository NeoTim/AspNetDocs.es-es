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
# <a name="aspnet-webhooks-handlers"></a>ASP.NET webHooks controladores

Una vez que webHooks solicita ha sido validado por un receptor de WebHook, está listo para ser procesado por el código de usuario. Aquí es donde entran *los controladores.* Los controladores derivan de la [interfaz IWebHookHandler,](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) pero normalmente usan la clase [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) en lugar de derivar directamente de la interfaz.

Uno o varios controladores pueden procesar una solicitud de WebHook. Los controladores se llaman en orden basado en su respectiva propiedad *Order* que va de menor a mayor donde Order es un entero simple (sugerido que está entre 1 y 100):

![Diagrama de propiedades de orden del controlador de WebHook](_static/Handlers.png)

Un controlador puede establecer opcionalmente el *Response* propiedad en el WebHookHandlerContext que llevará el procesamiento a detenerse y la respuesta que se enviará de vuelta como la respuesta HTTP a la WebHook. En el caso anterior, el controlador C no se llamará porque tiene un orden más alto que B y B establece la respuesta.

La configuración de la respuesta normalmente solo es relevante para WebHooks, donde la respuesta puede llevar información a la API de origen. Este es, por ejemplo, el caso de Slack WebHooks donde la respuesta se devuelve al canal de donde procede WebHook. Los controladores pueden establecer la propiedad Receiver si solo desean recibir WebHooks de ese receptor concreto. Si no establecen el receptor, se les llama a todos ellos.

Otro uso común de una respuesta es usar una respuesta *410 Gone* para indicar que el WebHook ya no está activo y no se deben enviar más solicitudes.

De forma predeterminada, todos los receptores de WebHook llamarán a un controlador. Sin embargo, si la propiedad *Receiver* se establece en el nombre de un controlador, ese controlador solo recibirá solicitudes WebHook de ese receptor.

## <a name="processing-a-webhook"></a>Procesamiento de un WebHook

Cuando se llama a un controlador, obtiene un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) que contiene información sobre la solicitud WebHook. Los datos, normalmente el cuerpo de la solicitud HTTP, están disponibles en la propiedad *Data.*

El tipo de los datos suele ser JSON o HTML, pero es posible convertir a un tipo más específico si se desea. Por ejemplo, los WebHooks personalizados generados por ASP.NET WebHooks se pueden convertir al tipo [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) de la siguiente manera:

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

  ## <a name="queued-processing"></a>Procesamiento en cola

La mayoría de los remitentes de WebHook volverán a enviar un WebHook si no se genera una respuesta en un puñado de segundos. Esto significa que el controlador debe completar el procesamiento dentro de ese período de tiempo para no volver a llamarlo.

Si el procesamiento tarda más o se controla mejor por separado, se puede usar [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) para enviar la solicitud WebHook a una cola, por ejemplo [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Aquí se proporciona un esquema de una implementación [de WebHookQueueHandler:](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs)

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
