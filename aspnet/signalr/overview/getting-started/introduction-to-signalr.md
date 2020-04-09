---
uid: signalr/overview/getting-started/introduction-to-signalr
title: Introducción a SignalR ( SignalR) Microsoft Docs
author: bradygaster
description: En este artículo se describe qué es SignalR y algunas de las soluciones que se diseñó para crear.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8dbc31a5c8d59fa55dc5b513c1a51d24d18a685f
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675944"
---
# <a name="introduction-to-signalr"></a>Introducción a SignalR

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este artículo se describe qué es SignalR y algunas de las soluciones que se diseñó para crear. 
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Por favor, deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el [ASP.NET foro](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) de SignalR o [StackOverflow.com](https://stackoverflow.com/questions/tagged/signalr).

## <a name="what-is-signalr"></a>¿Qué es SignalR?

ASP.NET SignalR es una biblioteca para desarrolladores ASP.NET que simplifica el proceso de agregar funcionalidad web en tiempo real a las aplicaciones. La funcionalidad web en tiempo real es la capacidad de hacer que el código del servidor envíe contenido a los clientes conectados al instante a medida que esté disponible, en lugar de hacer que el servidor espere a que un cliente solicite nuevos datos.

SignalR se puede utilizar para agregar cualquier tipo de funcionalidad web "en tiempo real" a su aplicación ASP.NET. Mientras que el chat se utiliza a menudo como un ejemplo, puede hacer mucho más. Cada vez que un usuario actualiza una página web para ver nuevos datos o la página implementa sondeo [largo](http://en.wikipedia.org/wiki/Push_technology#Long_polling) para recuperar nuevos datos, es un candidato para usar SignalR. Algunos ejemplos son los paneles y las aplicaciones de supervisión, las aplicaciones colaborativas (como la edición simultánea de documentos), las actualizaciones del progreso del trabajo y los formularios en tiempo real.

SignalR también permite tipos completamente nuevos de aplicaciones web que requieren actualizaciones de alta frecuencia desde el servidor, por ejemplo, juegos en tiempo real.

SignalR proporciona una API sencilla para crear llamadas a procedimientos remotos de servidor a cliente (RPC) que llaman a funciones JavaScript en exploradores cliente (y otras plataformas de cliente) desde código .NET del lado servidor. SignalR también incluye API para la administración de conexiones (por ejemplo, eventos de conexión y desconexión) y conexiones de agrupación.

![Invocar métodos con SignalR](introduction-to-signalr/_static/image1.png)

SignalR controla automáticamente la administración de conexiones y permite difundir mensajes a todos los clientes conectados de forma simultánea, como un salón de chat. También se pueden enviar mensajes a clientes concretos. La conexión entre el cliente y el servidor es persistente, a diferencia de una conexión HTTP clásica, que se vuelve a establecer para cada comunicación.

SignalR admite la funcionalidad de "inserción de servidor", en la que el código de servidor puede llamar al código de cliente en el explorador mediante llamadas a procedimiento remoto (RPC), en lugar del modelo de solicitud-respuesta común en la web actual.

Las aplicaciones de SignalR pueden escalar horizontalmente a miles de clientes mediante proveedores de escalado horizontal integrados y de terceros.

Los proveedores integrados incluyen:
* [Service Bus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3)
* [SQL Server](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
* [Redis](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Redis)

Los proveedores externos incluyen:
* [NCache](https://www.alachisoft.com/ncache/asp-net-core-signalr.html).

SignalR es de código abierto, accesible a través de [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>SignalR y WebSocket

SignalR utiliza el nuevo transporte WebSocket cuando está disponible y vuelve a los transportes más antiguos cuando sea necesario. Aunque sin duda podría escribir la aplicación mediante WebSocket directamente, usar SignalR significa que gran parte de la funcionalidad adicional que necesitaría implementar ya está hecha para usted. Lo más importante es que esto significa que puede codificar la aplicación para aprovechar WebSocket sin tener que preocuparse por crear una ruta de acceso de código independiente para los clientes más antiguos. SignalR también le protege de tener que preocuparse por las actualizaciones de WebSocket, ya que SignalR se actualiza para admitir cambios en el transporte subyacente, proporcionando a la aplicación una interfaz coherente entre las versiones de WebSocket.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Transportes y respaldos

SignalR es una abstracción sobre algunos de los transportes necesarios para realizar el trabajo en tiempo real entre el cliente y el servidor. Una conexión de SignalR se inicia como HTTP y, a continuación, se promueve a una conexión WebSocket si está disponible. WebSocket es el transporte ideal para SignalR, ya que hace el uso más eficiente de la memoria del servidor, tiene la latencia más baja y tiene las características más subyacentes (como la comunicación dúplex completa entre el cliente y el servidor), pero también tiene los requisitos más estrictos: WebSocket requiere que el servidor utilice Windows Server 2012 o Windows 8 y .NET Framework 4.5. Si no se cumplen estos requisitos, SignalR intentará utilizar otros transportes para realizar sus conexiones.

### <a name="html-5-transports"></a>Transportes HTML 5

Estos transportes dependen de la compatibilidad con [HTML 5](http://en.wikipedia.org/wiki/HTML5). Si el explorador del cliente no admite el estándar HTML 5, se utilizarán los transportes más antiguos.

- **WebSocket** (si tanto el servidor como el explorador indican que pueden admitir Websocket). WebSocket es el único transporte que establece una verdadera conexión bidireccional persistente entre el cliente y el servidor. Sin embargo, WebSocket también tiene los requisitos más estrictos; es totalmente compatible sólo con las últimas versiones de Microsoft Internet Explorer, Google Chrome y Mozilla Firefox, y sólo tiene una implementación parcial en otros navegadores como Opera y Safari.
- **Eventos enviados**por servidor , también conocido como EventSource (si el explorador admite eventos enviados por servidor, que son básicamente todos los exploradores excepto Internet Explorer.)

### <a name="comet-transports"></a>Transportes de cometas

Los siguientes transportes se basan en el modelo de aplicación web [Comet,](http://en.wikipedia.org/wiki/Comet_(programming)) en el que un explorador u otro cliente mantiene una solicitud HTTP de larga data, que el servidor puede utilizar para insertar datos en el cliente sin que el cliente lo solicite específicamente.

- **Forever Frame** (solo para Internet Explorer). Forever Frame crea un IFrame oculto que realiza una solicitud a un punto de conexión en el servidor que no se completa. A continuación, el servidor envía continuamente el script al cliente que se ejecuta inmediatamente, proporcionando una conexión unidireccional en tiempo real de servidor a cliente. La conexión de cliente a servidor utiliza una conexión independiente del servidor a la conexión de cliente, y al igual que una solicitud HTTP estándar, se crea una nueva conexión para cada fragmento de datos que se debe enviar.
- **Ajax sondeo largo**. El sondeo largo no crea una conexión persistente, sino que sondea el servidor con una solicitud que permanece abierta hasta que el servidor responde, momento en el que se cierra la conexión y se solicita una nueva conexión inmediatamente. Esto puede introducir cierta latencia mientras se restablece la conexión.

Para obtener más información sobre qué transportes se admiten en qué configuraciones, consulte [Plataformas admitidas](supported-platforms.md).

### <a name="transport-selection-process"></a>Proceso de selección de transporte

La lista siguiente muestra los pasos que SignalR utiliza para decidir qué transporte usar.

1. Si el explorador es Internet Explorer 8 o anterior, se utiliza sondeo largo.
2. Si se configura JSONP (es decir, el `jsonp` parámetro se establece `true` en cuando se inicia la conexión), se utiliza sondeo largo.
3. Si se realiza una conexión entre dominios (es decir, si el extremo de SignalR no está en el mismo dominio que la página de hospedaje), se usará WebSocket si se cumplen los criterios siguientes:

   - El cliente admite CORS (Compartir recursos entre orígenes). Para obtener más información sobre qué clientes admiten CORS, consulte [CORS en caniuse.com](http://www.caniuse.com/CORS).
   - El cliente admite WebSocket
   - El servidor es compatible con WebSocket

     Si no se cumple alguno de estos criterios, se utilizará el sondeo largo. Para obtener más información sobre las conexiones entre dominios, consulte [Cómo establecer una conexión entre dominios.](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain)
4. Si JSONP no está configurado y la conexión no es entre dominios, se utilizará WebSocket si tanto el cliente como el servidor lo admiten.
5. Si el cliente o el servidor no admiten WebSocket, se usan eventos enviados por servidor si está disponible.
6. Si los eventos enviados por el servidor no están disponibles, se intenta Forever Frame.
7. Si se produce un error en Forever Frame, se utiliza el sondeo largo.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Seguimiento de los transportes

Puede determinar qué transporte utiliza la aplicación habilitando el registro en el concentrador y abriendo la ventana de la consola en el explorador.

Para habilitar el registro de los eventos del concentrador en un explorador, agregue el siguiente comando a la aplicación cliente:

`$.connection.hub.logging = true;`

- En Internet Explorer, abra las herramientas de desarrollo presionando F12 y haga clic en la pestaña Consola.

    ![Consola en Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- En Chrome, abra la consola pulsando Ctrl+Mayús+J.

    ![Consola en Google Chrome](introduction-to-signalr/_static/image3.png)

Con la consola abierta y el registro habilitado, podrá ver qué transporte está utilizando SignalR.

![Consola en Internet Explorer que muestra el transporte WebSocket](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Especificar un transporte

La negociación de un transporte requiere una cierta cantidad de tiempo y recursos de cliente/servidor. Si se conocen las capacidades de cliente, se puede especificar un transporte cuando se inicia la conexión de cliente. El siguiente fragmento de código muestra cómo iniciar una conexión mediante el transporte de sondeo largo de Ajax, como se usaría si se supiera que el cliente no admite ningún otro protocolo:

`connection.start({ transport: 'longPolling' });`

Puede especificar un pedido de reserva si desea que un cliente pruebe transportes específicos en orden. En el siguiente fragmento de código se muestra cómo probar WebSocket y, en su defecto, ir directamente a sondeo largo.

`connection.start({ transport: ['webSockets','longPolling'] });`

Las constantes de cadena para especificar transportes se definen de la siguiente manera:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Conexiones y hubs

La API de SignalR contiene dos modelos para comunicarse entre clientes y servidores: Conexiones persistentes y concentradores.

Una conexión representa un punto de conexión simple para enviar mensajes de un solo destinatario, agrupados o de difusión. La API de conexión persistente (representada en código .NET por la clase PersistentConnection) proporciona al desarrollador acceso directo al protocolo de comunicación de bajo nivel que SignalR expone. El uso del modelo de comunicación Conexiones será familiar para los desarrolladores que han utilizado API basadas en conexiones como Windows Communication Foundation.

Un concentrador es una canalización de más alto nivel basada en la API de conexión que permite que el cliente y el servidor llamen a métodos entre sí directamente. SignalR controla la distribución a través de los límites de la máquina como por arte de magia, lo que permite a los clientes llamar a métodos en el servidor tan fácilmente como métodos locales y viceversa. El uso del modelo de comunicación de Hubs será familiar para los desarrolladores que han utilizado API de invocación remota como .NET Remoting. El uso de un concentrador también le permite pasar parámetros fuertemente tipados a métodos, lo que permite el enlace de modelos.

### <a name="architecture-diagram"></a>Diagrama de la arquitectura

En el diagrama siguiente se muestra la relación entre los concentradores, las conexiones persistentes y las tecnologías subyacentes utilizadas para los transportes.

![Diagrama de arquitectura de SignalR que muestra API, transportes y clientes](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Cómo funcionan los hubs

Cuando el código del lado servidor llama a un método en el cliente, se envía un paquete a través del transporte activo que contiene el nombre y los parámetros del método al que se va a llamar (cuando se envía un objeto como parámetro de método, se serializa mediante JSON). A continuación, el cliente hace coincidir el nombre del método con los métodos definidos en el código del lado cliente. Si hay una coincidencia, el método de cliente se ejecutará utilizando los datos de parámetro deserializados.

La llamada al método se puede supervisar mediante herramientas como [Fiddler.](http://fiddler2.com/) La siguiente imagen muestra una llamada de método enviada desde un servidor De SignalR a un cliente de explorador web en el panel Registros de Fiddler. La llamada al método se `MoveShapeHub`envía desde un concentrador `updateShape`al que se llama y se llama al método que se invoca .

![Vista del registro de Fiddler que muestra el tráfico de SignalR](introduction-to-signalr/_static/image6.png)

En este ejemplo, el nombre `H` del concentrador se identifica con el parámetro; el nombre del método `M` se identifica con el parámetro y los `A` datos que se envían al método se identifican con el parámetro. La aplicación que generó este mensaje se crea en el tutorial de alta frecuencia en [tiempo real.](tutorial-high-frequency-realtime-with-signalr.md)

### <a name="choosing-a-communication-model"></a>Elegir un modelo de comunicación

La mayoría de las aplicaciones deben usar la API de Hubs. La API de Connections se puede utilizar en las siguientes circunstancias:

- Es necesario especificar el formato del mensaje real enviado.
- El desarrollador prefiere trabajar con un modelo de mensajería y distribución en lugar de un modelo de invocación remota.
- Una aplicación existente que usa un modelo de mensajería se está porteando para usar SignalR.
