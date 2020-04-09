---
uid: signalr/overview/performance/signalr-performance
title: Rendimiento de la señal (SignalR Performance) Microsoft Docs
author: bradygaster
description: Rendimiento de SignalR
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: b8a44f4c924c94cdfff1ce7630539b45fe269bbf
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675722"
---
# <a name="signalr-performance"></a><span data-ttu-id="66938-103">Rendimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="66938-103">SignalR Performance</span></span>

<span data-ttu-id="66938-104">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="66938-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="66938-105">En este tema se describe cómo diseñar, medir y mejorar el rendimiento en una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="66938-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="66938-106">Versiones de software utilizadas en este tema</span><span class="sxs-lookup"><span data-stu-id="66938-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="66938-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="66938-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="66938-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="66938-108">.NET 4.5</span></span>
> - <span data-ttu-id="66938-109">SignalR versión 2</span><span class="sxs-lookup"><span data-stu-id="66938-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="66938-110">Versiones anteriores de este tema</span><span class="sxs-lookup"><span data-stu-id="66938-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="66938-111">Para obtener información acerca de las versiones anteriores de SignalR, vea [Versiones anteriores](../older-versions/index.md)de SignalR .</span><span class="sxs-lookup"><span data-stu-id="66938-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="66938-112">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="66938-112">Questions and comments</span></span>
>
> <span data-ttu-id="66938-113">Por favor, deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="66938-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="66938-114">Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el [ASP.NET foro](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) de SignalR o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="66938-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="66938-115">Para obtener una presentación reciente sobre el rendimiento y el escalado de SignalR, consulte Escalado de la Web en [tiempo real con ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="66938-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="66938-116">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="66938-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="66938-117">Consideraciones de diseño</span><span class="sxs-lookup"><span data-stu-id="66938-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="66938-118">Ajuste del servidor SignalR para el rendimiento</span><span class="sxs-lookup"><span data-stu-id="66938-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="66938-119">Solución de problemas de rendimiento</span><span class="sxs-lookup"><span data-stu-id="66938-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="66938-120">Uso de contadores de rendimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="66938-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="66938-121">Uso de otros contadores de rendimiento</span><span class="sxs-lookup"><span data-stu-id="66938-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="66938-122">Otros recursos</span><span class="sxs-lookup"><span data-stu-id="66938-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="66938-123">Consideraciones de diseño</span><span class="sxs-lookup"><span data-stu-id="66938-123">Design considerations</span></span>

<span data-ttu-id="66938-124">En esta sección se describen los patrones que se pueden implementar durante el diseño de una aplicación de SignalR, para asegurarse de que el rendimiento no se ve obstaculizado por la generación de tráfico de red innecesario.</span><span class="sxs-lookup"><span data-stu-id="66938-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="66938-125">Frecuencia de mensajes de limitación</span><span class="sxs-lookup"><span data-stu-id="66938-125">Throttling message frequency</span></span>

<span data-ttu-id="66938-126">Incluso en una aplicación que envía mensajes a una alta frecuencia (como una aplicación de juegos en tiempo real), la mayoría de las aplicaciones no necesitan enviar más de unos pocos mensajes por segundo.</span><span class="sxs-lookup"><span data-stu-id="66938-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="66938-127">Para reducir la cantidad de tráfico que genera cada cliente, se puede implementar un bucle de mensajes que pone en cola y envía mensajes no con más frecuencia que una velocidad fija (es decir, se enviarán hasta un cierto número de mensajes cada segundo, si hay mensajes en ese intervalo de tiempo que se enviarán).</span><span class="sxs-lookup"><span data-stu-id="66938-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="66938-128">Para obtener una aplicación de ejemplo que limita los mensajes a una velocidad determinada (tanto desde el cliente como desde el servidor), consulte [Tiempo real de alta frecuencia con SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="66938-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="66938-129">Reducción del tamaño del mensaje</span><span class="sxs-lookup"><span data-stu-id="66938-129">Reducing message size</span></span>

<span data-ttu-id="66938-130">Puede reducir el tamaño de un mensaje de SignalR reduciendo el tamaño de los objetos serializados.</span><span class="sxs-lookup"><span data-stu-id="66938-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="66938-131">En el código del servidor, si va a enviar un objeto que contiene propiedades que no es `JsonIgnore` necesario transmitir, evite que esas propiedades se serializan mediante el atributo.</span><span class="sxs-lookup"><span data-stu-id="66938-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="66938-132">Los nombres de las propiedades también se almacenan en el mensaje; los nombres de las propiedades se `JsonProperty` pueden acortar mediante el atributo.</span><span class="sxs-lookup"><span data-stu-id="66938-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="66938-133">En el ejemplo de código siguiente se muestra cómo excluir una propiedad de ser enviada al cliente y cómo acortar los nombres de propiedad:</span><span class="sxs-lookup"><span data-stu-id="66938-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="66938-134">**Código de servidor .NET que muestra el atributo JsonIgnore para excluir los datos de los datos que se envían al cliente y el atributo JsonProperty para reducir el tamaño del mensaje**</span><span class="sxs-lookup"><span data-stu-id="66938-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="66938-135">Para conservar la legibilidad/la capacidad de mantenimiento en el código de cliente, los nombres de propiedad abreviados se pueden reasignar a nombres descriptivos después de recibir el mensaje.</span><span class="sxs-lookup"><span data-stu-id="66938-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="66938-136">En el ejemplo de código siguiente se muestra una posible forma de reasignar nombres abreviados a `reMap` otros más largos, definiendo un contrato de mensaje (asignación) y utilizando la función para aplicar el contrato a la clase de mensaje optimizada:</span><span class="sxs-lookup"><span data-stu-id="66938-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="66938-137">**Código JavaScript del lado cliente que reasigna nombres de propiedad abreviados a nombres legibles por humanos**</span><span class="sxs-lookup"><span data-stu-id="66938-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="66938-138">Los nombres también se pueden acortar en los mensajes del cliente al servidor, utilizando el mismo método.</span><span class="sxs-lookup"><span data-stu-id="66938-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="66938-139">La reducción de la huella de memoria (es decir, la cantidad de memoria utilizada para el mensaje) del objeto de mensaje también puede mejorar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="66938-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="66938-140">Por ejemplo, si no `int` se necesita el `short` `byte` rango completo de un, se puede usar un o se puede usar en su lugar.</span><span class="sxs-lookup"><span data-stu-id="66938-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="66938-141">Puesto que los mensajes se almacenan en el bus de mensajes en la memoria del servidor, la reducción del tamaño de los mensajes también puede solucionar problemas de memoria del servidor.</span><span class="sxs-lookup"><span data-stu-id="66938-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="66938-142">Ajuste del servidor SignalR para el rendimiento</span><span class="sxs-lookup"><span data-stu-id="66938-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="66938-143">Los siguientes valores de configuración se pueden usar para ajustar el servidor para un mejor rendimiento en una aplicación de SignalR.</span><span class="sxs-lookup"><span data-stu-id="66938-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="66938-144">Para obtener información general sobre cómo mejorar el rendimiento en una aplicación ASP.NET, consulte [Mejora del rendimiento ASP.NET.](https://msdn.microsoft.com/library/ff647787.aspx)</span><span class="sxs-lookup"><span data-stu-id="66938-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="66938-145">**Configuración de SignalR**</span><span class="sxs-lookup"><span data-stu-id="66938-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="66938-146">**DefaultMessageBufferSize**: De forma predeterminada, SignalR conserva 1000 mensajes en memoria por concentrador por conexión.</span><span class="sxs-lookup"><span data-stu-id="66938-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="66938-147">Si se utilizan mensajes grandes, esto puede crear problemas de memoria que se pueden aliviar reduciendo este valor.</span><span class="sxs-lookup"><span data-stu-id="66938-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="66938-148">Esta configuración se puede `Application_Start` establecer en el controlador de `Configuration` eventos en una aplicación de ASP.NET o en el método de una clase de inicio OWIN en una aplicación autohospedada.</span><span class="sxs-lookup"><span data-stu-id="66938-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="66938-149">En el ejemplo siguiente se muestra cómo reducir este valor para reducir la huella de memoria de la aplicación para reducir la cantidad de memoria de servidor utilizada:</span><span class="sxs-lookup"><span data-stu-id="66938-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="66938-150">**Código de servidor .NET en Startup.cs para disminuir el tamaño predeterminado del búfer de mensajes**</span><span class="sxs-lookup"><span data-stu-id="66938-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="66938-151">**Configuración de IIS**</span><span class="sxs-lookup"><span data-stu-id="66938-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="66938-152">Máximo de **solicitudes simultáneas por aplicación:** aumentar el número de solicitudes de IIS simultáneas aumentará los recursos del servidor disponibles para atender solicitudes.</span><span class="sxs-lookup"><span data-stu-id="66938-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="66938-153">El valor predeterminado es 5000; para aumentar esta configuración, ejecute los siguientes comandos en un símbolo del sistema con privilegios elevados:</span><span class="sxs-lookup"><span data-stu-id="66938-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="66938-154">**ApplicationPool QueueLength**: Este es el número máximo de solicitudes que Http.sys pone en cola para el grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="66938-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="66938-155">Cuando la cola está llena, las nuevas solicitudes reciben una respuesta 503 "Servicio no disponible".</span><span class="sxs-lookup"><span data-stu-id="66938-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="66938-156">El valor predeterminado es 1000.</span><span class="sxs-lookup"><span data-stu-id="66938-156">The default value is 1000.</span></span>

    <span data-ttu-id="66938-157">Acortar la longitud de la cola para el proceso de trabajo en el grupo de aplicaciones que hospeda la aplicación conserve los recursos de memoria.</span><span class="sxs-lookup"><span data-stu-id="66938-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="66938-158">Para obtener más información, consulte [Administración, optimización y configuración de](https://technet.microsoft.com/library/cc745955.aspx)grupos de aplicaciones .</span><span class="sxs-lookup"><span data-stu-id="66938-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="66938-159">**ASP.NET configuración**</span><span class="sxs-lookup"><span data-stu-id="66938-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="66938-160">Esta sección incluye los valores de `aspnet.config` configuración que se pueden establecer en el archivo.</span><span class="sxs-lookup"><span data-stu-id="66938-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="66938-161">Este archivo se encuentra en una de las dos ubicaciones, dependiendo de la plataforma:</span><span class="sxs-lookup"><span data-stu-id="66938-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="66938-162">ASP.NET configuración que puede mejorar el rendimiento de SignalR incluyen lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="66938-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="66938-163">**Máximo de solicitudes simultáneas por CPU:** aumentar esta configuración puede aliviar los cuellos de botella de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="66938-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="66938-164">Para aumentar esta configuración, agregue la `aspnet.config` siguiente configuración al archivo:</span><span class="sxs-lookup"><span data-stu-id="66938-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="66938-165">**Límite**de cola de solicitudes: cuando `maxConcurrentRequestsPerCPU` el número total de conexiones supera la configuración, ASP.NET comenzará a limitar las solicitudes mediante una cola.</span><span class="sxs-lookup"><span data-stu-id="66938-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="66938-166">Para aumentar el tamaño de la `requestQueueLimit` cola, puede aumentar la configuración.</span><span class="sxs-lookup"><span data-stu-id="66938-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="66938-167">Para ello, agregue la siguiente `processModel` configuración al `config/machine.config` nodo `aspnet.config`en (en lugar de ):</span><span class="sxs-lookup"><span data-stu-id="66938-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="66938-168">Solución de problemas de rendimiento</span><span class="sxs-lookup"><span data-stu-id="66938-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="66938-169">En esta sección se describen formas de encontrar cuellos de botella de rendimiento en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="66938-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="66938-170">Comprobación de que se está utilizando WebSocket</span><span class="sxs-lookup"><span data-stu-id="66938-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="66938-171">Aunque SignalR puede usar una variedad de transportes para la comunicación entre el cliente y el servidor, WebSocket ofrece una ventaja de rendimiento significativa y debe usarse si el cliente y el servidor lo admiten.</span><span class="sxs-lookup"><span data-stu-id="66938-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="66938-172">Para determinar si el cliente y el servidor cumplen los requisitos de WebSocket, vea [Transportes y reserva.](../getting-started/introduction-to-signalr.md#transports)</span><span class="sxs-lookup"><span data-stu-id="66938-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="66938-173">Para determinar qué transporte se usa en la aplicación, puede usar las herramientas de desarrollador del explorador y examinar los registros para ver qué transporte se usa para la conexión.</span><span class="sxs-lookup"><span data-stu-id="66938-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="66938-174">Para obtener información sobre el uso de las herramientas de desarrollo del navegador en Internet Explorer y Chrome, consulte [Transportes y reserva .](../getting-started/introduction-to-signalr.md#transports)</span><span class="sxs-lookup"><span data-stu-id="66938-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="66938-175">Uso de contadores de rendimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="66938-175">Using SignalR performance counters</span></span>

<span data-ttu-id="66938-176">En esta sección se describe cómo habilitar y utilizar `Microsoft.AspNet.SignalR.Utils` los contadores de rendimiento de SignalR, que se encuentran en el paquete.</span><span class="sxs-lookup"><span data-stu-id="66938-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="66938-177">Instalación de signalr.exe</span><span class="sxs-lookup"><span data-stu-id="66938-177">Installing signalr.exe</span></span>

<span data-ttu-id="66938-178">Los contadores de rendimiento se pueden agregar al servidor mediante una utilidad denominada SignalR.exe.</span><span class="sxs-lookup"><span data-stu-id="66938-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="66938-179">Para instalar esta utilidad, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="66938-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="66938-180">En Visual Studio, seleccione **Herramientas** > **NuGet Package Manager** > **Administrar paquetes NuGet para la solución**</span><span class="sxs-lookup"><span data-stu-id="66938-180">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="66938-181">Busque **signalr.utils**y seleccione Instalar.</span><span class="sxs-lookup"><span data-stu-id="66938-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="66938-182">Acepte el acuerdo de licencia para instalar el paquete.</span><span class="sxs-lookup"><span data-stu-id="66938-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="66938-183">SignalR.exe se instalará `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`en .</span><span class="sxs-lookup"><span data-stu-id="66938-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="66938-184">Instalación de contadores de rendimiento con SignalR.exe</span><span class="sxs-lookup"><span data-stu-id="66938-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="66938-185">Para instalar contadores de rendimiento de SignalR, ejecute SignalR.exe en un símbolo del sistema con privilegios elevados con el siguiente parámetro:</span><span class="sxs-lookup"><span data-stu-id="66938-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="66938-186">Para quitar los contadores de rendimiento de SignalR, ejecute SignalR.exe en un símbolo del sistema con privilegios elevados con el siguiente parámetro:</span><span class="sxs-lookup"><span data-stu-id="66938-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="66938-187">Contadores de rendimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="66938-187">SignalR Performance counters</span></span>

<span data-ttu-id="66938-188">El paquete de utilidades instala los siguientes contadores de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="66938-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="66938-189">Los contadores "Total" miden el número de eventos desde el último grupo de aplicaciones o el reinicio del servidor.</span><span class="sxs-lookup"><span data-stu-id="66938-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="66938-190">**Métricas de conexión**</span><span class="sxs-lookup"><span data-stu-id="66938-190">**Connection metrics**</span></span>

<span data-ttu-id="66938-191">Las siguientes métricas miden los eventos de duración de la conexión que se producen.</span><span class="sxs-lookup"><span data-stu-id="66938-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="66938-192">Para obtener más información, consulte [Descripción y control](../guide-to-the-api/handling-connection-lifetime-events.md)de eventos de duración de la conexión .</span><span class="sxs-lookup"><span data-stu-id="66938-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="66938-193">**Conexiones conectadas**</span><span class="sxs-lookup"><span data-stu-id="66938-193">**Connections Connected**</span></span>
- <span data-ttu-id="66938-194">**Conexiones reconectadas**</span><span class="sxs-lookup"><span data-stu-id="66938-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="66938-195">**Conexiones desconectadas**</span><span class="sxs-lookup"><span data-stu-id="66938-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="66938-196">**Conexiones actuales**</span><span class="sxs-lookup"><span data-stu-id="66938-196">**Connections Current**</span></span>

<span data-ttu-id="66938-197">**Métricas de mensaje**</span><span class="sxs-lookup"><span data-stu-id="66938-197">**Message metrics**</span></span>

<span data-ttu-id="66938-198">Las siguientes métricas miden el tráfico de mensajes generado por SignalR.</span><span class="sxs-lookup"><span data-stu-id="66938-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="66938-199">**Mensajes de conexión Total recibidos**</span><span class="sxs-lookup"><span data-stu-id="66938-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="66938-200">**Mensajes de conexión enviados total**</span><span class="sxs-lookup"><span data-stu-id="66938-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="66938-201">**Mensajes de conexión recibidos/seg**</span><span class="sxs-lookup"><span data-stu-id="66938-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="66938-202">**Mensajes de conexión enviados por segundo**</span><span class="sxs-lookup"><span data-stu-id="66938-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="66938-203">**Métricas de bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="66938-203">**Message bus metrics**</span></span>

<span data-ttu-id="66938-204">Las métricas siguientes miden el tráfico a través del bus de mensajes de SignalR interno, la cola en la que se colocan todos los mensajes de SignalR entrantes y salientes.</span><span class="sxs-lookup"><span data-stu-id="66938-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="66938-205">Un mensaje se **publica** cuando se envía o se difunde.</span><span class="sxs-lookup"><span data-stu-id="66938-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="66938-206">Un **suscriptor** en este contexto es una suscripción en el bus de mensajes; esto debe ser igual al número de clientes más el propio servidor.</span><span class="sxs-lookup"><span data-stu-id="66938-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="66938-207">Un **trabajador asignado** es un componente que envía datos a conexiones activas; un **Trabajador Ocupado** es aquel que está enviando activamente un mensaje.</span><span class="sxs-lookup"><span data-stu-id="66938-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="66938-208">**Mensajes de bus de mensajes recibidos Total**</span><span class="sxs-lookup"><span data-stu-id="66938-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="66938-209">**Mensajes de bus de mensajes recibidos/seg**</span><span class="sxs-lookup"><span data-stu-id="66938-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="66938-210">**Mensajes de bus de mensajes publicados Total**</span><span class="sxs-lookup"><span data-stu-id="66938-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="66938-211">**Mensajes de bus de mensajes publicados/seg**</span><span class="sxs-lookup"><span data-stu-id="66938-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="66938-212">**Suscriptores de Bus de mensajes actuales**</span><span class="sxs-lookup"><span data-stu-id="66938-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="66938-213">**Total de suscriptores de Bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="66938-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="66938-214">**Suscriptores de Bus de mensajes/Sec**</span><span class="sxs-lookup"><span data-stu-id="66938-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="66938-215">**Mensaje Bus Asignado Trabajadores**</span><span class="sxs-lookup"><span data-stu-id="66938-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="66938-216">**Mensaje Bus Trabajadores ocupados**</span><span class="sxs-lookup"><span data-stu-id="66938-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="66938-217">**Temas de bus de mensajes actuales**</span><span class="sxs-lookup"><span data-stu-id="66938-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="66938-218">**Métricas de error**</span><span class="sxs-lookup"><span data-stu-id="66938-218">**Error metrics**</span></span>

<span data-ttu-id="66938-219">Las siguientes métricas miden los errores generados por el tráfico de mensajes de SignalR.</span><span class="sxs-lookup"><span data-stu-id="66938-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="66938-220">Los errores de **resolución** de concentradores se producen cuando no se puede resolver un método de concentrador o concentrador.</span><span class="sxs-lookup"><span data-stu-id="66938-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="66938-221">Los errores de **invocación** de concentrador son excepciones que se producen al invocar un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="66938-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="66938-222">**Los** errores de transporte son errores de conexión producidos durante una solicitud o respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="66938-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="66938-223">**Errores: Total Total**</span><span class="sxs-lookup"><span data-stu-id="66938-223">**Errors: All Total**</span></span>
- <span data-ttu-id="66938-224">**Errores: All/Sec**</span><span class="sxs-lookup"><span data-stu-id="66938-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="66938-225">**Errores: Total de resolución del concentrador**</span><span class="sxs-lookup"><span data-stu-id="66938-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="66938-226">**Errores: Resolución del concentrador/Sec**</span><span class="sxs-lookup"><span data-stu-id="66938-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="66938-227">**Errores: Total de invocación de concentrador**</span><span class="sxs-lookup"><span data-stu-id="66938-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="66938-228">**Errores: Invocación de Hub/Sec**</span><span class="sxs-lookup"><span data-stu-id="66938-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="66938-229">**Errores: Total de transporte**</span><span class="sxs-lookup"><span data-stu-id="66938-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="66938-230">**Errores: Transporte/Sec**</span><span class="sxs-lookup"><span data-stu-id="66938-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="66938-231">**Métricas de escalado horizontal**</span><span class="sxs-lookup"><span data-stu-id="66938-231">**Scaleout metrics**</span></span>

<span data-ttu-id="66938-232">Las siguientes métricas miden el tráfico y los errores generados por el proveedor de escalado horizontal.</span><span class="sxs-lookup"><span data-stu-id="66938-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="66938-233">Un **Stream** en este contexto es una unidad de escalado utilizada por el proveedor de escalado horizontal; se trata de una tabla si se usa SQL Server, un tema si se usa Service Bus y una suscripción si se usa Redis.</span><span class="sxs-lookup"><span data-stu-id="66938-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="66938-234">Cada secuencia garantiza operaciones ordenadas de lectura y escritura; una sola secuencia es un cuello de botella de escala potencial, por lo que el número de secuencias se puede aumentar para ayudar a reducir ese cuello de botella.</span><span class="sxs-lookup"><span data-stu-id="66938-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="66938-235">Si se utilizan varias secuencias, SignalR distribuirá automáticamente mensajes (partición) a través de estas secuencias de una manera que garantice que los mensajes enviados desde cualquier conexión dada estén en orden.</span><span class="sxs-lookup"><span data-stu-id="66938-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="66938-236">El [valor MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) controla la longitud de la cola de envío de escalado horizontal mantenida por SignalR.</span><span class="sxs-lookup"><span data-stu-id="66938-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="66938-237">Si se establece en un valor mayor que 0, todos los mensajes se colocarán en una cola de envío para enviarlos de uno en uno al backplane de mensajería configurado.</span><span class="sxs-lookup"><span data-stu-id="66938-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="66938-238">Si el tamaño de la cola supera la longitud configurada, las llamadas posteriores a enviar producirán un error inmediatamente con una [excepción InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) hasta que el número de mensajes de la cola sea menor que la configuración de nuevo.</span><span class="sxs-lookup"><span data-stu-id="66938-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="66938-239">La cola está deshabilitada de forma predeterminada porque los backplanes implementados generalmente tienen su propia cola o control de flujo en su lugar.</span><span class="sxs-lookup"><span data-stu-id="66938-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="66938-240">En el caso de SQL ServerSQL Server, la agrupación de conexiones limita eficazmente el número de envíos que se están sucediendo en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="66938-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="66938-241">De forma predeterminada, solo se usa una secuencia para SQL Server y Redis, se usan cinco secuencias para Service Bus y la cola está deshabilitada, pero esta configuración se puede cambiar a través de la configuración en SQL ServerSQL Server y Service Bus:</span><span class="sxs-lookup"><span data-stu-id="66938-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="66938-242">**Código de servidor .NET para configurar el recuento de tablas y la longitud de la cola para el backplane de SQL Server**</span><span class="sxs-lookup"><span data-stu-id="66938-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="66938-243">**.NET Server Code para configurar el recuento de temas y la longitud de cola para el backplane de Service Bus**</span><span class="sxs-lookup"><span data-stu-id="66938-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="66938-244">Una secuencia **de almacenamiento** en búfer es una que ha entrado en un estado con errores; cuando la secuencia está en el estado con errores, todos los mensajes enviados al backplane fallarán inmediatamente hasta que la secuencia ya no esté fallando.</span><span class="sxs-lookup"><span data-stu-id="66938-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="66938-245">La longitud de la cola de **envío** es el número de mensajes que se han publicado pero que aún no se han enviado.</span><span class="sxs-lookup"><span data-stu-id="66938-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="66938-246">**Mensajes de bus de mensajes de escalabilidad horizontal recibidos/seg**</span><span class="sxs-lookup"><span data-stu-id="66938-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="66938-247">**Scaleout Streams Total**</span><span class="sxs-lookup"><span data-stu-id="66938-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="66938-248">**Scaleout Streams Open**</span><span class="sxs-lookup"><span data-stu-id="66938-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="66938-249">**Escalado horizontal Streams Buffering**</span><span class="sxs-lookup"><span data-stu-id="66938-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="66938-250">**Total de errores de escalado**</span><span class="sxs-lookup"><span data-stu-id="66938-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="66938-251">**Errores de escalado horizontal/seg**</span><span class="sxs-lookup"><span data-stu-id="66938-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="66938-252">**Longitud de la cola de envío de escalado horizontal**</span><span class="sxs-lookup"><span data-stu-id="66938-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="66938-253">Para obtener más información sobre lo que miden estos contadores, vea Escalado horizontal de [SignalR con Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="66938-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="66938-254">Uso de otros contadores de rendimiento</span><span class="sxs-lookup"><span data-stu-id="66938-254">Using other performance counters</span></span>

<span data-ttu-id="66938-255">Los siguientes contadores de rendimiento también pueden ser útiles para supervisar el rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="66938-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="66938-256">**Memoria**</span><span class="sxs-lookup"><span data-stu-id="66938-256">**Memory**</span></span>

- <span data-ttu-id="66938-257">Memoria\\de .NET CLR: bytes en todos los montones (para w3wp)</span><span class="sxs-lookup"><span data-stu-id="66938-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="66938-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="66938-258">**ASP.NET**</span></span>

- <span data-ttu-id="66938-259">ASP.NET-Solicitudes actuales</span><span class="sxs-lookup"><span data-stu-id="66938-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="66938-260">ASP.NET-Queued</span><span class="sxs-lookup"><span data-stu-id="66938-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="66938-261">ASP.NET-Rejected</span><span class="sxs-lookup"><span data-stu-id="66938-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="66938-262">**Cpu**</span><span class="sxs-lookup"><span data-stu-id="66938-262">**CPU**</span></span>

- <span data-ttu-id="66938-263">Información del procesador-Tiempo del procesador</span><span class="sxs-lookup"><span data-stu-id="66938-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="66938-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="66938-264">**TCP/IP**</span></span>

- <span data-ttu-id="66938-265">TCPv6/Conexiones establecidas</span><span class="sxs-lookup"><span data-stu-id="66938-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="66938-266">TCPv4/Conexiones establecidas</span><span class="sxs-lookup"><span data-stu-id="66938-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="66938-267">**Servicio web**</span><span class="sxs-lookup"><span data-stu-id="66938-267">**Web Service**</span></span>

- <span data-ttu-id="66938-268">Servicio web- Conexiones actuales</span><span class="sxs-lookup"><span data-stu-id="66938-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="66938-269">Servicio web- Conexiones máximas</span><span class="sxs-lookup"><span data-stu-id="66938-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="66938-270">**Subprocesos**</span><span class="sxs-lookup"><span data-stu-id="66938-270">**Threading**</span></span>

- <span data-ttu-id="66938-271">Bloqueos y subprocesos\\de .NET CLR : de los subprocesos lógicos actuales</span><span class="sxs-lookup"><span data-stu-id="66938-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="66938-272">Bloqueos y subprocesos\\de .NET CLR : de los subprocesos físicos actuales</span><span class="sxs-lookup"><span data-stu-id="66938-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="66938-273">Otros recursos</span><span class="sxs-lookup"><span data-stu-id="66938-273">Other Resources</span></span>

<span data-ttu-id="66938-274">Para obtener más información sobre ASP.NET la supervisión y el ajuste del rendimiento, consulte los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="66938-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="66938-275">[Información general acerca del rendimiento de ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="66938-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="66938-276">ASP.NET el uso de subprocesos en IIS 7.5, IIS 7.0 e IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="66938-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="66938-277">&lt;Elemento&gt; applicationPool (Configuración web)</span><span class="sxs-lookup"><span data-stu-id="66938-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
