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
# <a name="aspnet-signalr-hubs-api-guide---net-client-c"></a>Guía de la API de ASP.NET SignalR Hubs- Cliente .NET (C-)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este documento proporciona una introducción al uso de la API de Hubs para SignalR versión 2 en clientes .NET, como Windows Store (WinRT), WPF, Silverlight y aplicaciones de consola.
>
> La API de SignalR Hubs le permite realizar llamadas a procedimientos remotos (RPC) desde un servidor a clientes conectados y desde clientes al servidor. En el código de servidor, se definen métodos a los que pueden llamar los clientes y se llama a métodos que se ejecutan en el cliente. En el código de cliente, se definen métodos a los que se puede llamar desde el servidor y se llama a métodos que se ejecutan en el servidor. SignalR se encarga de todas las tuberías de cliente a servidor por usted.
>
> SignalR también ofrece una API de nivel inferior denominada Conexiones persistentes. Para obtener una introducción a SignalR, concentradores y conexiones persistentes, o para un tutorial que muestra cómo crear una aplicación de SignalR completa, vea [SignalR - Introducción](../getting-started/index.md).
>
> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software utilizadas en este tema
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
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

## <a name="overview"></a>Información general

Este documento contiene las siguientes secciones:

- [Configuración del cliente](#clientsetup)
- [Cómo establecer una conexión](#establishconnection)

    - [Conexiones entre dominios de clientes de Silverlight](#slcrossdomain)
- [Cómo configurar la conexión](#configureconnection)

    - [Cómo establecer el número máximo de conexiones simultáneas en los clientes de WPFWPF](#maxconnections)
    - [Cómo especificar parámetros de cadena de consulta](#querystring)
    - [Cómo especificar el método de transporte](#transport)
    - [Cómo especificar encabezados HTTP](#httpheaders)
    - [Cómo especificar certificados de cliente](#clientcertificate)
- [Cómo crear el proxy de Hub](#proxy)
- [Cómo definir métodos en el cliente a los que el servidor puede llamar](#callclient)

    - [Métodos sin parámetros](#clientmethodswithoutparms)
    - [Métodos con parámetros, especificando tipos de parámetros](#clientmethodswithparmtypes)
    - [Métodos con parámetros, especificando objetos dinámicos para los parámetros](#clientmethodswithdynamparms)
    - [Cómo quitar un controlador](#removehandler)
- [Cómo llamar a métodos de servidor desde el cliente](#callserver)
- [Cómo controlar los eventos de duración de la conexión](#connectionlifetime)
- [Cómo manejar los errores](#handleerrors)
- [Cómo habilitar el registro del lado cliente](#logging)
- [EJEMPLOs de código de aplicación de wpf, Silverlight y consola para métodos de cliente a los que el servidor puede llamar](#wpfsl)

Para ver un ejemplo de proyectos de cliente de .NET, vea los siguientes recursos:

- [gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) en GitHub.com (WinRT, Silverlight, ejemplos de aplicaciones de consola).
- [DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) en GitHub.com (ejemplo DEWPF).
- [SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) en GitHub.com (ejemplo de aplicación de consola).

Para obtener documentación sobre cómo programar el servidor o los clientes JavaScript, consulte los siguientes recursos:

- [Guía de la API de SignalR Hubs - Servidor](hubs-api-guide-server.md)
- [SignalR Hubs API Guide - Cliente JavaScript](hubs-api-guide-javascript-client.md)

Los vínculos a temas de Referencia de API se vinculan a la versión 4.5 de la API. Si usa .NET 4, consulte [la versión de .NET 4 de los temas de la API.](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)

<a id="clientsetup"></a>

## <a name="client-setup"></a>Configuración del cliente

Instale el paquete NuGet [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) (no el paquete [Microsoft.AspNet.SignalR).](http://nuget.org/packages/microsoft.aspnet.signalr) Este paquete admite WinRT, Silverlight, WPF, aplicación de consola y clientes de Windows Phone, tanto para .NET 4 como para .NET 4.5.

Si la versión de SignalR que tiene en el cliente es diferente de la versión que tiene en el servidor, SignalR a menudo puede adaptarse a la diferencia. Por ejemplo, un servidor que ejecute SignalR versión 2 admitirá clientes que tengan 1.1.x instalado, así como clientes que tengan instalada la versión 2. Si la diferencia entre la versión en el servidor y la versión en el cliente es `InvalidOperationException` demasiado grande, o si el cliente es más reciente que el servidor, SignalR produce una excepción cuando el cliente intenta establecer una conexión. El mensaje de`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`error es " ".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Cómo establecer una conexión

Antes de poder establecer una conexión, `HubConnection` debe crear un objeto y crear un proxy. Para establecer la conexión, llame al `Start` método en el `HubConnection` objeto.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,5)]

> [!NOTE]
> Para los clientes de JavaScript debe registrar `Start` al menos un controlador de eventos antes de llamar al método para establecer la conexión. Esto no es necesario para los clientes .NET. Para los clientes JavaScript, el código proxy generado crea automáticamente servidores proxy para todos los concentradores que existen en el servidor y el registro de un controlador es la forma en que se indica qué concentradores piensa usar el cliente. Pero para un cliente de .NET se crean servidores proxy de concentrador manualmente, por lo que SignalR supone que va a usar cualquier concentrador para el que cree un proxy.

El código de ejemplo utiliza la dirección URL predeterminada "/signalr" para conectarse al servicio SignalR. Para obtener información sobre cómo especificar una dirección URL base diferente, consulte ASP.NET Guía de la API de [SignalR Hubs - Servidor - La dirección URL de /signalr](hubs-api-guide-server.md#signalrurl).

El `Start` método se ejecuta de forma asincrónica. Para asegurarse de que las líneas de código posteriores `await` no se ejecutan hasta después de establecer la conexión, utilice en un método asincrónico ASP.NET 4.5 o `.Wait()` en un método sincrónico. No usar `.Wait()` en un cliente WinRT.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Conexiones entre dominios de clientes de Silverlight

Para obtener información sobre cómo habilitar conexiones entre dominios desde clientes de Silverlight, vea [Hacer que un servicio esté disponible en todos los límites de dominio](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Cómo configurar la conexión

Antes de establecer una conexión, puede especificar cualquiera de las siguientes opciones:

- Límite de conexiones simultáneas.
- Parámetros de cadena de consulta.
- El método de transporte.
- Encabezados HTTP.
- Certificados de cliente.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>Cómo establecer el número máximo de conexiones simultáneas en los clientes de WPFWPF

En los clientes de WPFWPF, es posible que tenga que aumentar el número máximo de conexiones simultáneas a partir de su valor predeterminado de 2. El valor recomendado es 10.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=5)]

Para obtener más información, vea [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Cómo especificar parámetros de cadena de consulta

Si desea enviar datos al servidor cuando el cliente se conecta, puede agregar parámetros de cadena de consulta al objeto de conexión. En el ejemplo siguiente se muestra cómo establecer un parámetro de cadena de consulta en el código de cliente.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

En el ejemplo siguiente se muestra cómo leer un parámetro de cadena de consulta en el código del servidor.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Cómo especificar el método de transporte

Como parte del proceso de conexión, un cliente de SignalR normalmente negocia con el servidor para determinar el mejor transporte que es compatible con el servidor y el cliente. Si ya sabe qué transporte desea utilizar, puede omitir este proceso de negociación. Para especificar el método de transporte, pase un objeto de transporte al método Start. En el ejemplo siguiente se muestra cómo especificar el método de transporte en el código de cliente.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=5)]

El [espacio de nombres Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) incluye las siguientes clases que puede usar para especificar el transporte.

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponible solo cuando el servidor y el cliente usan .NET 4.5.)
- [AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (elige automáticamente el mejor transporte que admite tanto el cliente como el servidor. Este es el transporte predeterminado. Pasar esto al `Start` método tiene el mismo efecto que no pasar nada.)

El transporte ForeverFrame no se incluye en esta lista porque solo lo utilizan los exploradores.

Para obtener información acerca de cómo comprobar el método de transporte en el código del servidor, consulte ASP.NET Guía de la API de [SignalR Hubs - Servidor - Cómo obtener información sobre el cliente desde la propiedad Context](hubs-api-guide-server.md#contextproperty). Para obtener más información acerca de los transportes y las reservaciones, vea [Introducción a SignalR - Transportes y reserva .](../getting-started/introduction-to-signalr.md#transports)

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Cómo especificar encabezados HTTP

Para establecer encabezados HTTP, utilice la `Headers` propiedad del objeto de conexión. En el ejemplo siguiente se muestra cómo agregar un encabezado HTTP.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Cómo especificar certificados de cliente

Para agregar certificados de `AddClientCertificate` cliente, utilice el método en el objeto de conexión.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Cómo crear el proxy de Hub

Para definir métodos en el cliente que un concentrador puede llamar desde el servidor e invocar métodos `CreateHubProxy` en un concentrador en el servidor, cree un proxy para el concentrador llamando al objeto de conexión. La cadena a `CreateHubProxy` la que se pasa es el nombre de `HubName` la clase Hub o el nombre especificado por el atributo si se usó uno en el servidor. La coincidencia de nombres no distingue mayúsculas y minúsculas.

**Clase Hub en servidor**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Crear proxy de cliente para la clase Hub**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=3)]

Si decora sacada `HubName` la clase Hub con un atributo, use ese nombre.

**Clase Hub en servidor**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

**Crear proxy de cliente para la clase Hub**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=3)]

Si llama `HubConnection.CreateHubProxy` varias veces `hubName`con el mismo `IHubProxy` , obtendrá el mismo objeto almacenado en caché.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Cómo definir métodos en el cliente a los que el servidor puede llamar

Para definir un método al que el servidor `On` puede llamar, use el método del proxy para registrar un controlador de eventos.

La coincidencia de nombres de método no distingue mayúsculas de minúsculas. Por `Clients.All.UpdateStockPrice` ejemplo, en el `updateStockPrice` `updatestockprice`servidor `UpdateStockPrice` se ejecutará , , o en el cliente.

Diferentes plataformas de cliente tienen requisitos diferentes para escribir código de método para actualizar la interfaz de usuario. Los ejemplos que se muestran son para clientes de WinRT (Windows Store .NET). WPFWPF, Silverlight y ejemplos de aplicaciones de consola se proporcionan en [una sección independiente más adelante en este tema.](#wpfsl)

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Métodos sin parámetros

Si el método que está controlando no tiene parámetros, utilice la sobrecarga no genérica del `On` método:

**Código de servidor que llama al método cliente sin parámetros**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**Código de cliente WinRT para el método al que se llama desde el servidor sin parámetros[(vea ejemplos de WPF y Silverlight más adelante en este tema)](#wpfsl)**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Métodos con parámetros, especificando los tipos de parámetros

Si el método que está controlando tiene parámetros, especifique `On` los tipos de los parámetros como los tipos genéricos del método. Hay sobrecargas genéricas `On` del método para permitirle especificar hasta 8 parámetros (4 en Windows Phone 7). En el ejemplo siguiente, se `UpdateStockPrice` envía un parámetro al método.

**Método de cliente de llamada de código de servidor con un parámetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**La clase Stock utilizada para el parámetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

**Código de cliente WinRT para un método al que se llama desde el servidor con un parámetro[(vea WPF y ejemplos de Silverlight más adelante en este tema)](#wpfsl)**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Métodos con parámetros, especificando objetos dinámicos para los parámetros

Como alternativa a la especificación de `On` parámetros como tipos genéricos del método, puede especificar parámetros como objetos dinámicos:

**Método de cliente de llamada de código de servidor con un parámetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**La clase Stock utilizada para el parámetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

**Código de cliente WinRT para un método al que se llama desde el servidor con un parámetro, mediante un objeto dinámico para el parámetro[(consulte ejemplos de WPFwpf y Silverlight más adelante en este tema)](#wpfsl)**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Cómo quitar un controlador

Para quitar un controlador, llame a su `Dispose` método.

**Código de cliente para un método llamado desde el servidor**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Código de cliente para quitar el controlador**

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Cómo llamar a métodos de servidor desde el cliente

Para llamar a un método `Invoke` en el servidor, utilice el método en el proxy de concentrador.

Si el método de servidor no tiene ningún valor `Invoke` devuelto, utilice la sobrecarga no genérica del método.

**Código de servidor para un método que no tiene ningún valor devuelto**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Código de cliente que llama a un método que no tiene ningún valor devuelto**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Si el método de servidor tiene un valor devuelto, `Invoke` especifique el tipo de valor devuelto como el tipo genérico del método.

**Código de servidor para un método que tiene un valor devuelto y toma un parámetro de tipo complejo**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**La clase Stock utilizada para el parámetro y el valor devuelto**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

**Código de cliente que llama a un método que tiene un valor devuelto y toma un parámetro de tipo complejo, en un método asincrónico ASP.NET 4.5**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**Código de cliente que llama a un método que tiene un valor devuelto y toma un parámetro de tipo complejo, en un método sincrónico**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

El `Invoke` método se ejecuta de `Task` forma asincrónica y devuelve un objeto. Si no especifica `await` o `.Wait()`, la siguiente línea de código se ejecutará antes de que el método que invoque haya terminado de ejecutarse.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Cómo controlar los eventos de duración de la conexión

SignalR proporciona los siguientes eventos de duración de conexión que puede controlar:

- `Received`: Se genera cuando se reciben datos en la conexión. Proporciona los datos recibidos.
- `ConnectionSlow`: Se genera cuando el cliente detecta una conexión lenta o que se cae con frecuencia.
- `Reconnecting`: Se genera cuando el transporte subyacente comienza a volver a conectarse.
- `Reconnected`: Se genera cuando el transporte subyacente se ha vuelto a conectar.
- `StateChanged`: se genera cuando cambia el estado de conexión. Proporciona el estado antiguo y el nuevo estado. Para obtener información acerca de los valores de estado de conexión, vea [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: Se genera cuando la conexión se ha desconectado.

Por ejemplo, si desea mostrar mensajes de advertencia para errores que no son fatales pero causan problemas `ConnectionSlow` de conexión intermitentes, como lentitud o caída frecuente de la conexión, controle el evento.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

Para obtener más información, consulte Descripción y control de eventos de duración de [conexión en SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Cómo manejar los errores

Si no habilita explícitamente los mensajes de error detallados en el servidor, el objeto de excepción que SignalR devuelve después de un error contiene información mínima sobre el error. Por ejemplo, si `newContosoChatMessage` se produce un error en una`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`llamada, el mensaje de error del objeto de error contiene " " Enviar mensajes de error detallados a los clientes en producción no se recomienda por motivos de seguridad, pero si desea habilitar mensajes de error detallados para solucionar problemas, utilice el código siguiente en el servidor.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Para controlar los errores que SignalR genera, `Error` puede agregar un controlador para el evento en el objeto de conexión.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

Para controlar los errores de invocaciones de método, ajuste el código en un bloque try-catch.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Cómo habilitar el registro del lado cliente

Para habilitar el registro del `TraceLevel` `TraceWriter` lado cliente, establezca las propiedades y en el objeto de conexión.

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=3-4)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>EJEMPLOs de código de aplicación de wpf, Silverlight y consola para métodos de cliente a los que el servidor puede llamar

Los ejemplos de código mostrados anteriormente para definir métodos de cliente a los que el servidor puede llamar se aplican a los clientes de WinRT. En los ejemplos siguientes se muestra el código equivalente para WPFWPF, Silverlight y clientes de aplicación de consola.

### <a name="methods-without-parameters"></a>Métodos sin parámetros

**Código de cliente WPF para el método llamado desde el servidor sin parámetros**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Código de cliente Silverlight para el método llamado desde el servidor sin parámetros**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Código de cliente de aplicación de consola para el método llamado desde el servidor sin parámetros**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Métodos con parámetros, especificando los tipos de parámetros

**Código de cliente WPF para un método al que se llama desde el servidor con un parámetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Código de cliente Silverlight para un método llamado desde el servidor con un parámetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Código de cliente de aplicación de consola para un método al que se llama desde el servidor con un parámetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Métodos con parámetros, especificando objetos dinámicos para los parámetros

**Código de cliente WPF para un método al que se llama desde el servidor con un parámetro, mediante un objeto dinámico para el parámetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Código de cliente Silverlight para un método al que se llama desde el servidor con un parámetro, utilizando un objeto dinámico para el parámetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Código de cliente de aplicación de consola para un método al que se llama desde el servidor con un parámetro, utilizando un objeto dinámico para el parámetro**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
