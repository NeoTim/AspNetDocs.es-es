---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Guía de la API de ASP.NET SignalR Hubs - Servidor (C-) Microsoft Docs
author: bradygaster
description: Este documento proporciona una introducción a la programación del lado del servidor de la API de ASP.NET SignalR Hubs para SignalR versión 2, con ejemplos de código que demuestran...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c681b104b15bfc4a04587c7abf685dcf20def2ca
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675806"
---
# <a name="aspnet-signalr-hubs-api-guide---server-c"></a>Guía de la API de ASP.NET SignalR Hubs - Servidor (C-)

por [Patrick Fletcher](https://github.com/pfletcher), Tom [Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este documento proporciona una introducción a la programación del lado del servidor de la API de concentradores de ASP.NET SignalR para SignalR versión 2, con ejemplos de código que demuestran opciones comunes.
> 
> La API de SignalR Hubs le permite realizar llamadas a procedimientos remotos (RPC) desde un servidor a clientes conectados y desde clientes al servidor. En el código de servidor, se definen métodos a los que pueden llamar los clientes y se llama a métodos que se ejecutan en el cliente. En el código de cliente, se definen métodos a los que se puede llamar desde el servidor y se llama a métodos que se ejecutan en el servidor. SignalR se encarga de todas las tuberías de cliente a servidor por usted.
> 
> SignalR también ofrece una API de nivel inferior denominada Conexiones persistentes. Para obtener una introducción a SignalR, concentradores y conexiones persistentes, vea [Introducción a SignalR 2](../getting-started/introduction-to-signalr.md).
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software utilizadas en este tema
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versión 2
>   
> 
> 
> ## <a name="topic-versions"></a>Versiones temáticas
> 
> Para obtener información acerca de las versiones anteriores de SignalR, vea [Versiones anteriores](../older-versions/index.md)de SignalR .
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Por favor, deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el [ASP.NET foro](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) de SignalR o [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Información general

Este documento contiene las siguientes secciones:

- [Cómo registrar el middleware de SignalR](#route)

    - [La URL /signalr](#signalrurl)
    - [Configuración de las opciones de SignalR](#options)
- [Cómo crear y usar clases de Hub](#hubclass)

    - [Duración del objeto de concentrador](#transience)
    - [Camel-casing de nombres hub en clientes JavaScript](#hubnames)
    - [Múltiples hubs](#multiplehubs)
    - [Concentradores fuertemente tipados](#stronglytypedhubs)
- [Cómo definir métodos en la clase Hub a los que los clientes pueden llamar](#hubmethods)

    - [Camel-casing de nombres de métodos en clientes JavaScript](#methodnames)
    - [Cuándo ejecutar de forma asincrónica](#asyncmethods)
    - [Definición de sobrecargas](#overloads)
    - [Informes del progreso de las invocaciones de métodos de concentrador](#progress)
- [Cómo llamar a métodos de cliente desde la clase Hub](#callfromhub)

    - [Selección de qué clientes recibirán el RPC](#selectingclients)
    - [No hay validación en tiempo de compilación para nombres de método](#dynamicmethodnames)
    - [Coincidencia de nombre de método que no distingue mayúsculas de minúsculas](#caseinsensitive)
    - [Ejecución asincrónica](#asyncclient)
- [Cómo administrar la pertenencia a grupos desde la clase Hub](#groupsfromhub)

    - [Ejecución asincrónica de métodos Add y Remove](#asyncgroupmethods)
    - [Persistencia de la pertenencia al grupo](#grouppersistence)
    - [Grupos de un solo usuario](#singleusergroups)
- [Cómo controlar los eventos de duración de la conexión en la clase Hub](#connectionlifetime)

    - [Cuando OnConnected, OnDisconnected y OnReconnected se llaman](#onreconnected)
    - [Estado de llamada no poblado](#nocallerstate)
- [Cómo obtener información sobre el cliente desde la propiedad Context](#contextproperty)
- [Cómo pasar el estado entre los clientes y la clase Hub](#passstate)
- [Cómo controlar los errores en la clase Hub](#handleErrors)
- [Cómo llamar a métodos de cliente y administrar grupos desde fuera de la clase Hub](#callfromoutsidehub)

    - [Llamar a métodos de cliente](#callingclientsoutsidehub)
    - [Gestión de la pertenencia a grupos](#managinggroupsoutsidehub)
- [Cómo habilitar el seguimiento](#tracing)
- [Cómo personalizar la canalización de Hubs](#hubpipeline)

Para obtener documentación sobre cómo programar clientes, consulte los siguientes recursos:

- [SignalR Hubs API Guide - Cliente JavaScript](hubs-api-guide-javascript-client.md)
- [SignalR Hubs API Guide - .NET Client](hubs-api-guide-net-client.md)

Los componentes del servidor para SignalR 2 solo están disponibles en .NET 4.5. Los servidores que ejecutan .NET 4.0 deben usar SignalR v1.x.

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a>Cómo registrar el middleware de SignalR

Para definir la ruta que los clientes usarán para conectarse al concentrador, llame al `MapSignalR` método cuando se inicie la aplicación. `MapSignalR`es un [método](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) `OwinExtensions` de extensión para la clase. En el ejemplo siguiente se muestra cómo definir la ruta de SignalR Hubs mediante una clase de inicio OWIN.

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

Si va a agregar la funcionalidad de SignalR a una aplicación MVC ASP.NET, asegúrese de que la ruta de SignalR se agrega antes que las otras rutas. Para obtener más información, vea [Tutorial: Introducción a SignalR 2 y MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>La URL /signalr

De forma predeterminada, la dirección URL de ruta que los clientes utilizarán para conectarse a su concentrador es "/signalr". (No confunda esta URL con la DIRECCIÓN URL "/signalr/hubs", que es para el archivo JavaScript generado automáticamente. Para obtener más información sobre el proxy generado, consulte [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).)

Puede haber circunstancias extraordinarias que hagan que esta dirección URL base no se pueda usar para SignalR; por ejemplo, tiene una carpeta en el proyecto denominada *signalr* y no desea cambiar el nombre. En ese caso, puede cambiar la dirección URL base, como se muestra en los ejemplos siguientes (reemplazar "/signalr" en el código de ejemplo con la dirección URL deseada).

**Código de servidor que especifica la dirección URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

**Código de cliente JavaScript que especifica la dirección URL (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

**Código de cliente JavaScript que especifica la dirección URL (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

**Código de cliente .NET que especifica la dirección URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>Configuración de las opciones de SignalR

Las sobrecargas `MapSignalR` del método le permiten especificar una dirección URL personalizada, un solucionador de dependencias personalizado y las siguientes opciones:

- Habilite las llamadas entre dominios mediante CORS o JSONP desde clientes de explorador.

    Normalmente, si el explorador `http://contoso.com`carga una página desde , la `http://contoso.com/signalr`conexión de SignalR está en el mismo dominio, en . Si la `http://contoso.com` página de `http://fabrikam.com/signalr`realiza una conexión a , es decir, una conexión entre dominios. Por motivos de seguridad, las conexiones entre dominios están deshabilitadas de forma predeterminada. Para obtener más información, consulte [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](hubs-api-guide-javascript-client.md#crossdomain).
- Habilite los mensajes de error detallados.

    Cuando se producen errores, el comportamiento predeterminado de SignalR es enviar a los clientes un mensaje de notificación sin detalles sobre lo que sucedió. No se recomienda enviar información de error detallada a los clientes en producción, ya que los usuarios malintencionados pueden usar la información en ataques contra la aplicación. Para solucionar problemas, puede usar esta opción para habilitar temporalmente informes de errores más informativos.
- Deshabilite los archivos proxy JavaScript generados automáticamente.

    De forma predeterminada, se genera un archivo JavaScript con servidores proxy para las clases hub en respuesta a la dirección URL "/signalr/hubs". Si no desea utilizar los servidores proxy de JavaScript, o si desea generar este archivo manualmente y hacer referencia a un archivo físico en sus clientes, puede usar esta opción para deshabilitar la generación de proxy. Para obtener más información, consulte [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](hubs-api-guide-javascript-client.md#manualproxy).

En el ejemplo siguiente se muestra cómo especificar la dirección `MapSignalR` URL de conexión de SignalR y estas opciones en una llamada al método. Para especificar una dirección URL personalizada, reemplace "/signalr" en el ejemplo por la dirección URL que desea utilizar.

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Cómo crear y usar clases de Hub

Para crear un concentrador, cree una clase que derive de [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). En el ejemplo siguiente se muestra una clase Hub simple para una aplicación de chat.

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

En este ejemplo, un cliente `NewContosoChatMessage` conectado puede llamar al método y, cuando lo hace, los datos recibidos se transmiten a todos los clientes conectados.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Duración del objeto de concentrador

No se crea una instancia de la clase Hub ni se llama a sus métodos desde su propio código en el servidor; todo lo que se hace por usted por la canalización de SignalR Hubs. SignalR crea una nueva instancia de la clase Hub cada vez que necesita controlar una operación de concentrador, como cuando un cliente se conecta, se desconecta o realiza una llamada de método al servidor.

Dado que las instancias de la clase Hub son transitorias, no puede usarlas para mantener el estado de una llamada de método a la siguiente. Cada vez que el servidor recibe una llamada de método de un cliente, una nueva instancia de la clase Hub procesa el mensaje. Para mantener el estado a través de varias conexiones y llamadas a métodos, utilice algún otro `Hub`método, como una base de datos, o una variable estática en la clase Hub o una clase diferente que no se derive de . Si conserva los datos en la memoria, mediante un método como una variable estática en la clase Hub, los datos se perderán cuando el dominio de aplicación se recicle.

Si desea enviar mensajes a los clientes desde su propio código que se ejecuta fuera de la clase Hub, no puede hacerlo creando una instancia de clase Hub, pero puede hacerlo obteniendo una referencia al objeto de contexto SignalR para la clase Hub. Para obtener más información, vea [Cómo llamar a métodos](#callfromoutsidehub) de cliente y administrar grupos desde fuera de la clase Hub más adelante en este tema.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>Camel-casing de nombres hub en clientes JavaScript

De forma predeterminada, los clientes de JavaScript hacen referencia a Hubs mediante una versión con mayúsculas y minúsculas del nombre de clase. SignalR realiza automáticamente este cambio para que el código JavaScript pueda cumplir las convenciones de JavaScript. El ejemplo anterior se `contosoChatHub` denominaría en código JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

**Cliente JavaScript mediante proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

Si desea especificar un nombre diferente para que `HubName` los clientes lo usen, agregue el atributo. Cuando se `HubName` utiliza un atributo, no hay ningún cambio de nombre en el caso de camello en los clientes JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

**Cliente JavaScript mediante proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Múltiples hubs

Puede definir varias clases de concentrador en una aplicación. Al hacerlo, la conexión se comparte, pero los grupos son independientes:

- Todos los clientes usarán la misma dirección URL para establecer una conexión de SignalR con el servicio ("/signalr" o la dirección URL personalizada si especificó una), y esa conexión se usa para todos los concentradores definidos por el servicio.

    No hay ninguna diferencia de rendimiento para varios concentradores en comparación con la definición de toda la funcionalidad de concentrador en una sola clase.
- Todos los concentradores obtienen la misma información de solicitud HTTP.

    Puesto que todos los concentradores comparten la misma conexión, la única información de solicitud HTTP que obtiene el servidor es lo que viene en la solicitud HTTP original que establece la conexión SignalR. Si usa la solicitud de conexión para pasar información del cliente al servidor especificando una cadena de consulta, no puede proporcionar cadenas de consulta diferentes a diferentes concentradores. Todos los Hubs recibirán la misma información.
- El archivo de proxies de JavaScript generado contendrá proxies para todos los concentradores en un archivo.

    Para obtener información acerca de los servidores proxy de JavaScript, consulte [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).
- Los grupos se definen en Los concentradores.

    En SignalR puede definir grupos con nombre para difundir a subconjuntos de clientes conectados. Los grupos se mantienen por separado para cada concentrador. Por ejemplo, un grupo denominado "Administradores" incluiría un conjunto de clientes para la `ContosoChatHub` clase `StockTickerHub` y el mismo nombre de grupo haría referencia a un conjunto diferente de clientes para la clase.

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a>Concentradores fuertemente tipados

Para definir una interfaz para los métodos de concentrador a los que el `Hub<T>` cliente puede hacer referencia (y `Hub`habilitar Intellisense en los métodos de concentrador), derive el concentrador de (introducido en SignalR 2.1) en lugar de:

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>Cómo definir métodos en la clase Hub a los que los clientes pueden llamar

Para exponer un método en el concentrador al que desea que se pueda llamar desde el cliente, declare un método público, como se muestra en los ejemplos siguientes.

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

Puede especificar un tipo de valor devuelto y parámetros, incluidos los tipos complejos y matrices, como lo haría en cualquier método de C. Los datos que recibe en parámetros o vuelve al autor de la llamada se comunican entre el cliente y el servidor mediante JSON y SignalR controla automáticamente el enlace de objetos complejos y matrices de objetos.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Camel-casing de nombres de métodos en clientes JavaScript

De forma predeterminada, los clientes de JavaScript hacen referencia a los métodos Hub mediante una versión con mayúsculas y minúsculas del nombre del método. SignalR realiza automáticamente este cambio para que el código JavaScript pueda cumplir las convenciones de JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**Cliente JavaScript mediante proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

Si desea especificar un nombre diferente para que `HubMethodName` los clientes lo usen, agregue el atributo.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**Cliente JavaScript mediante proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Cuándo ejecutar de forma asincrónica

Si el método se ejecuta durante mucho tiempo o tiene que realizar un trabajo que implique esperar, como [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) una búsqueda de `void` base de datos o una llamada de servicio web, haga que el método Hub sea asincrónico devolviendo un objeto Task (en lugar de return) o [Task&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) (en lugar del `T` tipo de valor devuelto). Cuando devuelve `Task` un objeto del método, SignalR `Task` espera a que se complete y, a continuación, envía el resultado desencapsulado al cliente, por lo que no hay ninguna diferencia en cómo codificar la llamada al método en el cliente.

Hacer un método Hub asincrónico evita bloquear la conexión cuando usa el transporte WebSocket. Cuando un método Hub se ejecuta sincrónicamente y el transporte es WebSocket, las invocaciones posteriores de métodos en el concentrador desde el mismo cliente se bloquean hasta que se completa el método Hub.

En el ejemplo siguiente se muestra el mismo método codificado para ejecutarse de forma sincrónica o asincrónica, seguido de código de cliente JavaScript que funciona para llamar a cualquiera de las versiones.

**Sincrónica**

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

**Asincrónica**

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**Cliente JavaScript mediante proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

Para obtener más información acerca de cómo usar métodos asincrónicos en ASP.NET 4.5, vea Uso de [métodos asincrónicos en ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>Definición de sobrecargas

Si desea definir sobrecargas para un método, el número de parámetros de cada sobrecarga debe ser diferente. Si diferencia una sobrecarga simplemente especificando diferentes tipos de parámetros, la clase Hub se compilará, pero el servicio SignalR producirá una excepción en tiempo de ejecución cuando los clientes intenten llamar a una de las sobrecargas.

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a>Informes del progreso de las invocaciones de métodos de concentrador

SignalR 2.1 agrega compatibilidad con el patrón de [informes](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) de progreso introducido en .NET 4.5. Para implementar informes `IProgress<T>` de progreso, defina un parámetro para el método de concentrador al que el cliente pueda tener acceso:

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

Al escribir un método de servidor de ejecución prolongada, es importante usar un patrón de programación asincrónica como Async/ Await en lugar de bloquear el subproceso de concentrador.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>Cómo llamar a métodos de cliente desde la clase Hub

Para llamar a métodos de `Clients` cliente desde el servidor, use la propiedad en un método de la clase Hub. En el ejemplo siguiente `addNewMessageToPage` se muestra el código de servidor que llama a todos los clientes conectados y el código de cliente que define el método en un cliente JavaScript.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

Invocar un método de cliente es `Task`una operación asincrónica y devuelve un archivo . Use `await`:

* Para asegurarse de que el mensaje se envía sin errores. 
* Para habilitar la detección y el control de errores en un bloque try-catch.

**Cliente JavaScript mediante proxy generado**

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

No se puede obtener un valor devuelto de un método de cliente; sintaxis `int x = Clients.All.add(1,1)` como no funciona.

Puede especificar tipos y matrices complejos para los parámetros. En el ejemplo siguiente se pasa un tipo complejo al cliente en un parámetro de método.

**Código de servidor que llama a un método cliente mediante un objeto complejo**

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

**Código de servidor que define el objeto complejo**

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

**Cliente JavaScript mediante proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Selección de qué clientes recibirán el RPC

La propiedad Clients devuelve un objeto [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) que proporciona varias opciones para especificar qué clientes recibirán el RPC:

- Todos los clientes conectados.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- Sólo el cliente que llama.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- Todos los clientes excepto el cliente que realiza la llamada.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- Un cliente específico identificado por el identificador de conexión.

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    Este ejemplo `addContosoChatMessageToPage` llama al cliente que realiza `Clients.Caller`la llamada y tiene el mismo efecto que usar .
- Todos los clientes conectados excepto los clientes especificados, identificados por el identificador de conexión.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- Todos los clientes conectados en un grupo especificado.

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- Todos los clientes conectados en un grupo especificado excepto los clientes especificados, identificados por el identificador de conexión.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- Todos los clientes conectados en un grupo especificado excepto el cliente que realiza la llamada.

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- Un usuario específico, identificado por userId.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    De forma predeterminada, se `IPrincipal.Identity.Name`trata de , pero esto se puede cambiar [registrando una implementación de IUserIdProvider con el host global.](mapping-users-to-connections.md#IUserIdProvider)
- Todos los clientes y grupos de una lista de iDE de conexión.

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- Una lista de grupos.

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- Un usuario por nombre.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- Una lista de nombres de usuario (introducidos en SignalR 2.1).

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>No hay validación en tiempo de compilación para nombres de método

El nombre del método que especifique se interpreta como un objeto dinámico, lo que significa que no hay ninguna validación de IntelliSense o en tiempo de compilación para él. La expresión se evalúa en tiempo de ejecución. Cuando se ejecuta la llamada al método, SignalR envía el nombre del método y los valores de parámetro al cliente, y si el cliente tiene un método que coincide con el nombre, se llama a ese método y se le pasan los valores de parámetro. Si no se encuentra ningún método coincidente en el cliente, no se genera ningún error. Para obtener información sobre el formato de los datos que SignalR transmite al cliente en segundo plano cuando se llama a un método de cliente, vea [Introducción a SignalR](../getting-started/introduction-to-signalr.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>Coincidencia de nombre de método que no distingue mayúsculas de minúsculas

La coincidencia de nombres de método no distingue mayúsculas de minúsculas. Por `Clients.All.addContosoChatMessageToPage` ejemplo, en el `AddContosoChatMessageToPage` `addcontosochatmessagetopage`servidor `addContosoChatMessageToPage` se ejecutará , , o en el cliente.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Ejecución asincrónica

El método al que se llama se ejecuta de forma asincrónica. Cualquier código que viene después de una llamada de método a un cliente se ejecutará inmediatamente sin esperar a que SignalR termine de transmitir datos a los clientes a menos que especifique que las líneas de código subsiguientes deben esperar a que se complete el método. En el ejemplo de código siguiente se muestra cómo ejecutar dos métodos de cliente secuencialmente.

**Uso de Await (.NET 4.5)**

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

Si usa `await` esperar hasta que finalice un método de cliente antes de que se ejecute la siguiente línea de código, eso no significa que los clientes recibirán realmente el mensaje antes de que se ejecute la siguiente línea de código. "Completar" de una llamada al método de cliente solo significa que SignalR ha hecho todo lo necesario para enviar el mensaje. Si necesita verificar que los clientes recibieron el mensaje, debe programar ese mecanismo usted mismo. Por ejemplo, podría `MessageReceived` codificar un método en `addContosoChatMessageToPage` el concentrador y, `MessageReceived` en el método en el cliente, podría llamar después de realizar el trabajo que necesite hacer en el cliente. En `MessageReceived` el concentrador puede hacer cualquier trabajo depende de la recepción del cliente real y el procesamiento de la llamada al método original.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Cómo utilizar una variable de cadena como nombre del método

Si desea invocar un método de cliente mediante una variable `Clients.All` de `Clients.Others` `Clients.Caller`cadena como nombre `IClientProxy` de método, convertir (o , , etc.) a y, a continuación, llame a [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Cómo administrar la pertenencia a grupos desde la clase Hub

Los grupos de SignalR proporcionan un método para transmitir mensajes a subconjuntos especificados de clientes conectados. Un grupo puede tener cualquier número de clientes y un cliente puede ser miembro de cualquier número de grupos.

Para administrar la pertenencia a grupos, use `Groups` los métodos [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) y [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) proporcionados por la propiedad de la clase Hub. En el ejemplo `Groups.Add` `Groups.Remove` siguiente se muestran los métodos y utilizados en los métodos De concentrador a los que llama el código de cliente, seguido del código de cliente JavaScript que los llama.

**Server**

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

**Cliente JavaScript mediante proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

No es posible crear grupos explícitamente. En efecto, un grupo se crea automáticamente la primera `Groups.Add`vez que se especifica su nombre en una llamada a , y se elimina cuando se quita la última conexión de la pertenencia a él.

No hay ninguna API para obtener una lista de pertenencia a grupos o una lista de grupos. SignalR envía mensajes a clientes y grupos basados en un [modelo pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe)y el servidor no mantiene listas de grupos o pertenencias a grupos. Esto ayuda a maximizar la escalabilidad, porque cada vez que se agrega un nodo a una granja de servidores web, cualquier estado que SignalR mantiene tiene que propagarse al nuevo nodo.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Ejecución asincrónica de métodos Add y Remove

Los `Groups.Add` `Groups.Remove` métodos y se ejecutan de forma asincrónica. Si desea agregar un cliente a un grupo y enviar inmediatamente un mensaje al cliente `Groups.Add` mediante el grupo, debe asegurarse de que el método finaliza primero. En el ejemplo de código siguiente se muestra cómo hacerlo.

**Agregar un cliente a un grupo y, a continuación, enviar mensajes a ese cliente**

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Persistencia de la pertenencia al grupo

SignalR realiza un seguimiento de las conexiones, no de los usuarios, por lo que `Groups.Add` si desea que un usuario esté en el mismo grupo cada vez que el usuario establece una conexión, debe llamar cada vez que el usuario establece una nueva conexión.

Después de una pérdida temporal de conectividad, a veces SignalR puede restaurar la conexión automáticamente. En ese caso, SignalR está restaurando la misma conexión, no estableciendo una nueva conexión, por lo que la pertenencia al grupo del cliente se restaura automáticamente. Esto es posible incluso cuando el salto temporal es el resultado de un reinicio o error del servidor, porque el estado de conexión para cada cliente, incluidas las pertenencias a grupos, se realiza de ida y vuelta al cliente. Si un servidor deja de ser y es reemplazado por un nuevo servidor antes de que se agota el tiempo de espera de la conexión, un cliente puede volver a conectarse automáticamente al nuevo servidor y volver a inscribirse en grupos de los que es miembro.

Cuando una conexión no se puede restaurar automáticamente después de una pérdida de conectividad, o cuando se agota el tiempo de espera de la conexión, o cuando el cliente se desconecta (por ejemplo, cuando un explorador navega a una página nueva), se pierden las pertenencias a grupos. La próxima vez que el usuario se conecte será una nueva conexión. Para mantener las pertenencias a grupos cuando el mismo usuario establece una nueva conexión, la aplicación tiene que realizar un seguimiento de las asociaciones entre usuarios y grupos y restaurar las pertenencias a grupos cada vez que un usuario establece una nueva conexión.

Para obtener más información acerca de las conexiones y las reconexiones, vea Cómo controlar los eventos de duración de [la conexión en la clase Hub](#connectionlifetime) más adelante en este tema.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Grupos de un solo usuario

Las aplicaciones que utilizan SignalR normalmente tienen que realizar un seguimiento de las asociaciones entre los usuarios y las conexiones para saber qué usuario ha enviado un mensaje y qué usuario(s) deben recibir un mensaje. Los grupos se utilizan en uno de los dos patrones comúnmente utilizados para hacerlo.

- Grupos de un solo usuario.

    Puede especificar el nombre de usuario como nombre de grupo y agregar el identificador de conexión actual al grupo cada vez que el usuario se conecte o vuelva a conectarse. Para enviar mensajes al usuario que envía al grupo. Una desventaja de este método es que el grupo no le proporciona una manera de averiguar si el usuario está en línea o sin conexión.
- Realice un seguimiento de las asociaciones entre los nombres de usuario y los ID de conexión.

    Puede almacenar una asociación entre cada nombre de usuario y uno o varios ID de conexión en un diccionario o base de datos, y actualizar los datos almacenados cada vez que el usuario se conecta o se desconecta. Para enviar mensajes al usuario, especifique los identificadores de conexión. Una desventaja de este método es que se necesita más memoria.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Cómo controlar los eventos de duración de la conexión en la clase Hub

Las razones típicas para controlar los eventos de duración de la conexión son realizar un seguimiento de si un usuario está conectado o no y realizar un seguimiento de la asociación entre los nombres de usuario y los ID de conexión. Para ejecutar su propio código cuando los `OnConnected` `OnDisconnected`clientes `OnReconnected` se conectan o desconectan, reemplace los métodos , y virtuales de la clase Hub, como se muestra en el ejemplo siguiente.

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>Cuando OnConnected, OnDisconnected y OnReconnected se llaman

Cada vez que un explorador navega a una nueva página, se debe establecer `OnDisconnected` una nueva `OnConnected` conexión, lo que significa que SignalR ejecutará el método seguido por el método. SignalR siempre crea un nuevo identificador de conexión cuando se establece una nueva conexión.

Se `OnReconnected` llama al método cuando se ha producido una interrupción temporal en la conectividad de la que SignalR puede recuperarse automáticamente, como cuando un cable se desconecta temporalmente y se vuelve a conectar antes de que se agota el tiempo de espera de la conexión. Se `OnDisconnected` llama al método cuando se desconecta el cliente y SignalR no se puede volver a conectar automáticamente, como cuando un explorador navega a una nueva página. Por lo tanto, una posible secuencia `OnConnected` `OnReconnected`de `OnDisconnected`eventos para un cliente determinado es , , ; o `OnConnected` `OnDisconnected`, . No verá la secuencia `OnConnected` `OnDisconnected`, `OnReconnected` , para una conexión determinada.

No `OnDisconnected` se llama al método en algunos escenarios, como cuando un servidor deja de ser o el dominio de aplicación se recicla. Cuando otro servidor entra en línea o el dominio de la aplicación completa `OnReconnected` su reciclaje, algunos clientes pueden volver a conectarse e desencadenar el evento.

Para obtener más información, consulte Descripción y control de eventos de duración de [conexión en SignalR](handling-connection-lifetime-events.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>Estado de llamada no poblado

Los métodos de controlador de eventos de duración de la conexión `state` se llaman desde el servidor, `Caller` lo que significa que cualquier estado que coloque en el objeto en el cliente no se rellenará en la propiedad del servidor. Para obtener `state` información sobre `Caller` el objeto y la propiedad, vea Cómo pasar el [estado entre los clientes y la clase Hub](#passstate) más adelante en este tema.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Cómo obtener información sobre el cliente desde la propiedad Context

Para obtener información sobre el `Context` cliente, use la propiedad de la clase Hub. La `Context` propiedad devuelve un objeto [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) que proporciona acceso a la siguiente información:

- El identificador de conexión del cliente que realiza la llamada.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    El identificador de conexión es un GUID asignado por SignalR (no se puede especificar el valor en su propio código). Hay un identificador de conexión para cada conexión y todos los concentradores usan el mismo identificador de conexión si tiene varios concentradores en la aplicación.
- Datos de encabezado HTTP.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    También puede obtener encabezados `Context.Headers`HTTP desde . La razón de varias referencias a `Context.Headers` la misma `Context.Request` cosa es que `Context.Headers` se creó primero, la propiedad se agregó más adelante y se retuvo por compatibilidad con versiones anteriores.
- Datos de cadena de consulta.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    También puede obtener datos `Context.QueryString`de cadena de consulta de .

    La cadena de consulta que se obtiene en esta propiedad es la que se usó con la solicitud HTTP que estableció la conexión De SignalR. Puede agregar parámetros de cadena de consulta en el cliente configurando la conexión, que es una manera conveniente de pasar datos sobre el cliente desde el cliente al servidor. En el ejemplo siguiente se muestra una manera de agregar una cadena de consulta en un cliente JavaScript cuando se usa el proxy generado.

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    Para obtener más información acerca de cómo establecer parámetros de cadena de consulta, consulte las guías de API para los clientes [de JavaScript](hubs-api-guide-javascript-client.md) y [.NET.](hubs-api-guide-net-client.md)

    Puede encontrar el método de transporte utilizado para la conexión en los datos de cadena de consulta, junto con algunos otros valores utilizados internamente por SignalR:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    El valor `transportMethod` de será "webSockets", "serverSentEvents", "foreverFrame" o "longPolling". Tenga en cuenta que si `OnConnected` comprueba este valor en el método de controlador de eventos, en algunos escenarios puede obtener inicialmente un valor de transporte que no es el método de transporte negociado final para la conexión. En ese caso, el método producirá una excepción y se llamará de nuevo más adelante cuando se establezca el método de transporte final.
- Galletas.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    También puede obtener `Context.RequestCookies`cookies de .
- información del usuario.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- El objeto HttpContext para la solicitud:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    Utilice este método `HttpContext.Current` en lugar `HttpContext` de obtener el objeto para la conexión De SignalR.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Cómo pasar el estado entre los clientes y la clase Hub

El proxy de `state` cliente proporciona un objeto en el que puede almacenar los datos que desea que se transmitan al servidor con cada llamada al método. En el servidor puede tener `Clients.Caller` acceso a estos datos en la propiedad en los métodos de concentrador a los que llaman los clientes. La `Clients.Caller` propiedad no se rellena para los `OnConnected` `OnDisconnected`métodos `OnReconnected`de controlador de eventos de duración de conexión , , y .

La creación o `state` actualización de `Clients.Caller` datos en el objeto y la propiedad funciona en ambas direcciones. Puede actualizar los valores en el servidor y se devuelven al cliente.

En el ejemplo siguiente se muestra el código de cliente de JavaScript que almacena el estado para la transmisión al servidor con cada llamada al método.

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

En el ejemplo siguiente se muestra el código equivalente en un cliente .NET.

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

En la clase Hub, puede tener `Clients.Caller` acceso a estos datos en la propiedad. En el ejemplo siguiente se muestra código que recupera el estado al que se hace referencia en el ejemplo anterior.

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> Este mecanismo para conservar el estado no está pensado para grandes `state` cantidades de datos, ya que todo lo que se coloca en la propiedad o `Clients.Caller` se realiza de ida y vuelta con cada invocación de método. Es útil para elementos más pequeños, como nombres de usuario o contadores.

En VB.NET o en un concentrador fuertemente tipado, no `Clients.Caller`se puede tener acceso al objeto de estado del llamador a través de ; en su `Clients.CallerState` lugar, el uso (introducido en SignalR 2.1):

**Uso de CallerState en C #**

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

**Uso de CallerState en Visual Basic**

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Cómo controlar los errores en la clase Hub

Para controlar los errores que se producen en los métodos de la clase Hub, primero asegúrese de "observar" cualquier excepción de las operaciones asincrónicas (como invocar métodos de cliente) mediante `await`. A continuación, utilice uno o varios de los métodos siguientes:

- Ajuste el código del método en bloques try-catch y registre el objeto de excepción. Para fines de depuración, puede enviar la excepción al cliente, pero por motivos de seguridad no se recomienda enviar información detallada a los clientes en producción.
- Cree un módulo de canalización de Hubs que controle el método [OnIncomingError.](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) En el ejemplo siguiente se muestra un módulo de canalización que registra errores, seguido de código en Startup.cs que inserta el módulo en la canalización de concentradores.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- Utilice `HubException` la clase (introducida en SignalR 2). Este error se puede producir desde cualquier invocación de concentrador. El `HubError` constructor toma un mensaje de cadena y un objeto para almacenar datos de error adicionales. SignalR serializará automáticamente la excepción y la enviará al cliente, donde se usará para rechazar o producir un error en la invocación del método de concentrador.

    En los ejemplos de `HubException` código siguientes se muestra cómo iniciar una invocación de concentrador durante una invocación de concentrador y cómo controlar la excepción en clientes JavaScript y .NET.

    **Código de servidor que muestra la clase HubException**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    **Código de cliente JavaScript que muestra la respuesta a la generación de una excepción HubException en un concentrador**

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    **Código de cliente .NET que muestra la respuesta a la generación de una excepción HubException en un concentrador**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

Para obtener más información acerca de los módulos de canalización de concentrador, vea [cómo personalizar la canalización](#hubpipeline) de concentradores más adelante en este tema.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>Cómo habilitar el seguimiento

Para habilitar el seguimiento del lado del servidor, agregue un elemento system.diagnostics al archivo Web.config, como se muestra en este ejemplo:

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

Al ejecutar la aplicación en Visual Studio, puede ver los registros en **la** salida ventana.

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>Cómo llamar a métodos de cliente y administrar grupos desde fuera de la clase Hub

Para llamar a métodos de cliente desde una clase diferente de la clase Hub, obtenga una referencia al objeto de contexto SignalR para el concentrador y úselo para llamar a métodos en el cliente o administrar grupos.

La siguiente `StockTicker` clase de ejemplo obtiene el objeto de contexto, lo almacena en una instancia de la clase, almacena la `updateStockPrice` instancia de clase en una `StockTickerHub`propiedad estática y utiliza el contexto de la instancia de clase singleton para llamar al método en clientes que están conectados a un concentrador denominado .

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

Si necesita usar el contexto varias veces en un objeto de larga duración, obtenga la referencia una vez y guárdela en lugar de volver a obtenerla cada vez. Obtener el contexto una vez garantiza que SignalR envía mensajes a los clientes en la misma secuencia en la que los métodos de concentrador realizan invocaciones de método de cliente. Para obtener un tutorial que muestra cómo usar el contexto de SignalR para un concentrador, vea Difusión de [servidor con ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>Llamar a métodos de cliente

Puede especificar qué clientes recibirán el RPC, pero tiene menos opciones que cuando llama desde una clase Hub. La razón de esto es que el contexto no está asociado a una llamada determinada de un `Clients.Others`cliente, por lo que los métodos que requieren conocimiento del identificador de conexión actual, como , o `Clients.Caller`, o `Clients.OthersInGroup`, no están disponibles. Están disponibles las siguientes opciones:

- Todos los clientes conectados.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- Un cliente específico identificado por el identificador de conexión.

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- Todos los clientes conectados excepto los clientes especificados, identificados por el identificador de conexión.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- Todos los clientes conectados en un grupo especificado.

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- Todos los clientes conectados en un grupo especificado excepto los clientes especificados, identificados por el identificador de conexión.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

Si llama a la clase que no es de Concentrador desde métodos de la `Clients.Client`clase `Clients.AllExcept`Hub, puede pasar el identificador de conexión actual y usarlo con , , `Clients.Group` o para simular `Clients.Caller`, `Clients.Others`, o `Clients.OthersInGroup`. En el ejemplo `MoveShapeHub` siguiente, la clase pasa `Broadcaster` el identificador `Broadcaster` de `Clients.Others`conexión a la clase para que la clase pueda simular .

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>Gestión de la pertenencia a grupos

Para administrar grupos, tiene las mismas opciones que en una clase Hub.

- Agregar un cliente a un grupo

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- Quitar un cliente de un grupo

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Cómo personalizar la canalización de Hubs

SignalR le permite insertar su propio código en la canalización de concentrador. En el ejemplo siguiente se muestra un módulo de canalización de concentrador personalizado que registra cada llamada al método entrante recibida del cliente y la llamada al método saliente invocada en el cliente:

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

El código siguiente en el *archivo Startup.cs* registra el módulo para que se ejecute en la canalización de concentradores:

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

Hay muchos métodos diferentes que puede invalidar. Para obtener una lista completa, vea [Métodos HubPipelineModule](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).
