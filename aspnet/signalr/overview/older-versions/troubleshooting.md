---
uid: signalr/overview/older-versions/troubleshooting
title: Solución de problemas de SignalR (SignalR 1.x) Microsoft Docs
author: bradygaster
description: En este artículo se describen problemas comunes con el desarrollo de aplicaciones de SignalR.
ms.author: bradyg
ms.date: 06/05/2013
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: e65ce086d28cff2a36c609f47a05af632081be63
ms.sourcegitcommit: 022f79dbc1350e0c6ffaa1e7e7c6e850cdabf9af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81543098"
---
# <a name="signalr-troubleshooting-signalr-1x"></a>Solución de problemas de SignalR (SignalR 1.x)

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este documento describe los problemas comunes de Troubleshooting con el SignalR.

Este documento contiene las siguientes secciones.

- [Se produce un error silencioso al llamar a métodos entre el cliente y el servidor](#connection)
- [Otros problemas de conexión](#other)
- [Errores de compilación y del lado del servidor](#server)
- [Problemas de Visual Studio](#vs)
- [Problemas con los servicios de Internet Information Server](#iis)
- [Problemas de Azure](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>Se produce un error silencioso al llamar a métodos entre el cliente y el servidor

En esta sección se describen las posibles causas de que una llamada de método entre el cliente y el servidor falle sin un mensaje de error significativo. En una aplicación de SignalR, el servidor no tiene información sobre los métodos que implementa el cliente; cuando el servidor invoca un método de cliente, el nombre del método y los datos de parámetro se envían al cliente y el método se ejecuta solo si existe en el formato especificado por el servidor. Si no se encuentra ningún método coincidente en el cliente, no ocurre nada y no se genera ningún mensaje de error en el servidor.

Para investigar más a fondo los métodos de cliente que no se llaman, puede activar el registro antes de llamar al método start en el concentrador para ver qué llamadas proceden del servidor. Para habilitar el registro en una aplicación JavaScript, consulte Cómo habilitar el registro del [lado cliente (versión de cliente JavaScript).](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging) Para habilitar el registro en una aplicación cliente .NET, consulte Cómo habilitar el registro del [lado cliente (versión de cliente .NET).](../guide-to-the-api/hubs-api-guide-net-client.md#logging)

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Método mal escrito, firma incorrecta del método o nombre de concentrador incorrecto

Si el nombre o la firma de un método llamado no coincide exactamente con un método adecuado en el cliente, se producirá un error en la llamada. Compruebe que el nombre del método al que llama el servidor coincide con el nombre del método en el cliente. Además, SignalR crea el proxy de concentrador mediante métodos con mayúsculas `SendMessage` y minúsculas, `sendMessage` como es apropiado en JavaScript, por lo que se llamaría a un método llamado en el servidor en el proxy de cliente. Si usa `HubName` el atributo en el código del lado servidor, compruebe que el nombre utilizado coincide con el nombre utilizado para crear el concentrador en el cliente. Si no utiliza `HubName` el atributo, compruebe que el nombre del concentrador en un cliente JavaScript está en mayúsculas y minúsculas, como chatHub en lugar de ChatHub.

### <a name="duplicate-method-name-on-client"></a>Nombre de método duplicado en el cliente

Compruebe que no tiene un método duplicado en el cliente que difiere solo por mayúsculas y minúsculas. Si la aplicación cliente `sendMessage`tiene un método llamado , compruebe `SendMessage` que no hay también un método al que se llama.

### <a name="missing-json-parser-on-the-client"></a>Perdiendo el analizador JSON en el cliente

SignalR requiere que un analizador JSON esté presente para serializar las llamadas entre el servidor y el cliente. Si el cliente no tiene un analizador JSON integrado (como Internet Explorer 7), deberá incluir uno en la aplicación. Puede descargar el analizador JSON [aquí](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Mezcla de la sintaxis de Hub y PersistentConnection

SignalR utiliza dos modelos de comunicación: Hubs y PersistentConnections. La sintaxis para llamar a estos dos modelos de comunicación es diferente en el código de cliente. Si ha agregado un concentrador en el código del servidor, compruebe que todo el código de cliente usa la sintaxis de concentrador adecuada.

**Código de cliente JavaScript que crea un PersistentConnection en un cliente JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Código de cliente JavaScript que crea un proxy de concentrador en un cliente Javascript**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**Código de servidor de C- que asigna una ruta a un PersistentConnection**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**Código de servidor de C- que asigna una ruta a un concentrador o a varios concentradores si tiene varias aplicaciones**

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a>La conexión se inició antes de agregar las suscripciones

Si la conexión del concentrador se inicia antes de que se agreguen métodos a los que se puede llamar desde el servidor al proxy, no se recibirán mensajes. El siguiente código JavaScript no iniciará el concentrador correctamente:

**Código de cliente JavaScript incorrecto que no permitirá recibir mensajes de Hubs**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

En su lugar, agregue las suscripciones de método antes de llamar a Start:

**Código de cliente JavaScript que agrega correctamente suscripciones a un concentrador**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Falta el nombre del método en el proxy del concentrador

Compruebe que el método definido en el servidor está suscrito en el cliente. Aunque el servidor define el método, todavía debe agregarse al proxy de cliente. Los métodos se pueden agregar al proxy de cliente de `client` las siguientes maneras (tenga en cuenta que el método se agrega al miembro del concentrador, no al concentrador directamente):

**Código de cliente JavaScript que agrega métodos a un proxy de concentrador**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Métodos hub o hub no declarados como públicos

Para ser visible en el cliente, la implementación del concentrador y los métodos deben declararse como `public`.

### <a name="accessing-hub-from-a-different-application"></a>Acceso al hub desde una aplicación diferente

Solo se puede tener acceso a los concentradores de SignalR a través de aplicaciones que implementan clientes de SignalR. SignalR no puede interoperar con otras bibliotecas de comunicación (como SOAP o servicios web WCF.) Si no hay ningún cliente de SignalR disponible para la plataforma de destino, no puede acceder directamente al punto de conexión del servidor.

### <a name="manually-serializing-data"></a>Serialización manual de datos

SignalR usará automáticamente JSON para serializar los parámetros del método: no hay necesidad de hacerlo usted mismo.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Método de concentrador remoto no ejecutado en el cliente en la función OnDisconnected

Este comportamiento es así por diseño. Cuando `OnDisconnected` se llama, el concentrador `Disconnected` ya ha entrado en el estado, que no permite llamar a otros métodos de concentrador.

**Código de servidor de C- que ejecuta correctamente el código en el evento OnDisconnected**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a>Límite de conexión alcanzado

Cuando se usa la versión completa de IIS en un sistema operativo cliente como Windows 7, se impone un límite de 10 conexiones. Cuando utilice un sistema operativo cliente, use IIS Express en su lugar para evitar este límite.

### <a name="cross-domain-connection-not-set-up-properly"></a>La conexión entre dominios no se ha configurado correctamente

Si una conexión entre dominios (una conexión para la que la dirección URL de SignalR no está en el mismo dominio que la página de hospedaje) no está configurada correctamente, la conexión puede fallar sin un mensaje de error. Para obtener información sobre cómo habilitar la comunicación entre dominios, consulte [Cómo establecer una conexión entre dominios.](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>Conexión mediante NTLM (Active Directory) que no funciona en el cliente .NET

Una conexión en una aplicación cliente .NET que usa seguridad de dominio puede producir un error si la conexión no está configurada correctamente. Para utilizar SignalR en un entorno de dominio, establezca la propiedad de conexión necesaria de la siguiente manera:

**Código de cliente de C- que implementa las credenciales de conexión**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a>Otros problemas de conexión

En esta sección se describen las causas y las soluciones para síntomas específicos o mensajes de error que se producen durante una conexión.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>Error "Se debe llamar al inicio antes de que se puedan enviar los datos"

Este error se ve normalmente si el código hace referencia a objetos SignalR antes de que se inicie la conexión. La conexión para los controladores y similares que llamará a los métodos definidos en el servidor debe agregarse una vez completada la conexión. Tenga en cuenta `Start` que la llamada a es asincrónica, por lo que el código después de la llamada se puede ejecutar antes de que se complete. La mejor manera de agregar controladores después de que una conexión se inicia por completo es colocarlos en una función de devolución de llamada que se pasa como parámetro al método start:

**Código de cliente JavaScript que agrega correctamente controladores de eventos que hacen referencia a objetos SignalR**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Este error también se verá si una conexión se detiene mientras se sigue haciendo referencia a objetos SignalR.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>Error "301 Moved Permanently" o "302 Moved Temporarily"

Este error se puede ver si el proyecto contiene una carpeta llamada SignalR, que interferirá con el proxy creado automáticamente. Para evitar este error, no `SignalR` utilice una carpeta a la que se llame en la aplicación ni desactive la generación automática de proxy. Consulte [el proxy generado y lo que hace por usted](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) para obtener más detalles.

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>Error "403 Prohibido" en .NET o cliente Silverlight

Este error puede producirse en entornos entre dominios en los que la comunicación entre dominios no está habilitada correctamente. Para obtener información sobre cómo habilitar la comunicación entre dominios, consulte [Cómo establecer una conexión entre dominios.](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) Para establecer una conexión entre dominios en un cliente de Silverlight, consulte [Conexiones entre dominios desde clientes de Silverlight.](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain)

### <a name="404-not-found-error"></a>Error "404 Not Found"

Hay varias causas para este problema. Compruebe todo lo siguiente:

- Referencia de **dirección proxy de concentrador no formateada correctamente:** Este error se ve comúnmente si la referencia a la dirección proxy del concentrador generada no tiene el formato correcto. Compruebe que la referencia a la dirección del concentrador se realiza correctamente. Consulte [Cómo hacer referencia al proxy generado dinámicamente](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) para obtener más información.
- **Agregar rutas a la aplicación antes de agregar la ruta del concentrador:** Si la aplicación utiliza otras rutas, compruebe que la `MapHubs`primera ruta agregada es la llamada a .

### <a name="500-internal-server-error"></a>"500 Error interno del servidor"

Este es un error muy genérico que podría tener una amplia variedad de causas. Los detalles del error deben aparecer en el registro de eventos del servidor o se pueden encontrar mediante la depuración del servidor. Se puede obtener información de error más detallada activando errores detallados en el servidor. Para obtener más información, consulte [Cómo controlar errores en la clase Hub](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>Error &lt;"TypeError:&gt; hubType is undefined"

Este error dará lugar `MapHubs` si la llamada a no se realiza correctamente. Consulte [Cómo registrar la ruta de SignalR y configurar](../guide-to-the-api/hubs-api-guide-server.md#route) las opciones de SignalR para obtener más información.

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>JsonSerializationException no fue controlado por el código de usuario

Compruebe que los parámetros que envía a los métodos no incluyen tipos no serializables (como identificadores de archivo o conexiones de base de datos). Si necesita usar miembros en un objeto del lado servidor que no desea que se envíe al cliente `JSONIgnore` (ya sea por seguridad o por motivos de serialización), utilice el atributo.

### <a name="protocol-error-unknown-transport-error"></a>Error "Error de protocolo: transporte desconocido"

Este error puede producirse si el cliente no admite los transportes que utiliza SignalR. Consulte [Transportes y reserva para](../getting-started/introduction-to-signalr.md#transports) obtener información sobre qué navegadores se pueden utilizar con SignalR.

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"Se ha deshabilitado la generación de proxy de JavaScript Hub."

Este error se `DisableJavaScriptProxies` producirá si se establece al incluir también una `signalr/hubs`referencia al proxy generado dinámicamente en . Para obtener más información sobre cómo crear el proxy manualmente, consulte [El proxy generado y lo que hace por usted.](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy)

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"El ID de conexión está en el formato incorrecto" o "La identidad del usuario no puede cambiar durante una conexión De SignalR activa"

Este error se puede ver si se está utilizando la autenticación y el cliente cierra la sesión antes de que se detenga la conexión. La solución es detener la conexión de SignalR antes de cerrar la sesión del cliente.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"Error no capturado: SignalR: jQuery no encontrado. Asegúrese de que jQuery hace referencia antes del error SignalR.js file"

El cliente JavaScript de SignalR requiere jQuery para ejecutarse. Compruebe que la referencia a jQuery es correcta, que la ruta de acceso utilizada es válida y que la referencia a jQuery está antes de la referencia a SignalR.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>Error "Uncaught TypeError:&lt;Cannot&gt;Can read property ' property ' of undefined"

Este error se debe a que no se hace referencia correctamente a jQuery o al proxy de concentradores. Compruebe que la referencia a jQuery y el proxy de concentradores es correcta, que la ruta de acceso utilizada es válida y que la referencia a jQuery está antes de la referencia al proxy de concentradores. La referencia predeterminada al proxy de concentradores debe tener el siguiente aspecto:

**Código HTML del lado cliente que hace referencia correctamente al proxy de Hubs**

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>Error "RuntimeBinderException no se controló mediante el código de usuario"

Este error puede producirse `Hub.On` cuando se utiliza la sobrecarga incorrecta de. Si el método tiene un valor devuelto, el tipo de valor devuelto debe especificarse como un parámetro de tipo genérico:

**Método definido en el cliente (sin proxy generado)**

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>El ID de conexión es incoherente o los saltos de conexión entre cargas de página

Este comportamiento es así por diseño. Dado que el objeto de concentrador se hospeda en el objeto de página, el concentrador se destruye cuando se actualiza la página. Una aplicación de varias páginas debe mantener la asociación entre los usuarios y los identificadores de conexión para que sean coherentes entre las cargas de página. Los identificadores de conexión se pueden almacenar `ConcurrentDictionary` en el servidor en un objeto o en una base de datos.

### <a name="value-cannot-be-null-error"></a>Error "Value cannot be null"

Actualmente no se admiten métodos del lado del servidor con parámetros opcionales; si se omite el parámetro opcional, se producirá un error en el método. Para obtener más información, consulte [Parámetros opcionales](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Firefox no puede establecer una conexión &lt;&gt;con el servidor en la dirección" error en Firebug

Este mensaje de error se puede ver en Firebug si se produce un error en la negociación del transporte WebSocket y se utiliza otro transporte en su lugar. Este comportamiento es así por diseño.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>Error "El certificado remoto no es válido según el procedimiento de validación" en la aplicación cliente .NET

Si el servidor requiere certificados de cliente personalizados, puede agregar un certificado x509 a la conexión antes de realizar la solicitud. Agregue el certificado a `Connection.AddClientCertificate`la conexión mediante .

### <a name="connection-drops-after-authentication-times-out"></a>La conexión cae después de que se agota el tiempo de espera de la autenticación

Este comportamiento es así por diseño. Las credenciales de autenticación no se pueden modificar mientras una conexión está activa; para actualizar las credenciales, la conexión debe detenerse y reiniciarse.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>OnConnected recibe una llamada dos veces cuando se usa jQuery Mobile

La función `initializePage` de jQuery Mobile obliga a que los scripts de cada página se vuelvan a ejecutar, creando así una segunda conexión. Las soluciones para este problema incluyen:

- Incluya la referencia a jQuery Mobile antes de su archivo JavaScript.
- Desactive `initializePage` la función estableciendo `$.mobile.autoInitializePage = false`.
- Espere a que la página termine de inicializarse antes de iniciar la conexión.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>Los mensajes se retrasan en las aplicaciones de Silverlight que usan eventos enviados por el servidor

Los mensajes se retrasan cuando se usan eventos enviados por el servidor en Silverlight. Para forzar el sondeo largo que se utilizará en su lugar, utilice lo siguiente al iniciar la conexión:

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>"Permiso denegado" usando el protocolo Forever Frame

Este es un problema conocido, descrito [aquí](https://github.com/SignalR/SignalR/issues/1963). Este síntoma se puede ver utilizando la última biblioteca JQuery; la solución consiste en degradar la aplicación a JQuery 1.8.2.

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Errores de compilación y del lado del servidor

 La siguiente sección contiene posibles soluciones a los errores de tiempo de ejecución del compilador y del lado del servidor. 

### <a name="reference-to-hub-instance-is-null"></a>La referencia a la instancia de Hub es null

Puesto que se crea una instancia de concentrador para cada conexión, no puede crear una instancia de un concentrador en el código usted mismo. Para llamar a métodos en un concentrador desde fuera del propio concentrador, vea [Cómo llamar a métodos](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) de cliente y administrar grupos desde fuera de la clase Hub para obtener una referencia al contexto del concentrador.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext.Current.Session es null

Este comportamiento es así por diseño. SignalR no admite el estado de sesión ASP.NET, ya que habilitar el estado de sesión interrumpiría la mensajería dúplex.

### <a name="no-suitable-method-to-override"></a>No hay un método adecuado para anular

Es posible que vea este error si está utilizando código de documentación o blogs anteriores. Compruebe que no hace referencia a nombres de métodos `OnConnectedAsync`que se han cambiado o en desuso (como ).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions.WebSocketServerUrl es null

Este comportamiento es así por diseño. Este miembro está en desuso y no se debe usar.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>"Una ruta llamada 'signalr.hubs' ya está en la colección de rutas"

Este error se verá `MapHubs` si la aplicación llama dos veces. Algunas aplicaciones `MapHubs` de ejemplo llaman directamente en el archivo de aplicación global; otros hacen la llamada en una clase de envoltura. Asegúrese de que la aplicación no hace ambas cosas.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Problemas de Visual Studio

En esta sección se describen los problemas encontrados en Visual Studio.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>El nodo Documentos de script no aparece en el Explorador de soluciones

Algunos de nuestros tutoriales le dirigen al nodo "Documentos de script" en el Explorador de soluciones durante la depuración. Este nodo es generado por el depurador de JavaScript y solo aparecerá durante la depuración de clientes del explorador en Internet Explorer; el nodo no aparecerá si se utilizan Chrome o Firefox. El depurador de JavaScript tampoco se ejecutará si se está ejecutando otro depurador de cliente, como el depurador de Silverlight.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>SignalR no funciona en Visual Studio 2008 o versiones anteriores

Este comportamiento es así por diseño. SignalR requiere .NET Framework 4 o posterior; Esto requiere que las aplicaciones de SignalR se desarrollen en Visual Studio 2010 o posterior.

<a id="iis"></a>

## <a name="iis-issues"></a>Problemas con IIS

Esta sección contiene problemas con Internet Information Services.

### <a name="web-site-crashes-after-maphubs-call"></a>El sitio web se bloquea después de la llamada de MapHubs

Este problema se ha corregido en la última versión de SignalR. Compruebe que está utilizando la versión más reciente de SignalR mediante la actualización de la instalación mediante NuGet.

<a id="azure"></a>

## <a name="azure-issues"></a>Problemas de Azure

Esta sección contiene problemas con Microsoft Azure.

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>Los mensajes no se reciben a través del backplane de Azure después de modificar los nombres de los temas

Los temas utilizados por el backplane de Azure no están diseñados para ser configurables por el usuario.
