---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: Guía de la API de ASP.NET SignalR Hubs - Cliente de JavaScript Microsoft Docs
author: bradygaster
description: Este documento proporciona una introducción al uso de la API de Hubs para SignalR versión 2 en clientes JavaScript, como exploradores y aplicación de la Tienda Windows (WinJS).
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 8befe133c3627dac1f7d011959c68e2054d345da
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675716"
---
# <a name="aspnet-signalr-hubs-api-guide---javascript-client"></a>Guía de la API de ASP.NET SignalR Hubs - Cliente JavaScript

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este documento proporciona una introducción al uso de la API de Hubs para SignalR versión 2 en clientes JavaScript, como exploradores y aplicaciones de la Tienda Windows (WinJS).
>
> La API de SignalR Hubs le permite realizar llamadas a procedimientos remotos (RPC) desde un servidor a clientes conectados y desde clientes al servidor. En el código de servidor, se definen métodos a los que pueden llamar los clientes y se llama a métodos que se ejecutan en el cliente. En el código de cliente, se definen métodos a los que se puede llamar desde el servidor y se llama a métodos que se ejecutan en el servidor. SignalR se encarga de todas las tuberías de cliente a servidor por usted.
>
> SignalR también ofrece una API de nivel inferior denominada Conexiones persistentes. Para obtener una introducción a SignalR, concentradores y conexiones persistentes, vea [Introducción a SignalR](../getting-started/introduction-to-signalr.md).
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

