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
# <a name="signalr-performance"></a>Rendimiento de SignalR

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este tema se describe cómo diseñar, medir y mejorar el rendimiento en una aplicación de SignalR.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software utilizadas en este tema
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR versión 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versiones anteriores de este tema
>
> Para obtener información acerca de las versiones anteriores de SignalR, vea [Versiones anteriores](../older-versions/index.md)de SignalR .
>
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
>
> Por favor, deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el [ASP.NET foro](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) de SignalR o [StackOverflow.com](http://stackoverflow.com/).

Para obtener una presentación reciente sobre el rendimiento y el escalado de SignalR, consulte Escalado de la Web en [tiempo real con ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).

Este tema contiene las siguientes secciones:

- [Consideraciones de diseño](#design)
- [Ajuste del servidor SignalR para el rendimiento](#tuning)
- [Solución de problemas de rendimiento](#troubleshooting)
- [Uso de contadores de rendimiento de SignalR](#perfcounters)
- [Uso de otros contadores de rendimiento](#othercounters)
- [Otros recursos](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Consideraciones de diseño

En esta sección se describen los patrones que se pueden implementar durante el diseño de una aplicación de SignalR, para asegurarse de que el rendimiento no se ve obstaculizado por la generación de tráfico de red innecesario.

### <a name="throttling-message-frequency"></a>Frecuencia de mensajes de limitación

Incluso en una aplicación que envía mensajes a una alta frecuencia (como una aplicación de juegos en tiempo real), la mayoría de las aplicaciones no necesitan enviar más de unos pocos mensajes por segundo. Para reducir la cantidad de tráfico que genera cada cliente, se puede implementar un bucle de mensajes que pone en cola y envía mensajes no con más frecuencia que una velocidad fija (es decir, se enviarán hasta un cierto número de mensajes cada segundo, si hay mensajes en ese intervalo de tiempo que se enviarán). Para obtener una aplicación de ejemplo que limita los mensajes a una velocidad determinada (tanto desde el cliente como desde el servidor), consulte [Tiempo real de alta frecuencia con SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Reducción del tamaño del mensaje

Puede reducir el tamaño de un mensaje de SignalR reduciendo el tamaño de los objetos serializados. En el código del servidor, si va a enviar un objeto que contiene propiedades que no es `JsonIgnore` necesario transmitir, evite que esas propiedades se serializan mediante el atributo. Los nombres de las propiedades también se almacenan en el mensaje; los nombres de las propiedades se `JsonProperty` pueden acortar mediante el atributo. En el ejemplo de código siguiente se muestra cómo excluir una propiedad de ser enviada al cliente y cómo acortar los nombres de propiedad:

**Código de servidor .NET que muestra el atributo JsonIgnore para excluir los datos de los datos que se envían al cliente y el atributo JsonProperty para reducir el tamaño del mensaje**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Para conservar la legibilidad/la capacidad de mantenimiento en el código de cliente, los nombres de propiedad abreviados se pueden reasignar a nombres descriptivos después de recibir el mensaje. En el ejemplo de código siguiente se muestra una posible forma de reasignar nombres abreviados a `reMap` otros más largos, definiendo un contrato de mensaje (asignación) y utilizando la función para aplicar el contrato a la clase de mensaje optimizada:

**Código JavaScript del lado cliente que reasigna nombres de propiedad abreviados a nombres legibles por humanos**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

Los nombres también se pueden acortar en los mensajes del cliente al servidor, utilizando el mismo método.

La reducción de la huella de memoria (es decir, la cantidad de memoria utilizada para el mensaje) del objeto de mensaje también puede mejorar el rendimiento. Por ejemplo, si no `int` se necesita el `short` `byte` rango completo de un, se puede usar un o se puede usar en su lugar.

Puesto que los mensajes se almacenan en el bus de mensajes en la memoria del servidor, la reducción del tamaño de los mensajes también puede solucionar problemas de memoria del servidor.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Ajuste del servidor SignalR para el rendimiento

Los siguientes valores de configuración se pueden usar para ajustar el servidor para un mejor rendimiento en una aplicación de SignalR. Para obtener información general sobre cómo mejorar el rendimiento en una aplicación ASP.NET, consulte [Mejora del rendimiento ASP.NET.](https://msdn.microsoft.com/library/ff647787.aspx)

**Configuración de SignalR**

- **DefaultMessageBufferSize**: De forma predeterminada, SignalR conserva 1000 mensajes en memoria por concentrador por conexión. Si se utilizan mensajes grandes, esto puede crear problemas de memoria que se pueden aliviar reduciendo este valor. Esta configuración se puede `Application_Start` establecer en el controlador de `Configuration` eventos en una aplicación de ASP.NET o en el método de una clase de inicio OWIN en una aplicación autohospedada. En el ejemplo siguiente se muestra cómo reducir este valor para reducir la huella de memoria de la aplicación para reducir la cantidad de memoria de servidor utilizada:

    **Código de servidor .NET en Startup.cs para disminuir el tamaño predeterminado del búfer de mensajes**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**Configuración de IIS**

- Máximo de **solicitudes simultáneas por aplicación:** aumentar el número de solicitudes de IIS simultáneas aumentará los recursos del servidor disponibles para atender solicitudes. El valor predeterminado es 5000; para aumentar esta configuración, ejecute los siguientes comandos en un símbolo del sistema con privilegios elevados:

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- **ApplicationPool QueueLength**: Este es el número máximo de solicitudes que Http.sys pone en cola para el grupo de aplicaciones. Cuando la cola está llena, las nuevas solicitudes reciben una respuesta 503 "Servicio no disponible". El valor predeterminado es 1000.

    Acortar la longitud de la cola para el proceso de trabajo en el grupo de aplicaciones que hospeda la aplicación conserve los recursos de memoria. Para obtener más información, consulte [Administración, optimización y configuración de](https://technet.microsoft.com/library/cc745955.aspx)grupos de aplicaciones .

**ASP.NET configuración**

Esta sección incluye los valores de `aspnet.config` configuración que se pueden establecer en el archivo. Este archivo se encuentra en una de las dos ubicaciones, dependiendo de la plataforma:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

ASP.NET configuración que puede mejorar el rendimiento de SignalR incluyen lo siguiente:

- **Máximo de solicitudes simultáneas por CPU:** aumentar esta configuración puede aliviar los cuellos de botella de rendimiento. Para aumentar esta configuración, agregue la `aspnet.config` siguiente configuración al archivo:

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Límite**de cola de solicitudes: cuando `maxConcurrentRequestsPerCPU` el número total de conexiones supera la configuración, ASP.NET comenzará a limitar las solicitudes mediante una cola. Para aumentar el tamaño de la `requestQueueLimit` cola, puede aumentar la configuración. Para ello, agregue la siguiente `processModel` configuración al `config/machine.config` nodo `aspnet.config`en (en lugar de ):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Solución de problemas de rendimiento

En esta sección se describen formas de encontrar cuellos de botella de rendimiento en la aplicación.

### <a name="verifying-that-websocket-is-being-used"></a>Comprobación de que se está utilizando WebSocket

Aunque SignalR puede usar una variedad de transportes para la comunicación entre el cliente y el servidor, WebSocket ofrece una ventaja de rendimiento significativa y debe usarse si el cliente y el servidor lo admiten. Para determinar si el cliente y el servidor cumplen los requisitos de WebSocket, vea [Transportes y reserva.](../getting-started/introduction-to-signalr.md#transports) Para determinar qué transporte se usa en la aplicación, puede usar las herramientas de desarrollador del explorador y examinar los registros para ver qué transporte se usa para la conexión. Para obtener información sobre el uso de las herramientas de desarrollo del navegador en Internet Explorer y Chrome, consulte [Transportes y reserva .](../getting-started/introduction-to-signalr.md#transports)

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>Uso de contadores de rendimiento de SignalR

En esta sección se describe cómo habilitar y utilizar `Microsoft.AspNet.SignalR.Utils` los contadores de rendimiento de SignalR, que se encuentran en el paquete.

### <a name="installing-signalrexe"></a>Instalación de signalr.exe

Los contadores de rendimiento se pueden agregar al servidor mediante una utilidad denominada SignalR.exe. Para instalar esta utilidad, siga estos pasos:

1. En Visual Studio, seleccione **Herramientas** > **NuGet Package Manager** > **Administrar paquetes NuGet para la solución**
2. Busque **signalr.utils**y seleccione Instalar.

    ![](signalr-performance/_static/image1.png)
3. Acepte el acuerdo de licencia para instalar el paquete.
4. SignalR.exe se instalará `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`en .

### <a name="installing-performance-counters-with-signalrexe"></a>Instalación de contadores de rendimiento con SignalR.exe

Para instalar contadores de rendimiento de SignalR, ejecute SignalR.exe en un símbolo del sistema con privilegios elevados con el siguiente parámetro:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Para quitar los contadores de rendimiento de SignalR, ejecute SignalR.exe en un símbolo del sistema con privilegios elevados con el siguiente parámetro:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>Contadores de rendimiento de SignalR

El paquete de utilidades instala los siguientes contadores de rendimiento. Los contadores "Total" miden el número de eventos desde el último grupo de aplicaciones o el reinicio del servidor.

**Métricas de conexión**

Las siguientes métricas miden los eventos de duración de la conexión que se producen. Para obtener más información, consulte [Descripción y control](../guide-to-the-api/handling-connection-lifetime-events.md)de eventos de duración de la conexión .

- **Conexiones conectadas**
- **Conexiones reconectadas**
- **Conexiones desconectadas**
- **Conexiones actuales**

**Métricas de mensaje**

Las siguientes métricas miden el tráfico de mensajes generado por SignalR.

- **Mensajes de conexión Total recibidos**
- **Mensajes de conexión enviados total**
- **Mensajes de conexión recibidos/seg**
- **Mensajes de conexión enviados por segundo**

**Métricas de bus de mensajes**

Las métricas siguientes miden el tráfico a través del bus de mensajes de SignalR interno, la cola en la que se colocan todos los mensajes de SignalR entrantes y salientes. Un mensaje se **publica** cuando se envía o se difunde. Un **suscriptor** en este contexto es una suscripción en el bus de mensajes; esto debe ser igual al número de clientes más el propio servidor. Un **trabajador asignado** es un componente que envía datos a conexiones activas; un **Trabajador Ocupado** es aquel que está enviando activamente un mensaje.

- **Mensajes de bus de mensajes recibidos Total**
- **Mensajes de bus de mensajes recibidos/seg**
- **Mensajes de bus de mensajes publicados Total**
- **Mensajes de bus de mensajes publicados/seg**
- **Suscriptores de Bus de mensajes actuales**
- **Total de suscriptores de Bus de mensajes**
- **Suscriptores de Bus de mensajes/Sec**
- **Mensaje Bus Asignado Trabajadores**
- **Mensaje Bus Trabajadores ocupados**
- **Temas de bus de mensajes actuales**

**Métricas de error**

Las siguientes métricas miden los errores generados por el tráfico de mensajes de SignalR. Los errores de **resolución** de concentradores se producen cuando no se puede resolver un método de concentrador o concentrador. Los errores de **invocación** de concentrador son excepciones que se producen al invocar un método de concentrador. **Los** errores de transporte son errores de conexión producidos durante una solicitud o respuesta HTTP.

- **Errores: Total Total**
- **Errores: All/Sec**
- **Errores: Total de resolución del concentrador**
- **Errores: Resolución del concentrador/Sec**
- **Errores: Total de invocación de concentrador**
- **Errores: Invocación de Hub/Sec**
- **Errores: Total de transporte**
- **Errores: Transporte/Sec**

<a id="scaleout_metrics"></a>

**Métricas de escalado horizontal**

Las siguientes métricas miden el tráfico y los errores generados por el proveedor de escalado horizontal. Un **Stream** en este contexto es una unidad de escalado utilizada por el proveedor de escalado horizontal; se trata de una tabla si se usa SQL Server, un tema si se usa Service Bus y una suscripción si se usa Redis. Cada secuencia garantiza operaciones ordenadas de lectura y escritura; una sola secuencia es un cuello de botella de escala potencial, por lo que el número de secuencias se puede aumentar para ayudar a reducir ese cuello de botella. Si se utilizan varias secuencias, SignalR distribuirá automáticamente mensajes (partición) a través de estas secuencias de una manera que garantice que los mensajes enviados desde cualquier conexión dada estén en orden.

El [valor MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) controla la longitud de la cola de envío de escalado horizontal mantenida por SignalR. Si se establece en un valor mayor que 0, todos los mensajes se colocarán en una cola de envío para enviarlos de uno en uno al backplane de mensajería configurado. Si el tamaño de la cola supera la longitud configurada, las llamadas posteriores a enviar producirán un error inmediatamente con una [excepción InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) hasta que el número de mensajes de la cola sea menor que la configuración de nuevo. La cola está deshabilitada de forma predeterminada porque los backplanes implementados generalmente tienen su propia cola o control de flujo en su lugar. En el caso de SQL ServerSQL Server, la agrupación de conexiones limita eficazmente el número de envíos que se están sucediendo en cualquier momento.

De forma predeterminada, solo se usa una secuencia para SQL Server y Redis, se usan cinco secuencias para Service Bus y la cola está deshabilitada, pero esta configuración se puede cambiar a través de la configuración en SQL ServerSQL Server y Service Bus:

**Código de servidor .NET para configurar el recuento de tablas y la longitud de la cola para el backplane de SQL Server**

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

**.NET Server Code para configurar el recuento de temas y la longitud de cola para el backplane de Service Bus**

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

Una secuencia **de almacenamiento** en búfer es una que ha entrado en un estado con errores; cuando la secuencia está en el estado con errores, todos los mensajes enviados al backplane fallarán inmediatamente hasta que la secuencia ya no esté fallando. La longitud de la cola de **envío** es el número de mensajes que se han publicado pero que aún no se han enviado.

- **Mensajes de bus de mensajes de escalabilidad horizontal recibidos/seg**
- **Scaleout Streams Total**
- **Scaleout Streams Open**
- **Escalado horizontal Streams Buffering**
- **Total de errores de escalado**
- **Errores de escalado horizontal/seg**
- **Longitud de la cola de envío de escalado horizontal**

Para obtener más información sobre lo que miden estos contadores, vea Escalado horizontal de [SignalR con Azure Service Bus](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Uso de otros contadores de rendimiento

Los siguientes contadores de rendimiento también pueden ser útiles para supervisar el rendimiento de la aplicación.

**Memoria**

- Memoria\\de .NET CLR: bytes en todos los montones (para w3wp)

**ASP.NET**

- ASP.NET-Solicitudes actuales
- ASP.NET-Queued
- ASP.NET-Rejected

**Cpu**

- Información del procesador-Tiempo del procesador

**TCP/IP**

- TCPv6/Conexiones establecidas
- TCPv4/Conexiones establecidas

**Servicio web**

- Servicio web- Conexiones actuales
- Servicio web- Conexiones máximas

**Subprocesos**

- Bloqueos y subprocesos\\de .NET CLR : de los subprocesos lógicos actuales
- Bloqueos y subprocesos\\de .NET CLR : de los subprocesos físicos actuales

<a id="otherresources"></a>

## <a name="other-resources"></a>Otros recursos

Para obtener más información sobre ASP.NET la supervisión y el ajuste del rendimiento, consulte los temas siguientes:

- [Información general acerca del rendimiento de ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [ASP.NET el uso de subprocesos en IIS 7.5, IIS 7.0 e IIS 6.0](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;Elemento&gt; applicationPool (Configuración web)](https://msdn.microsoft.com/library/dd560842.aspx)
