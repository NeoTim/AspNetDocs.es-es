---
uid: signalr/overview/guide-to-the-api/handling-connection-lifetime-events
title: Comprender y controlar los eventos de por vida de la conexión en SignalR Microsoft Docs
author: bradygaster
description: En este artículo se describe cómo usar los eventos expuestos por la API de Concentradores.
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: 03960de2-8d95-4444-9169-4426dcc64913
msc.legacyurl: /signalr/overview/guide-to-the-api/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 5bdf20549fccab5d644e35fdf4ce351540c8620d
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675830"
---
# <a name="understanding-and-handling-connection-lifetime-events-in-signalr"></a>Comprender y controlar eventos de duración de la conexión en SignalR

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este artículo se proporciona información general sobre los eventos de conexión, reconexión y desconexión de SignalR que puede controlar, así como los valores de tiempo de espera y keepalive que puede configurar.
>
> En el artículo se supone que ya tiene algún conocimiento de SignalR y eventos de duración de conexión. Para obtener una introducción a SignalR, consulte [Introducción a SignalR](../getting-started/introduction-to-signalr.md). Para obtener listas de eventos de duración de conexión, consulte los siguientes recursos:
>
> - [Cómo controlar los eventos de duración de la conexión en la clase Hub](hubs-api-guide-server.md#connectionlifetime)
> - [Cómo controlar los eventos de duración de la conexión en clientes JavaScript](hubs-api-guide-javascript-client.md#connectionlifetime)
> - [Cómo controlar eventos de duración de conexión en clientes .NET](hubs-api-guide-net-client.md#connectionlifetime)
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

Este artículo contiene las secciones siguientes:

- [Terminología y escenarios de duración de la conexión](#terminology)

    - [Conexiones de SignalR, conexiones de transporte y conexiones físicas](#signalrvstransport)
    - [Escenarios de desconexión del transporte](#transportdisconnect)
    - [Escenarios de desconexión del cliente](#clientdisconnect)
    - [Escenarios de desconexión del servidor](#serverdisconnect)
- [Tiempo de espera y ajustes de keepalive](#timeoutkeepalive)

    - [Connectiontimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [Cómo cambiar el tiempo de espera y la configuración de keepalive](#changetimeout)
- [Cómo notificar al usuario sobre desconexiones](#notifydisconnect)
- [Cómo volver a conectarse continuamente](#continuousreconnect)
- [Cómo desconectar un cliente en el código del servidor](#disconnectclientfromserver)
- [Detectar la razón de una desconexión](#detectingreasonfordisconnection)

Los vínculos a temas de Referencia de API se vinculan a la versión 4.5 de la API. Si usa .NET 4, consulte [la versión de .NET 4 de los temas de la API.](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Terminología y escenarios de duración de la conexión

El `OnReconnected` controlador de eventos en un `OnConnected` concentrador de `OnDisconnected` SignalR puede ejecutarse directamente después pero no después para un cliente determinado. La razón por la que puede tener una reconexión sin una desconexión es que hay varias maneras en que la palabra "conexión" se utiliza en SignalR.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>Conexiones de SignalR, conexiones de transporte y conexiones físicas

Este artículo diferenciará entre *conexiones de SignalR,* *conexiones*de transporte y *conexiones físicas:*

- **La conexión de SignalR** hace referencia a una relación lógica entre un cliente y una dirección URL de servidor, mantenida por la API de SignalR e identificada de forma exclusiva por un identificador de conexión. SignalR mantiene los datos sobre esta relación y se utiliza para establecer una conexión de transporte. La relación finaliza y SignalR elimina los datos `Stop` cuando el cliente llama al método o se alcanza un límite de tiempo de espera mientras SignalR intenta restablecer una conexión de transporte perdida.
- **La conexión** de transporte hace referencia a una relación lógica entre un cliente y un servidor, mantenida por una de las cuatro API de transporte: WebSockets, eventos enviados por el servidor, marco para siempre o sondeo largo. SignalR usa la API de transporte para crear una conexión de transporte y la API de transporte depende de la existencia de una conexión de red física para crear la conexión de transporte. La conexión de transporte finaliza cuando SignalR la termina o cuando la API de transporte detecta que la conexión física está interrumpida.
- **La conexión física** se refiere a los enlaces de red física -- cables, señales inalámbricas, enrutadores, etc.- que facilitan la comunicación entre un equipo cliente y un equipo servidor. La conexión física debe estar presente para establecer una conexión de transporte, y debe establecerse una conexión de transporte para establecer una conexión SignalR. Sin embargo, interrumpir la conexión física no siempre finaliza inmediatamente la conexión de transporte o la conexión de SignalR, como se explicará más adelante en este tema.

En el diagrama siguiente, la conexión de SignalR se representa mediante la API de concentradores y la capa PersistentConnection API SignalR, la conexión de transporte se representa mediante la capa Transports y la conexión física se representa mediante las líneas entre el servidor y los clientes.

![Diagrama de arquitectura SignalR](handling-connection-lifetime-events/_static/image1.png)

Cuando se `Start` llama al método en un cliente de SignalR, se proporciona el código de cliente de SignalR con toda la información que necesita para establecer una conexión física a un servidor. El código de cliente de SignalR utiliza esta información para realizar una solicitud HTTP y establecer una conexión física que utiliza uno de los cuatro métodos de transporte. Si se produce un error en la conexión de transporte o se produce un error en el servidor, la conexión de SignalR no desaparece inmediatamente porque el cliente todavía tiene la información que necesita para restablecer automáticamente una nueva conexión de transporte a la misma dirección URL de SignalR. En este escenario, no hay ninguna intervención de la aplicación de usuario implicada y cuando el código de cliente de SignalR establece una nueva conexión de transporte, no inicia una nueva conexión de SignalR. La continuidad de la conexión SignalR se refleja en el hecho de que `Start` el identificador de conexión, que se crea cuando se llama al método, no cambia.

El `OnReconnected` controlador de eventos en el concentrador se ejecuta cuando una conexión de transporte se restablece automáticamente después de haberse perdido. El `OnDisconnected` controlador de eventos se ejecuta al final de una conexión de SignalR. Una conexión de SignalR puede finalizar de cualquiera de las siguientes maneras:

- Si el cliente `Stop` llama al método, se envía un mensaje de detención al servidor y tanto el cliente como el servidor finalizan la conexión de SignalR inmediatamente.
- Después de que se pierda la conectividad entre el cliente y el servidor, el cliente intenta volver a conectarse y el servidor espera a que el cliente vuelva a conectarse. Si los intentos de volver a conectarse no tienen éxito y finaliza el período de tiempo de espera de desconexión, el cliente y el servidor finalizan la conexión de SignalR. El cliente deja de intentar volver a conectarse y el servidor elimina su representación de la conexión de SignalR.
- Si el cliente deja de ejecutarse `Stop` sin tener la oportunidad de llamar al método, el servidor espera a que el cliente se vuelva a conectar y, a continuación, finaliza la conexión de SignalR después del período de tiempo de espera de desconexión.
- Si el servidor deja de ejecutarse, el cliente intenta volver a conectarse (volver a crear la conexión de transporte) y, a continuación, finaliza la conexión de SignalR después del período de tiempo de espera de desconexión.

Cuando no hay problemas de conexión y la aplicación de `Stop` usuario finaliza la conexión de SignalR mediante una llamada al método, la conexión de SignalR y la conexión de transporte comienzan y terminan aproximadamente al mismo tiempo. En las secciones siguientes se describen con más detalle los demás escenarios.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Escenarios de desconexión del transporte

Las conexiones físicas pueden ser lentas o puede haber interrupciones en la conectividad. Dependiendo de factores como la duración de la interrupción, la conexión de transporte puede descartarse. SignalR entonces intenta restablecer la conexión de transporte. A veces, la API de conexión de transporte detecta la interrupción y elimina la conexión de transporte, y SignalR descubre inmediatamente que se pierde la conexión. En otros escenarios, ni la API de conexión de transporte ni SignalR se da cuenta inmediatamente de que se ha perdido la conectividad. Para todos los transportes excepto el sondeo largo, el cliente de SignalR utiliza una función llamada *keepalive* para comprobar la pérdida de conectividad que la API de transporte no puede detectar. Para obtener información acerca de las conexiones de sondeo largos, consulte Tiempo de espera y configuración de [keepalive](#timeoutkeepalive) más adelante en este tema.

Cuando una conexión está inactiva, periódicamente el servidor envía un paquete de keepalive al cliente. A partir de la fecha en que se escribe este artículo, la frecuencia predeterminada es cada 10 segundos. Al escuchar estos paquetes, los clientes pueden decir si hay un problema de conexión. Si un paquete del keepalive no se recibe cuando se espera, después de un corto tiempo el cliente asume que hay problemas de conexión tales como lentitud o interrupciones. Si el keepalive todavía no se recibe después de un tiempo más largo, el cliente asume que la conexión se ha caído, y comienza a intentar volver a conectar.

En el diagrama siguiente se muestran los eventos de cliente y servidor que se generan en un escenario típico cuando hay problemas con la conexión física que la API de transporte no reconoce inmediatamente. El diagrama se aplica a las siguientes circunstancias:

- El transporte es WebSockets, marco para siempre o eventos enviados por el servidor.
- Hay diferentes períodos de interrupción en la conexión de red física.
- La API de transporte no se da cuenta de las interrupciones, por lo que SignalR se basa en la funcionalidad de keepalive para detectarlas.

![Desconexiones de transporte](handling-connection-lifetime-events/_static/image2.png)

Si el cliente entra en modo de reconexión pero no puede establecer una conexión de transporte dentro del límite de tiempo de espera de desconexión, el servidor finaliza la conexión de SignalR. Cuando esto sucede, el servidor ejecuta `OnDisconnected` el método del concentrador y pone en cola un mensaje de desconexión para enviarlo al cliente en caso de que el cliente se administre conectarse más adelante. Si el cliente entonces vuelve a conectarse, `Stop` recibe el comando disconnect y llama al método. En este `OnReconnected` escenario, no se ejecuta cuando `OnDisconnected` el cliente se `Stop`vuelve a conectar y no se ejecuta cuando el cliente llama a . En el diagrama siguiente se muestra este escenario.

![Interrupciones de transporte - tiempo de espera del servidor](handling-connection-lifetime-events/_static/image3.png)

Los eventos de duración de la conexión de SignalR que se pueden generar en el cliente son los siguientes:

- `ConnectionSlow`evento del cliente.

    Se genera cuando una proporción preestablecida del período de tiempo de espera de keepalive ha pasado desde que se recibió el último mensaje o ping de keepalive. El período de advertencia predeterminado del tiempo de espera de keepalive es 2/3 del tiempo de espera del keepalive. El tiempo de espera de keepalive es de 20 segundos, por lo que la advertencia se produce en unos 13 segundos.

    Por abandono, el servidor envía los pings del keepalive cada 10 segundos, y el cliente comprueba para los pings del keepalive sobre cada 2 segundos (un tercio de la diferencia entre el valor del tiempo de espera del keepalive y el valor de advertencia del tiempo de espera del keepalive).

    Si la API de transporte tiene conocimiento de una desconexión, SignalR podría ser informado de la desconexión antes de que pase el período de advertencia de tiempo de espera de keepalive. En ese caso, el `ConnectionSlow` evento no se generaría y `Reconnecting` SignalR iría directamente al evento.
- `Reconnecting`evento del cliente.

    Se genera cuando (a) la API de transporte detecta que se ha perdido la conexión, o (b) el período de tiempo de espera de keepalive ha pasado desde que se recibió el último mensaje o ping de keepalive. El código de cliente de SignalR comienza a intentar volver a conectarse. Puede controlar este evento si desea que la aplicación realice alguna acción cuando se pierde una conexión de transporte. El período de tiempo de espera de keepalive predeterminado es actualmente de 20 segundos.

    Si el código de cliente intenta llamar a un método Hub mientras SignalR está en modo de reconexión, SignalR intentará enviar el comando. La mayoría de las veces, tales intentos fracasarán, pero en algunas circunstancias podrían tener éxito. Para los eventos enviados por el servidor, la trama para siempre y los transportes de sondeo largo, SignalR utiliza dos canales de comunicación, uno que el cliente utiliza para enviar mensajes y otro que utiliza para recibir mensajes. El canal utilizado para recibir es el abierto permanentemente, y ese es el que se cierra cuando se interrumpe la conexión física. El canal utilizado para enviar permanece disponible, por lo que si se restaura la conectividad física, una llamada de método de cliente a servidor podría tener éxito antes de que se restablezca el canal de recepción. El valor devuelto no se recibiría hasta que SignalR vuelva a abrir el canal utilizado para la recepción.
- `Reconnected`evento del cliente.

    Se genera cuando se restablece la conexión de transporte. Se `OnReconnected` ejecuta el controlador de eventos en el concentrador.
- `Closed`evento de`disconnected` cliente (evento en JavaScript).

    Se genera cuando expira el período de tiempo de espera de desconexión mientras el código de cliente de SignalR intenta volver a conectarse después de perder la conexión de transporte. El tiempo de espera de desconexión predeterminado es de 30 segundos. (Este evento también se produce cuando `Stop` finaliza la conexión porque se llama al método.)

Las interrupciones de la conexión de transporte que no son detectadas por la API de transporte y no retrasan la recepción de pings de keepalive desde el servidor durante más tiempo que el período de advertencia de tiempo de espera de keepalive podrían no provocar ningún evento de duración de la conexión.

Algunos entornos de red cierran deliberadamente las conexiones inactivas, y otra función de los paquetes de keepalive es ayudar a prevenir esto dejando que estas redes sepan que una conexión SignalR está en uso. En casos extremos, la frecuencia predeterminada de los ping de keepalive podría no ser suficiente para evitar las conexiones cerradas. En ese caso usted puede configurar los pings del keepalive para ser enviados más a menudo. Para obtener más información, consulte Tiempo de espera y configuración de [keepalive](#timeoutkeepalive) más adelante en este tema.

> [!NOTE]
>
> **Importante**: La secuencia de eventos descrita aquí no está garantizada. SignalR realiza todos los intentos de generar eventos de duración de la conexión de una manera predecible de acuerdo con este esquema, pero hay muchas variaciones de eventos de red y muchas maneras en que los marcos de comunicaciones subyacentes, como las API de transporte, los controlan. Por ejemplo, `Reconnected` es posible que el evento no se `OnConnected` genere cuando el cliente se vuelva a conectar o que el controlador del servidor se ejecute cuando el intento de establecer una conexión no se realice correctamente. En este tema se describen solo los efectos que normalmente se producirían en determinadas circunstancias típicas.

<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Escenarios de desconexión del cliente

En un cliente de explorador, el código de cliente de SignalR que mantiene una conexión de SignalR se ejecuta en el contexto de JavaScript de una página web. Es por eso que la conexión de SignalR tiene que finalizar cuando navega de una página a otra, y es por eso que tiene varias conexiones con varios iDs de conexión si se conecta desde varias ventanas o pestañas del explorador. Cuando el usuario cierra una ventana o pestaña del explorador, o navega a una nueva página o actualiza la página, la `Stop` conexión de SignalR finaliza inmediatamente porque el código de cliente de SignalR controla ese evento de explorador y llama al método. En estos escenarios, o en cualquier plataforma `Stop` cliente `OnDisconnected` cuando la aplicación llama al método, el `Closed` controlador de eventos `disconnected` se ejecuta inmediatamente en el servidor y el cliente genera el evento (el evento se denomina en JavaScript).

Si una aplicación cliente o el equipo que se está ejecutando en caso de bloqueo o se queda en reposo (por ejemplo, cuando el usuario cierra el equipo portátil), el servidor no se informa sobre lo que sucedió. Por lo que el servidor sabe, la pérdida del cliente pudo deberse a la interrupción de la conectividad y el cliente podría estar intentando volver a conectarse. Por lo tanto, en estos escenarios el servidor espera para `OnDisconnected` dar al cliente la oportunidad de volver a conectarse y no se ejecuta hasta que expire el período de tiempo de espera de desconexión (alrededor de 30 segundos de forma predeterminada). En el diagrama siguiente se muestra este escenario.

![Fallo del equipo cliente](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Escenarios de desconexión del servidor

Cuando un servidor se desconecta (se reinicia, se produce un error, el dominio de la aplicación se recicla, etc.), el resultado puede ser similar a una `ConnectionSlow` conexión perdida, o la API de transporte y SignalR podrían saber inmediatamente que el servidor se ha ido y SignalR podría comenzar a intentar volver a conectarse sin provocar el evento. Si el cliente entra en modo de reconexión, y si el servidor se recupera o se reinicia o se pone en línea un nuevo servidor antes de que expire el período de tiempo de espera de desconexión, el cliente volverá a conectarse al servidor restaurado o nuevo. En ese caso, la conexión de SignalR `Reconnected` continúa en el cliente y se genera el evento. En el primer `OnDisconnected` servidor, nunca se ejecuta, y en el nuevo servidor, `OnReconnected` se ejecuta aunque `OnConnected` nunca se ejecutó para ese cliente en ese servidor antes. (El efecto es el mismo si el cliente se vuelve a conectar al mismo servidor después de un reinicio o reciclaje de dominio de aplicación, porque cuando el servidor se reinicia no tiene memoria de actividad de conexión anterior.) En el diagrama siguiente se supone que la API de `ConnectionSlow` transporte tiene conocimiento de la conexión perdida inmediatamente, por lo que no se genera el evento.

![Error del servidor y reconexión](handling-connection-lifetime-events/_static/image5.png)

Si un servidor no está disponible dentro del período de tiempo de espera de desconexión, finaliza la conexión de SignalR. En este escenario, `Closed` `disconnected` el evento (en clientes JavaScript) se genera en el cliente, pero `OnDisconnected` nunca se llama en el servidor. En el diagrama siguiente se supone que la API de transporte no tiene conocimiento de la `ConnectionSlow` conexión perdida, por lo que se detecta mediante la funcionalidad de keepalive de SignalR y se genera el evento.

![Error y tiempo de espera del servidor](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Tiempo de espera y ajustes de keepalive

El `ConnectionTimeout`valor `DisconnectTimeout`predeterminado `KeepAlive` , , y los valores son adecuados para la mayoría de los escenarios, pero se pueden cambiar si el entorno tiene necesidades especiales. Por ejemplo, si el entorno de red cierra las conexiones que están inactivas durante 5 segundos, es posible que tenga que disminuir el valor de keepalive.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

Esta configuración representa la cantidad de tiempo para dejar una conexión de transporte abierta y esperar una respuesta antes de cerrarla y abrir una nueva conexión. El valor predeterminado es 110 segundos.

Esta configuración solo se aplica cuando se deshabilita la funcionalidad keepalive, que normalmente solo se aplica al transporte de sondeo largo. El diagrama siguiente ilustra el efecto de esta configuración en una conexión de transporte de sondeo largo.

![Larga conexión de transporte de sondeo](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

Esta configuración representa la cantidad de tiempo que se debe `Disconnected` esperar después de que se pierda una conexión de transporte antes de generar el evento. El valor predeterminado es 30 segundos. Cuando se `DisconnectTimeout` `KeepAlive` establece , se establece automáticamente `DisconnectTimeout` en 1/3 del valor.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

Esta configuración representa la cantidad de tiempo que hay que esperar antes de enviar un paquete de keepalive a través de una conexión inactiva. El valor predeterminado es 10 segundos. Este valor no debe ser superior `DisconnectTimeout` a 1/3 del valor.

Si desea establecer `DisconnectTimeout` ambos `KeepAlive`y `KeepAlive` `DisconnectTimeout`, establecer después de . De `KeepAlive` lo contrario, la `DisconnectTimeout` configuración `KeepAlive` se sobrescribirá cuando se establezca automáticamente en 1/3 del valor de tiempo de espera.

Si desea deshabilitar la funcionalidad keepalive, establezca en `KeepAlive` null. La funcionalidad Keepalive se deshabilita automáticamente para el transporte de sondeo largo.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Cómo cambiar el tiempo de espera y la configuración de keepalive

Para cambiar los valores predeterminados de `Application_Start` esta configuración, establézcalos en el archivo *Global.asax,* como se muestra en el ejemplo siguiente. Los valores que se muestran en el código de ejemplo son los mismos que los valores predeterminados.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Cómo notificar al usuario sobre desconexiones

En algunas aplicaciones es posible que desee mostrar un mensaje al usuario cuando hay problemas de conectividad. Tiene varias opciones para cómo y cuándo hacer esto. Los siguientes ejemplos de código son para un cliente JavaScript que usa el proxy generado.

- Controle `connectionSlow` el evento para mostrar un mensaje tan pronto como SignalR sea consciente de los problemas de conexión, antes de que entre en modo de reconexión.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Controlar `reconnecting` el evento para mostrar un mensaje cuando SignalR es consciente de una desconexión y está entrando en modo de reconexión.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Controle `disconnected` el evento para mostrar un mensaje cuando se ha agotado el tiempo de espera de un intento de volver a conectarse. En este escenario, la única manera de restablecer una conexión con el servidor `Start` de nuevo es reiniciar la conexión de SignalR llamando al método, que creará un nuevo identificador de conexión. En el ejemplo de código siguiente se usa una marca para asegurarse de que emite la notificación `Stop` solo después de un tiempo de espera de reconexión, no después de un final normal a la conexión de SignalR causada por una llamada al método.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Cómo volver a conectarse continuamente

En algunas aplicaciones es posible que desee restablecer automáticamente una conexión después de que se ha perdido y se ha agotado el tiempo de espera del intento de volver a conectarse. Para ello, puede llamar `Start` al `Closed` método desde`disconnected` el controlador de eventos (controlador de eventos en clientes JavaScript). Es posible que desee esperar `Start` un período de tiempo antes de llamar para evitar hacer esto con demasiada frecuencia cuando el servidor o la conexión física no están disponibles. El ejemplo de código siguiente es para un cliente JavaScript que usa el proxy generado.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Un problema potencial a tener en cuenta en los clientes móviles es que los intentos de reconexión continua cuando el servidor o la conexión física no está disponible podrían causar un agotamiento innecesario de la batería.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Cómo desconectar un cliente en el código del servidor

SignalR versión 2 no tiene una API de servidor integrada para desconectar clientes. Hay [planes para agregar esta funcionalidad en el futuro.](https://github.com/SignalR/SignalR/issues/2101) En la versión actual de SignalR, la forma más sencilla de desconectar un cliente del servidor es implementar un método de desconexión en el cliente y llamar a ese método desde el servidor. En el ejemplo de código siguiente se muestra un método de desconexión para un cliente JavaScript mediante el proxy generado.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Seguridad: ni este método para desconectar clientes ni la API integrada propuesta abordarán el escenario de los clientes hackeados `stopClient` que ejecutan código malintencionado, ya que los clientes podrían volver a conectarse o el código hackeado podría eliminar el método o cambiar lo que hace. El lugar adecuado para implementar la protección de denegación de servicio (DOS) con estado no está en el marco de trabajo o la capa de servidor, sino en la infraestructura de front-end.

<a id="detectingreasonfordisconnection"></a>
## <a name="detecting-the-reason-for-a-disconnection"></a>Detectar la razón de una desconexión

SignalR 2.1 agrega una sobrecarga `OnDisconnect` al evento de servidor que indica si el cliente se desconectó deliberadamente en lugar de agotar el tiempo de espera. El `StopCalled` parámetro es true si el cliente cerró explícitamente la conexión. En JavaScript, si un error de servidor llevó al cliente a `$.connection.hub.lastError`desconectarse, la información de error se pasará al cliente como .

**Código de servidor `stopCalled` de C-: parámetro**

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample7.cs?highlight=1,3)]

**Código de cliente `lastError` JavaScript: acceso en caso de `disconnect` evento.**

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample8.js?highlight=2-3)]