- [El proxy generado y lo que hace por usted](#genproxy)

    - [Cuándo utilizar el proxy generado](#cantusegenproxy)
- [Configuración del cliente](#clientsetup)

    - [Cómo hacer referencia al proxy generado dinámicamente](#dynamicproxy)
    - [Cómo crear un archivo físico para el proxy generado por SignalR](#manualproxy)
- [Cómo establecer una conexión](#establishconnection)

    - [$.connection.hub es el mismo objeto que $.hubConnection() crea](#connequivalence)
    - [Ejecución asincrónica del método start](#asyncstart)
- [Cómo establecer una conexión entre dominios](#crossdomain)
- [Cómo configurar la conexión](#configureconnection)

    - [Cómo especificar parámetros de cadena de consulta](#querystring)
    - [Cómo especificar el método de transporte](#transport)
- [Cómo obtener un proxy para una clase Hub](#getproxy)
- [Cómo definir métodos en el cliente a los que el servidor puede llamar](#callclient)
- [Cómo llamar a métodos de servidor desde el cliente](#callserver)
- [Cómo controlar los eventos de duración de la conexión](#connectionlifetime)
- [Cómo manejar los errores](#handleerrors)
- [Cómo habilitar el registro del lado cliente](#logging)

Para obtener documentación sobre cómo programar el servidor o los clientes .NET, consulte los siguientes recursos:

- [Guía de la API de SignalR Hubs - Servidor](hubs-api-guide-server.md)
- [SignalR Hubs API Guide - .NET Client](hubs-api-guide-net-client.md)

El componente de servidor SignalR 2 solo está disponible en .NET 4.5 (aunque hay un cliente de .NET para SignalR 2 en .NET 4.0).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>El proxy generado y lo que hace por usted

Puede programar un cliente JavaScript para comunicarse con un servicio SignalR con o sin un proxy que SignalR genera para usted. Lo que el proxy hace por usted es simplificar la sintaxis del código que se usa para conectarse, escribir métodos a los que llama el servidor y llamar a métodos en el servidor.

Al escribir código para llamar a métodos de servidor, el proxy generado le permite usar una `serverMethod(arg1, arg2)` sintaxis que parece que estaba ejecutando una función local: puede escribir en lugar de `invoke('serverMethod', arg1, arg2)`. La sintaxis de proxy generada también permite un error inmediato e inteligible del lado cliente si escribe incorrectamente un nombre de método de servidor. Y si crea manualmente el archivo que define los servidores proxy, también puede obtener compatibilidad con IntelliSense para escribir código que llame a métodos de servidor.

Por ejemplo, supongamos que tiene la siguiente clase Hub en el servidor:

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

Los siguientes ejemplos de código muestran el `NewContosoChatMessage` aspecto del código JavaScript para `addContosoChatMessageToPage` invocar el método en el servidor y recibir invocaciones del método desde el servidor.

**Con el proxy generado**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Sin el proxy generado**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Cuándo utilizar el proxy generado

Si desea registrar varios controladores de eventos para un método de cliente al que llama el servidor, no puede usar el proxy generado. De lo contrario, puede optar por utilizar el proxy generado o no en función de su preferencia de codificación. Si decide no usarlo, no tiene que hacer referencia a la dirección URL `script` "signalr/hubs" en un elemento del código de cliente.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Configuración del cliente

Un cliente JavaScript requiere referencias a jQuery y al archivo JavaScript principal de SignalR. La versión de jQuery debe ser 1.6.4 o versiones posteriores principales, como 1.7.2, 1.8.2 o 1.9.1. Si decide utilizar el proxy generado, también necesita una referencia al archivo JavaScript de proxy generado por SignalR. En el ejemplo siguiente se muestra el aspecto que podrían tener las referencias en una página HTML que usa el proxy generado.

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

Estas referencias deben incluirse en este orden: jQuery primero, el núcleo de SignalR después de eso y los proxies de SignalR en último lugar.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Cómo hacer referencia al proxy generado dinámicamente

En el ejemplo anterior, la referencia al proxy generado por SignalR es al código JavaScript generado dinámicamente, no a un archivo físico. SignalR crea el código JavaScript para el proxy sobre la marcha y lo sirve al cliente en respuesta a la dirección URL "/signalr/hubs". Si especificó una dirección URL base diferente para `MapSignalR` las conexiones de SignalR en el servidor del método, la dirección URL del archivo proxy generado dinámicamente es la dirección URL personalizada con "/hubs" anexado a él.

> [!NOTE]
> Para los clientes JavaScript de Windows 8 (Windows Store), use el archivo proxy físico en lugar del generado dinámicamente. Para obtener más información, vea [Cómo crear un archivo físico para el proxy generado SignalR](#manualproxy) más adelante en este tema.

En una vista de Razor de ASP.NET MVC 4 o 5, use la tilde para hacer referencia a la raíz de la aplicación en la referencia del archivo proxy:

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

Para obtener más información sobre el uso de SignalR en MVC 5, vea [Introducción a SignalR y MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

En una vista de Razor `Url.Content` de ASP.NET MVC 3, úselo para la referencia del archivo proxy:

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

En una aplicación de `ResolveClientUrl` formularios Web Forms ASP.NET, use para la referencia de archivo de proxies o regístrela a través de ScriptManager mediante una ruta de acceso relativa raíz de la aplicación (comenzando con una tilde):

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

Como regla general, utilice el mismo método para especificar la dirección URL "/signalr/hubs" que utiliza para los archivos CSS o JavaScript. Si especifica una dirección URL sin usar una tilde, en algunos escenarios la aplicación funcionará correctamente al probar en Visual Studio mediante IIS Express, pero se producirá un error 404 al implementar en IIS completo. Para obtener más información, vea **Resolver referencias a recursos de nivel raíz** en servidores web en Visual Studio para ASP.NET proyectos [web](https://msdn.microsoft.com/library/58wxa9w5.aspx) en el sitio MSDN.

Al ejecutar un proyecto web en Visual Studio 2017 en modo de depuración y, si usa Internet Explorer como explorador, puede ver el archivo proxy en el Explorador de **soluciones** en **Scripts**.

Para ver el contenido del archivo, haga doble clic en **hubs**. Si no usa Visual Studio 2012 o 2013 e Internet Explorer, o si no está en modo de depuración, también puede obtener el contenido del archivo navegando a la dirección URL "/signalR/hubs". Por ejemplo, si su `http://localhost:56699`sitio se `http://localhost:56699/SignalR/hubs` está ejecutando en , vaya a su navegador.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Cómo crear un archivo físico para el proxy generado por SignalR

Como alternativa al proxy generado dinámicamente, puede crear un archivo físico que tenga el código proxy y hacer referencia a ese archivo. Es posible que desee hacerlo para controlar el comportamiento de almacenamiento en caché o agrupación, o para obtener IntelliSense al codificar llamadas a métodos de servidor.

Para crear un archivo proxy, siga estos pasos:

1. Instale el paquete NuGet [Microsoft.AspNet.SignalR.Utils.](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/)
2. Abra un símbolo del sistema y vaya a la carpeta *de herramientas* que contiene el archivo SignalR.exe. La carpeta tools se encuentra en la siguiente ubicación:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. Escriba el comando siguiente:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    La ruta de acceso al *archivo .dll* suele ser la carpeta *bin* de la carpeta del proyecto.

    Este comando crea un archivo denominado *server.js* en la misma carpeta que *signalr.exe*.
4. Coloque el archivo *server.js* en una carpeta adecuada del proyecto, cámbiele el nombre según corresponda a la aplicación y agregue una referencia a él en lugar de la referencia "signalr/hubs".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Cómo establecer una conexión

Para poder establecer una conexión, debe crear un objeto de conexión, crear un proxy y registrar controladores de eventos para los métodos a los que se puede llamar desde el servidor. Cuando se configuren los controladores de proxy y `start` eventos, establezca la conexión llamando al método.

Si usa el proxy generado, no tiene que crear el objeto de conexión en su propio código porque el código proxy generado lo hace por usted.

<a id="nogenconnection"></a>

**Establecer una conexión (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Establecer una conexión (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

El código de ejemplo utiliza la dirección URL predeterminada "/signalr" para conectarse al servicio SignalR. Para obtener información sobre cómo especificar una dirección URL base diferente, consulte ASP.NET Guía de la API de [SignalR Hubs - Servidor - La dirección URL de /signalr](hubs-api-guide-server.md#signalrurl).

De forma predeterminada, la ubicación del concentrador es el servidor actual; Si se conecta a un servidor diferente, `start` especifique la dirección URL antes de llamar al método, como se muestra en el ejemplo siguiente:

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> Normalmente se registran controladores `start` de eventos antes de llamar al método para establecer la conexión. Si desea registrar algunos controladores de eventos después de establecer la conexión, puede hacerlo, pero debe registrar `start` al menos uno de los controladores de eventos antes de llamar al método. Una razón para esto es que puede haber muchos concentradores en una `OnConnected` aplicación, pero no desea desencadenar el evento en cada concentrador si solo va a usar para uno de ellos. Cuando se establece la conexión, la presencia de un método de cliente en `OnConnected` el proxy de un concentrador es lo que indica a SignalR que desencadene el evento. Si no registra ningún controlador de eventos `start` antes de llamar al método, podrá invocar métodos `OnConnected` en el concentrador, pero no se llamará al método del concentrador y no se invocará ningún método de cliente desde el servidor.

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$.connection.hub es el mismo objeto que $.hubConnection() crea

Como puede ver en los ejemplos, cuando se `$.connection.hub` utiliza el proxy generado, hace referencia al objeto de conexión. Este es el mismo objeto `$.hubConnection()` que se obtiene al llamar cuando no está utilizando el proxy generado. El código proxy generado crea la conexión automáticamente ejecutando la siguiente instrucción:

![Creación de una conexión en el archivo proxy generado](hubs-api-guide-javascript-client/_static/image3.png)

Cuando usa el proxy generado, puede hacer `$.connection.hub` cualquier cosa con lo que puede hacer con un objeto de conexión cuando no está utilizando el proxy generado.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Ejecución asincrónica del método start

El `start` método se ejecuta de forma asincrónica. Devuelve un [objeto jQuery Deferred](http://api.jquery.com/category/deferred-object/), lo que significa que `pipe`puede `done`agregar `fail`funciones de devolución de llamada llamando a métodos como , , y . Si tiene código que desea ejecutar después de establecer la conexión, como una llamada a un método de servidor, coloque ese código en una función de devolución de llamada o llámelo desde una función de devolución de llamada. El `.done` método de devolución de llamada se ejecuta después de `OnConnected` que se haya establecido la conexión y después de que cualquier código que tenga en el método de controlador de eventos en el servidor termine de ejecutarse.

Si coloca la instrucción "Now connected" del ejemplo anterior como `start` la siguiente línea `.done` de `console.log` código después de la llamada al método (no en una devolución de llamada), la línea se ejecutará antes de que se establezca la conexión, como se muestra en el ejemplo siguiente:

![Forma incorrecta de escribir código que se ejecuta después de establecer la conexión](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Cómo establecer una conexión entre dominios

Normalmente, si el explorador `http://contoso.com`carga una página desde , la `http://contoso.com/signalr`conexión de SignalR está en el mismo dominio, en . Si la `http://contoso.com` página de `http://fabrikam.com/signalr`realiza una conexión a , es decir, una conexión entre dominios. Por motivos de seguridad, las conexiones entre dominios están deshabilitadas de forma predeterminada.

En SignalR 1.x, las solicitudes entre dominios se controlaban mediante una única marca EnableCrossDomain. Esta marca controlaba las solicitudes JSONP y CORS. Para una mayor flexibilidad, toda la compatibilidad con CORS se ha quitado del componente de servidor de SignalR (los clientes JavaScript siguen utilizando CORS normalmente si se detecta que el explorador lo admite) y el nuevo middleware OWIN se ha puesto a disposición para admitir estos escenarios.

Si se requiere JSONP en el cliente (para admitir solicitudes entre dominios en exploradores `HubConfiguration` antiguos), `true`deberá habilitarse explícitamente estableciendo `EnableJSONP` en el objeto en , como se muestra a continuación. JSONP está deshabilitado de forma predeterminada, ya que es menos seguro que CORS.

**Agregar Microsoft.Owin.Cors al proyecto:** Para instalar esta biblioteca, ejecute el siguiente comando en la consola del Administrador de paquetes:

`Install-Package Microsoft.Owin.Cors`

Este comando agregará la versión 2.1.0 del paquete al proyecto.

### <a name="calling-usecors"></a>Llamando a UseCors

 En el siguiente fragmento de código se muestra cómo implementar conexiones entre dominios en SignalR 2.

**Implementación de solicitudes entre dominios en SignalR 2**

El código siguiente muestra cómo habilitar CORS o JSONP en un proyecto de SignalR 2. Este ejemplo `Map` de `RunSignalR` código `MapSignalR`usa y en lugar de , para que el middleware CORS se ejecute solo para `MapSignalR`las solicitudes de SignalR que requieren compatibilidad con CORS (en lugar de para todo el tráfico en la ruta especificada en .) Map también se puede usar para cualquier otro middleware que necesite ejecutarse para un prefijo de dirección URL específico, en lugar de para toda la aplicación.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - No establezca `jQuery.support.cors` en true en el código.
>
>     ![No establezca jQuery.support.cors en true](hubs-api-guide-javascript-client/_static/image7.png)
>
>     SignalR controla el uso de CORS. Establecer `jQuery.support.cors` en true deshabilita JSONP porque hace que SignalR suponga que el explorador admite CORS.
> - Cuando se conecta a una dirección URL de host local, Internet Explorer 10 no la considerará una conexión entre dominios, por lo que la aplicación funcionará localmente con IE 10 incluso si no ha habilitado las conexiones entre dominios en el servidor.
> - Para obtener información sobre el uso de conexiones entre dominios con Internet Explorer 9, consulte [este subproceso StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Para obtener información sobre el uso de conexiones entre dominios con Chrome, consulte [este subproceso StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - El código de ejemplo utiliza la dirección URL predeterminada "/signalr" para conectarse al servicio SignalR. Para obtener información sobre cómo especificar una dirección URL base diferente, consulte ASP.NET Guía de la API de [SignalR Hubs - Servidor - La dirección URL de /signalr](hubs-api-guide-server.md#signalrurl).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Cómo configurar la conexión

Antes de establecer una conexión, puede especificar parámetros de cadena de consulta o especificar el método de transporte.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Cómo especificar parámetros de cadena de consulta

Si desea enviar datos al servidor cuando el cliente se conecta, puede agregar parámetros de cadena de consulta al objeto de conexión. En los ejemplos siguientes se muestra cómo establecer un parámetro de cadena de consulta en el código de cliente.

**Establecer un valor de cadena de consulta antes de llamar al método start (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

**Establecer un valor de cadena de consulta antes de llamar al método start (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

En el ejemplo siguiente se muestra cómo leer un parámetro de cadena de consulta en el código del servidor.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Cómo especificar el método de transporte

Como parte del proceso de conexión, un cliente de SignalR normalmente negocia con el servidor para determinar el mejor transporte que es compatible con el servidor y el cliente. Si ya sabe qué transporte desea usar, puede omitir este proceso de negociación especificando el método de transporte al llamar al `start` método.

**Código de cliente que especifica el método de transporte (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

**Código de cliente que especifica el método de transporte (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

Como alternativa, puede especificar varios métodos de transporte en el orden en el que desea que SignalR los pruebe:

**Código de cliente que especifica un esquema de reserva de transporte personalizado (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Código de cliente que especifica un esquema de reserva de transporte personalizado (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Puede utilizar los siguientes valores para especificar el método de transporte:

- "webSockets"
- "ForeverFrame"
- "serverSentEvents"
- "LongPolling"

En los ejemplos siguientes se muestra cómo averiguar qué método de transporte está utilizando una conexión.

**Código de cliente que muestra el método de transporte utilizado por una conexión (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

**Código de cliente que muestra el método de transporte utilizado por una conexión (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

Para obtener información acerca de cómo comprobar el método de transporte en el código del servidor, consulte ASP.NET Guía de la API de [SignalR Hubs - Servidor - Cómo obtener información sobre el cliente desde la propiedad Context](hubs-api-guide-server.md#contextproperty). Para obtener más información acerca de los transportes y las reservaciones, vea [Introducción a SignalR - Transportes y reserva .](../getting-started/introduction-to-signalr.md#transports)

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Cómo obtener un proxy para una clase Hub

Cada objeto de conexión que cree encapsula información sobre una conexión a un servicio de SignalR que contiene una o varias clases de concentrador. Para comunicarse con una clase Hub, utilice un objeto proxy que cree usted mismo (si no está utilizando el proxy generado) o que se genera automáticamente.

En el cliente, el nombre de proxy es una versión con mayúsculas y minúsculas del nombre de clase Hub. SignalR realiza automáticamente este cambio para que el código JavaScript pueda cumplir las convenciones de JavaScript.

**Clase Hub en servidor**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

**Obtener una referencia al proxy de cliente generado para el concentrador**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

**Crear proxy de cliente para la clase Hub (sin proxy generado)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

Si decora sin un `HubName` atributo la clase Hub, utilice el nombre exacto sin cambiar de mayúsculas y minúsculas.

**Clase Hub en servidor con atributo HubName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

**Obtener una referencia al proxy de cliente generado para el concentrador**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

**Crear proxy de cliente para la clase Hub (sin proxy generado)**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Cómo definir métodos en el cliente a los que el servidor puede llamar

Para definir un método al que el servidor puede llamar desde un `client` concentrador, agregue un controlador de `on` eventos al proxy de concentrador mediante la propiedad del proxy generado o llame al método si no usa el proxy generado. Los parámetros pueden ser objetos complejos.

Agregue el controlador de `start` eventos antes de llamar al método para establecer la conexión. (Si desea agregar controladores de `start` eventos después de llamar al método, vea la nota en [Cómo establecer una conexión](#establishconnection) anteriormente en este documento y utilice la sintaxis que se muestra para definir un método sin usar el proxy generado.)

La coincidencia de nombres de método no distingue mayúsculas de minúsculas. Por `Clients.All.addContosoChatMessageToPage` ejemplo, en el `AddContosoChatMessageToPage` `addContosoChatMessageToPage`servidor `addcontosochatmessagetopage` se ejecutará , , o en el cliente.

**Definir método en el cliente (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

**Forma alternativa de definir el método en el cliente (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

**Definir método en el cliente (sin el proxy generado o al agregar después de llamar al método de inicio)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

**Código de servidor que llama al método cliente**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

Los ejemplos siguientes incluyen un objeto complejo como parámetro de método.

**Definir método en el cliente que toma un objeto complejo (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

**Definir método en el cliente que toma un objeto complejo (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

**Código de servidor que define el objeto complejo**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

**Código de servidor que llama al método cliente mediante un objeto complejo**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Cómo llamar a métodos de servidor desde el cliente

Para llamar a un método de `server` servidor desde el cliente, use la propiedad del proxy generado o el `invoke` método en el proxy de concentrador si no está utilizando el proxy generado. El valor devuelto o los parámetros pueden ser objetos complejos.

Pase una versión en mayúsculas y minúsculas del nombre del método en el concentrador. SignalR realiza automáticamente este cambio para que el código JavaScript pueda cumplir las convenciones de JavaScript.

En los ejemplos siguientes se muestra cómo llamar a un método de servidor que no tiene un valor devuelto y cómo llamar a un método de servidor que tiene un valor devuelto.

**Método Server sin atributo HubMethodName**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

**Código de servidor que define el objeto complejo pasado en un parámetro**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

**Código de cliente que invoca el método de servidor (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

**Código de cliente que invoca el método de servidor (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

Si ha decorado el `HubMethodName` método Hub con un atributo, use ese nombre sin cambiar de mayúsculas y minúsculas.

**Método Server** con un atributo HubMethodName

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

**Código de cliente que invoca el método de servidor (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

**Código de cliente que invoca el método de servidor (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

Los ejemplos anteriores muestran cómo llamar a un método de servidor que no tiene ningún valor devuelto. En los ejemplos siguientes se muestra cómo llamar a un método de servidor que tiene un valor devuelto.

**Código de servidor para un método que tiene un valor devuelto**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

**La clase Stock utilizada para el** valor devuelto

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

**Código de cliente que invoca el método de servidor (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

**Código de cliente que invoca el método de servidor (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Cómo controlar los eventos de duración de la conexión

SignalR proporciona los siguientes eventos de duración de conexión que puede controlar:

- `starting`: Se genera antes de que se envíen datos a través de la conexión.
- `received`: Se genera cuando se reciben datos en la conexión. Proporciona los datos recibidos.
- `connectionSlow`: Se genera cuando el cliente detecta una conexión lenta o que se cae con frecuencia.
- `reconnecting`: Se genera cuando el transporte subyacente comienza a volver a conectarse.
- `reconnected`: Se genera cuando el transporte subyacente se ha vuelto a conectar.
- `stateChanged`: se genera cuando cambia el estado de conexión. Proporciona el estado antiguo y el nuevo estado (Conexión, Conectado, Reconexión o Desconectado).
- `disconnected`: Se genera cuando la conexión se ha desconectado.

Por ejemplo, si desea mostrar mensajes de advertencia cuando hay problemas de `connectionSlow` conexión que pueden causar retrasos notables, controle el evento.

**Controlar el evento connectionSlow (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

**Controlar el evento connectionSlow (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

Para obtener más información, consulte Descripción y control de eventos de duración de [conexión en SignalR](handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Cómo manejar los errores

El cliente de JavaScript `error` de SignalR proporciona un evento para el que puede agregar un controlador. También puede usar el método fail para agregar un controlador para los errores que resultan de una invocación de método de servidor.

Si no habilita explícitamente los mensajes de error detallados en el servidor, el objeto de excepción que SignalR devuelve después de un error contiene información mínima sobre el error. Por ejemplo, si `newContosoChatMessage` se produce un error en una`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`llamada, el mensaje de error del objeto de error contiene " " Enviar mensajes de error detallados a los clientes en producción no se recomienda por motivos de seguridad, pero si desea habilitar mensajes de error detallados para solucionar problemas, utilice el código siguiente en el servidor.

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

En el ejemplo siguiente se muestra cómo agregar un controlador para el evento de error.

**Agregar un controlador de errores (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

**Agregar un controlador de errores (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

En el ejemplo siguiente se muestra cómo controlar un error de una invocación de método.

**Controlar un error de una invocación de método (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

**Controlar un error de una invocación de método (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

Si se produce un `error` error en una invocación de `error` método, también `.fail` se genera el evento, por lo que el código en el controlador de método y en la devolución de llamada del método se ejecutaría.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Cómo habilitar el registro del lado cliente

Para habilitar el registro del lado `logging` cliente en una conexión, `start` establezca la propiedad en el objeto de conexión antes de llamar al método para establecer la conexión.

**Habilitar el registro (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

**Habilitar el registro (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Para ver los registros, abra las herramientas de desarrollador de su navegador y vaya a la pestaña Consola. Para ver un tutorial que muestra instrucciones paso a paso y capturas de pantalla que muestran cómo hacerlo, consulte Difusión del [servidor con ASP.NET Signalr - Habilitar registro](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).
