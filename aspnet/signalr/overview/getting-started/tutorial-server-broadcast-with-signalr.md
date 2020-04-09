---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Tutorial: Transmisión del servidor con SignalR 2 Microsoft Docs'
author: tdykstra
description: En este tutorial se muestra cómo crear una aplicación web que usa ASP.NET SignalR 2 para proporcionar funcionalidad de difusión del servidor.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14924109fff8db3e537e6bc08b6dc868792ee660
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675728"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a>Tutorial: Transmisión del servidor con SignalR 2

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

En este tutorial se muestra cómo crear una aplicación web que usa ASP.NET SignalR 2 para proporcionar funcionalidad de difusión del servidor. La difusión del servidor significa que el servidor inicia las comunicaciones enviadas a los clientes.

La aplicación que creará en este tutorial simula un ticker de acciones, un escenario típico para la funcionalidad de difusión del servidor. Periódicamente, el servidor actualiza aleatoriamente los precios de las acciones y transmite las actualizaciones a todos los clientes conectados. En el explorador, los números **%** y símbolos del **cambio** y las columnas cambian dinámicamente en respuesta a las notificaciones del servidor. Si abre navegadores adicionales en la misma URL, todos mostrarán los mismos datos y los mismos cambios en los datos simultáneamente.

![Crear web](tutorial-server-broadcast-with-signalr/_static/image1.png)

En este tutorial, hizo lo siguiente:

> [!div class="checklist"]
> * Creación del proyecto
> * Configuración del código de servidor
> * Examine el código del servidor
> * Configurar el código de cliente
> * Examine el código de cliente
> * Prueba de la aplicación
> * Habilitación del registro

> [!IMPORTANT]
> Si no desea trabajar en los pasos de creación de la aplicación, puede instalar el paquete SignalR.Sample en un nuevo proyecto de aplicación web de ASP.NET vacía. Si instala el paquete NuGet sin realizar los pasos de este tutorial, debe seguir las instrucciones del archivo *readme.txt.* Para ejecutar el paquete, debe agregar una clase `ConfigureSignalR` de inicio OWIN que llame al método en el paquete instalado. Recibirá un error si no agrega la clase de inicio OWIN. Consulte la sección [de ejemplo Instalar StockTicker](#install-the-stockticker-sample) de este artículo.

## <a name="prerequisites"></a>Prerrequisitos

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con la carga de trabajo de **ASP.NET y desarrollo web**.

## <a name="create-the-project"></a>Creación del proyecto

En esta sección se muestra cómo usar Visual Studio 2017 para crear una aplicación web de ASP.NET vacía.

1. En Visual Studio, cree una aplicación web ASP.NET.

    ![Crear web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. En la ventana **Nueva aplicación web ASP.NET - SignalR.StockTicker,** deje **vacío** seleccionado y seleccione **Aceptar**.

## <a name="set-up-the-server-code"></a>Configuración del código de servidor

En esta sección, configurará el código que se ejecuta en el servidor.

### <a name="create-the-stock-class"></a>Crear la clase Stock

Para empezar, cree la clase de modelo *Stock* que utilizará para almacenar y transmitir información sobre un stock.

1. En **el Explorador**de soluciones , haga clic con el botón secundario en el proyecto y seleccione **Agregar** > **clase**.

1. Asigne un nombre a la clase *Stock* y agréguela al proyecto.

1. Reemplace el código del archivo *Stock.cs* por este código:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Las dos propiedades que establecerá al crear `Symbol` stocks son (por ejemplo, `Price`MSFT para Microsoft) y . Las otras propiedades dependen de `Price`cómo y cuándo se establece . La primera vez `Price`que se establece , `DayOpen`el valor se propaga a . Después de eso, cuando se `Price` `Change` establece `PercentChange` , la aplicación `Price` calcula `DayOpen`los valores de propiedad y en función de la diferencia entre y .

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a>Cree las clases StockTickerHub y StockTicker

Usará la API de SignalR Hub para controlar la interacción de servidor a cliente. Una `StockTickerHub` clase que deriva de `Hub` la SignalR clase controlará la recepción de conexiones y llamadas de método de los clientes. También debe actualizar los datos `Timer` de stock y ejecutar un objeto. El `Timer` objeto desencadenará periódicamente actualizaciones de precios independientemente de las conexiones de cliente. No se pueden colocar estas `Hub` funciones en una clase, porque los concentradores son transitorios. La aplicación `Hub` crea una instancia de clase para cada tarea en el concentrador, como conexiones y llamadas desde el cliente al servidor. Por lo tanto, el mecanismo que mantiene los datos de stock, actualiza los precios y difunde las actualizaciones de precios tiene que ejecutarse en una clase independiente. Nombrará la clase `StockTicker`.

![Radiodifusión desde StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

Solo desea que una `StockTicker` instancia de la clase se ejecute en el servidor, `StockTickerHub` por lo que `StockTicker` deberá configurar una referencia de cada instancia a la instancia singleton. La `StockTicker` clase tiene que transmitir a los clientes porque `StockTicker` tiene los `Hub` datos de stock y desencadena actualizaciones, pero no es una clase. La `StockTicker` clase tiene que obtener una referencia al objeto de contexto de conexión de concentrador de SignalR. A continuación, puede utilizar el objeto de contexto de conexión SignalR para difundir a los clientes.

#### <a name="create-stocktickerhubcs"></a>Crear StockTickerHub.cs

1. En **el Explorador**de soluciones , haga clic con el botón secundario en el proyecto y seleccione **Agregar** > nuevo**elemento**.

1. En **Agregar nuevo elemento - SignalR.StockTicker**, seleccione **Instalado** > **Visual C-** > **Web** > **SignalR** y, a continuación, seleccione **SignalR Hub Class (v2)**.

1. Asigne a la clase el nombre *StockTickerHub* y agréguela al proyecto.

    Este paso crea el archivo de clase *StockTickerHub.cs.* Simultáneamente, agrega un conjunto de archivos de script y referencias de ensamblado que admite SignalR al proyecto.

1. Reemplace el código del archivo *StockTickerHub.cs* por este código:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. Guarde el archivo.

La aplicación usa la clase [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) para definir métodos a los que los clientes pueden llamar en el servidor. Estás definiendo un método: `GetAllStocks()`. Cuando un cliente se conecta inicialmente al servidor, llamará a este método para obtener una lista de todas las acciones con sus precios actuales. El método se puede `IEnumerable<Stock>` ejecutar sincrónicamente y devolver porque devuelve datos de la memoria.

Si el método tuviera que obtener los datos haciendo algo que implicaría esperar, como `Task<IEnumerable<Stock>>` una búsqueda de base de datos o una llamada de servicio web, especificaría como valor devuelto para habilitar el procesamiento asincrónico. Para obtener más información, consulte [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

El `HubName` atributo especifica cómo la aplicación hará referencia al concentrador en código JavaScript en el cliente. El nombre predeterminado en el cliente si no utiliza este atributo, es una versión camelCase del nombre de clase, que en este caso sería `stockTickerHub`.

Como verá más adelante cuando `StockTicker` cree la clase, la aplicación crea una instancia `Instance` singleton de esa clase en su propiedad estática. Esa instancia singleton de `StockTicker` está en la memoria no importa cuántos clientes se conecten o desconecten. Esa instancia `GetAllStocks()` es lo que el método utiliza para devolver la información de stock actual.

#### <a name="create-stocktickercs"></a>Crear StockTicker.cs

1. En **el Explorador**de soluciones , haga clic con el botón secundario en el proyecto y seleccione **Agregar** > **clase**.

1. Asigne un nombre a la clase *StockTicker* y agréguela al proyecto.

1. Reemplace el código del archivo *StockTicker.cs* por este código:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

Dado que todos los subprocesos van a ejecutar la misma instancia de código StockTicker, la clase StockTicker tiene que ser segura para subprocesos.

### <a name="examine-the-server-code"></a>Examine el código del servidor

Si examina el código del servidor, le ayudará a comprender cómo funciona la aplicación.

#### <a name="storing-the-singleton-instance-in-a-static-field"></a>Almacenamiento de la instancia singleton en un campo estático

El código inicializa `_instance` el campo estático `Instance` que respalda la propiedad con una instancia de la clase. Dado que el constructor es privado, es la única instancia de la clase que la aplicación puede crear. La aplicación usa la `_instance` [inicialización diferida](/dotnet/framework/performance/lazy-initialization) para el campo. No es por razones de rendimiento. Es para asegurarse de que la creación de la instancia es segura para subprocesos.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

Cada vez que un cliente se conecta al servidor, una nueva instancia de la clase StockTickerHub `StockTicker.Instance` que se ejecuta en un `StockTickerHub` subproceso independiente obtiene la instancia singleton de StockTicker de la propiedad estática, como se vio anteriormente en la clase.

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Almacenamiento de datos de stock en un ConcurrentDictionary

El constructor inicializa `_stocks` la colección con `GetAllStocks` algunos datos de stock de ejemplo y devuelve las existencias. Como ha visto anteriormente, esta colección `StockTickerHub.GetAllStocks`de acciones es devuelta por , que es un método de servidor en la `Hub` clase que los clientes pueden llamar.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

La colección stocks se define como un tipo [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) para la seguridad de subprocesos. Como alternativa, puede usar un [objeto Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) y bloquear explícitamente el diccionario cuando realice cambios en él.

Para esta aplicación de ejemplo, está bien almacenar los datos de la aplicación en `StockTicker` la memoria y perder los datos cuando la aplicación elimina la instancia. En una aplicación real, trabajaría con un almacén de datos back-end como una base de datos.

#### <a name="periodically-updating-stock-prices"></a>Actualización periódica de los precios de las acciones

El constructor inicia `Timer` un objeto que llama periódicamente a métodos que actualizan los precios de las acciones de forma aleatoria.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 `Timer`llama `UpdateStockPrices`, que pasa null en el parámetro state. Antes de actualizar los precios, `_updateStockPricesLock` la aplicación toma un bloqueo en el objeto. El código comprueba si otro subproceso ya está `TryUpdateStockPrice` actualizando los precios y, a continuación, llama a cada acción de la lista. El `TryUpdateStockPrice` método decide si se debe cambiar el precio de la acción y cuánto cambiarlo. Si el precio de la `BroadcastStockPrice` acción cambia, la aplicación llama para transmitir el cambio de precio de la acción a todos los clientes conectados.

La `_updatingStockPrices` marca designada [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) para asegurarse de que es seguro para subprocesos.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

En una aplicación `TryUpdateStockPrice` real, el método llamaría a un servicio web para buscar el precio. En este código, la aplicación utiliza un generador de números aleatorios para realizar cambios al azar.

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Obtener el contexto SignalR para que la clase StockTicker pueda transmitir a los clientes

Dado que los cambios `StockTicker` de precio se originan aquí `updateStockPrice` en el objeto, es el objeto que necesita llamar a un método en todos los clientes conectados. En `Hub` una clase, tiene una API para `StockTicker` llamar a métodos de cliente, pero no deriva de la `Hub` clase y no tiene una referencia a ningún `Hub` objeto. Para transmitir a clientes conectados, la `StockTicker` clase tiene `StockTickerHub` que obtener la instancia de contexto de SignalR para la clase y usarla para llamar a métodos en los clientes.

El código obtiene una referencia al contexto de SignalR cuando crea la instancia de clase singleton, pasa `Clients` esa referencia al constructor y el constructor lo coloca en la propiedad.

Hay dos razones por las que desea obtener el contexto una sola vez: obtener el contexto es una tarea costosa y obtenerlo una vez se asegura de que la aplicación conserva el orden previsto de los mensajes enviados a los clientes.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

Obtener `Clients` la propiedad del contexto y `StockTickerClient` colocarla en la propiedad le permite escribir código `Hub` para llamar a métodos de cliente que tiene el mismo aspecto que en una clase. Por ejemplo, para transmitir a `Clients.All.updateStockPrice(stock)`todos los clientes puede escribir .

El `updateStockPrice` método al `BroadcastStockPrice` que llamas aún no existe. Lo agregará más adelante cuando escriba código que se ejecute en el cliente. Puede hacer `updateStockPrice` referencia `Clients.All` aquí porque es dinámico, lo que significa que la aplicación evaluará la expresión en tiempo de ejecución. Cuando se ejecuta esta llamada al método, SignalR enviará el nombre del método y `updateStockPrice`el valor del parámetro al cliente, y si el cliente tiene un método denominado , la aplicación llamará a ese método y le pasará el valor del parámetro.

`Clients.All`significa enviar a todos los clientes. SignalR le ofrece otras opciones para especificar a qué clientes o grupos de clientes enviar. Para obtener más información, vea [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Registre la ruta SignalR

El servidor necesita saber qué dirección URL interceptar y dirigir a SignalR. Para ello, agregue una clase de inicio OWIN:

1. En **el Explorador**de soluciones , haga clic con el botón secundario en el proyecto y seleccione **Agregar** > nuevo**elemento**.

1. En **Agregar nuevo elemento - SignalR.StockTicker** seleccione **Instalado** > **Visual C-** > **Web** y, a continuación, seleccione **OWIN Startup Class**.

1. Asigne un nombre a la clase *Inicio* y seleccione **Aceptar**.

1. Reemplace el código predeterminado del archivo *Startup.cs* por este código:

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Ya ha terminado de configurar el código del servidor. En la siguiente sección, configurará el cliente.

## <a name="set-up-the-client-code"></a>Configurar el código de cliente

En esta sección, configurará el código que se ejecuta en el cliente.

### <a name="create-the-html-page-and-javascript-file"></a>Cree la página HTML y el archivo JavaScript

La página HTML mostrará los datos y el archivo JavaScript organizará los datos.

#### <a name="create-stocktickerhtml"></a>Crear StockTicker.html

En primer lugar, agregará el cliente HTML.

1. En **el Explorador**de soluciones , haga clic con el botón secundario en el proyecto y seleccione **Agregar** > página**HTML**.

1. Asigne al archivo el nombre *StockTicker* y seleccione **Aceptar**.

1. Reemplace el código predeterminado en el archivo *StockTicker.html* por este código:

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    El código HTML crea una tabla con cinco columnas, una fila de encabezado y una fila de datos con una sola celda que abarca las cinco columnas. La fila de datos muestra "carga..." momentáneamente cuando se inicia la aplicación. El código JavaScript eliminará esa fila y agregará en su lugar filas con datos de stock recuperados del servidor.

    Las etiquetas de script especifican:

    * El archivo de script jQuery.

    * El archivo de script principal de SignalR.

    * El archivo de script de proxies de SignalR.

    * Un archivo de script StockTicker que creará más adelante.

    La aplicación genera dinámicamente el archivo de script de proxies de SignalR. Especifica la dirección URL "/signalr/hubs" y define métodos proxy para los `StockTickerHub.GetAllStocks`métodos de la clase Hub, en este caso, para . Si lo prefiere, puede generar este archivo JavaScript manualmente mediante [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/). No olvide deshabilitar la creación dinámica `MapHubs` de archivos en la llamada al método.

1. En **el Explorador**de soluciones , expanda **scripts**.

    Las bibliotecas de scripts para jQuery y SignalR son visibles en el proyecto.

    > [!IMPORTANT]
    > El administrador de paquetes instalará una versión posterior de los scripts de SignalR.

1. Actualice las referencias de script en el bloque de código para que correspondan a las versiones de los archivos de script del proyecto.

1. En **el Explorador**de soluciones , haga clic con el botón secundario en *StockTicker.html*y, a continuación, seleccione **Establecer como página de inicio**.

#### <a name="create-stocktickerjs"></a>Crear StockTicker.js

Ahora cree el archivo JavaScript.

1. En **el Explorador**de soluciones , haga clic con el botón secundario en el proyecto y seleccione **Agregar** > **archivo JavaScript**.

1. Asigne al archivo el nombre *StockTicker* y seleccione **Aceptar**.

1. Agregue este código al archivo *StockTicker.js:*

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a>Examine el código de cliente

Si examina el código de cliente, le ayudará a aprender cómo interactúa el código de cliente con el código de servidor para que la aplicación funcione.

#### <a name="starting-the-connection"></a>Inicio de la conexión

`$.connection`se refiere a los proxies de SignalR. El código obtiene una referencia al `StockTickerHub` proxy de la `ticker` clase y lo coloca en la variable. El nombre de proxy es el `HubName` nombre establecido por el atributo:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

Después de definir todas las variables y funciones, la última línea de código `start` del archivo inicializa la conexión de SignalR llamando a la función SignalR. La `start` función se ejecuta de forma asincrónica y devuelve un [objeto jQuery Deferred](http://api.jquery.com/category/deferred-object/). Puede llamar a la función done para especificar la función a la que se llamará cuando la aplicación finalice la acción asincrónica.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a>Conseguir todas las existencias

La `init` función `getAllStocks` llama a la función en el servidor y utiliza la información que el servidor devuelve para actualizar la tabla de valores. Tenga en cuenta que, de forma predeterminada, debe usar camelCasing en el cliente aunque el nombre del método esté en mayúsculas y minúsculas en el servidor. La regla camelCasing solo se aplica a los métodos, no a los objetos. Por ejemplo, hace `stock.Symbol` `stock.Price`referencia `stock.symbol` a `stock.price`y , no o .

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

En `init` el método, la aplicación crea HTML para una fila de `formatStock` tabla para `stock` cada objeto de `supplant` stock recibido del `rowTemplate` servidor llamando `stock` a las propiedades de formato del objeto y, a continuación, llamando para reemplazar los marcadores de posición de la variable con los valores de propiedad del objeto. A continuación, el HTML resultante se anexa a la tabla de valores.

> [!NOTE]
> Se `init` llama pasándolo como `callback` una función que `start` se ejecuta una vez finalizada la función asincrónica. Si llamó `init` como una instrucción `start`JavaScript independiente después de llamar , la función produciría un error porque se ejecutaría inmediatamente sin esperar a que la función de inicio termine de establecer la conexión. En ese caso, la `init` función `getAllStocks` intentaría llamar a la función antes de que la aplicación establezca una conexión de servidor.

#### <a name="getting-updated-stock-prices"></a>Obtener precios actualizados de las acciones

Cuando el servidor cambia el precio de `updateStockPrice` una acción, llama a los clientes conectados. La aplicación agrega la función a `stockTicker` la propiedad client del proxy para que esté disponible para las llamadas desde el servidor.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

La `updateStockPrice` función da formato a un objeto de stock recibido `init` del servidor en una fila de tabla de la misma manera que en la función. En lugar de anexar la fila a la tabla, busca la fila actual del stock en la tabla y reemplaza esa fila por la nueva.

## <a name="test-the-application"></a>Prueba de la aplicación

Puedeprobar la aplicación para asegurarse de que funciona. Verá que todas las ventanas del navegador muestran la tabla de acciones en vivo con los precios de las acciones fluctuando.

1. En la barra de herramientas, active **Depuración** de scripts y, a continuación, seleccione el botón de reproducción para ejecutar la aplicación en modo Depurar.

    ![Captura de pantalla del usuario activando el modo de depuración y seleccionando play.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    Se abrirá una ventana del navegador que muestra la **Tabla de acciones**en vivo . La tabla de valores muestra inicialmente la "carga..." línea, luego, después de un corto tiempo, la aplicación muestra los datos de stock iniciales y, a continuación, los precios de las acciones comienzan a cambiar.

1. Copie la dirección URL del explorador, abra otros dos exploradores y pegue las direcciones URL en las barras de direcciones.

    La visualización inicial del stock es la misma que la del primer navegador y los cambios se producen simultáneamente.

1. Cierre todos los navegadores, abra un nuevo navegador y vaya a la misma URL.

    El objeto singleton StockTicker continuó ejecutándose en el servidor. La **Tabla de Acciones En Vivo** muestra que las acciones han seguido cambiando. No verá la tabla inicial con cifras de cambio cero.

1. Cierre el explorador.

## <a name="enable-logging"></a>Habilitación del registro

SignalR tiene una función de registro integrada que puede habilitar en el cliente para ayudar en la solución de problemas. En esta sección, habilitará el registro y verá ejemplos que muestran cómo los registros le indican cuál de los siguientes métodos de transporte utiliza SignalR:

* [WebSockets](http://en.wikipedia.org/wiki/WebSocket), compatible con IIS 8 y los exploradores actuales.

* [Eventos enviados por](http://en.wikipedia.org/wiki/Server-sent_events)el servidor , compatibles con exploradores distintos de Internet Explorer.

* [Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), compatible con Internet Explorer.

* [Ajax sondeo largo](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), compatible con todos los navegadores.

Para cualquier conexión dada, SignalR elige el mejor método de transporte que tanto el servidor como el cliente admiten.

1. Abra *StockTicker.js*.

1. Agregue esta línea de código resaltada para habilitar el registro inmediatamente antes del código que inicializa la conexión al final del archivo:

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. Presione **F5** para ejecutar el proyecto.

1. Abra la ventana de herramientas para desarrolladores del explorador y seleccione la consola para ver los registros. Es posible que tenga que actualizar la página para ver los registros de SignalR negociando el método de transporte para una nueva conexión.

    * Si está ejecutando Internet Explorer 10 en Windows 8 (IIS 8), el método de transporte es **WebSockets**.

    * Si está ejecutando Internet Explorer 10 en Windows 7 (IIS 7.5), el método de transporte es **iframe**.

    * Si está ejecutando Firefox 19 en Windows 8 (IIS 8), el método de transporte es **WebSockets**.

        > [!TIP]
        > En Firefox, instala el complemento Firebug para obtener una ventana de consola.

    * Si está ejecutando Firefox 19 en Windows 7 (IIS 7.5), el método de transporte es eventos **enviados por el servidor.**

## <a name="install-the-stockticker-sample"></a>Instale el ejemplo StockTicker

[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) instala la aplicación StockTicker. El paquete NuGet incluye más características que la versión simplificada que creó desde cero. En esta sección del tutorial, instalará el paquete NuGet y revisará las nuevas características y el código que las implementa.

> [!IMPORTANT]
> Si instala el paquete sin realizar los pasos anteriores de este tutorial, debe agregar una clase de inicio OWIN al proyecto. Este archivo readme.txt para el paquete NuGet explica este paso.

### <a name="install-the-signalrsample-nuget-package"></a>Instale el paquete NuGet SignalR.Sample

1. En **el Explorador**de soluciones , haga clic con el botón secundario en el proyecto y seleccione Administrar **paquetes NuGet**.

1. En **el Administrador de paquetes NuGet: SignalR.StockTicker**, seleccione **Examinar**.

1. En **Origen del paquete**, seleccione **nuget.org**.

1. Escriba *SignalR.Sample* en el cuadro de búsqueda y seleccione **Microsoft.AspNet.SignalR.Sample** > **Install**.

1. En el **Explorador**de soluciones , expanda la carpeta *SignalR.Sample.*

    La instalación del paquete SignalR.Sample creó la carpeta y su contenido.

1. En la carpeta *SignalR.Sample* , haga clic con el botón derecho en *StockTicker.html*y, a continuación, seleccione **Establecer como página**de inicio .

    > [!NOTE]
    > La instalación del paquete NuGet SignalR.Sample podría cambiar la versión de jQuery que tiene en la carpeta *Scripts.* El nuevo archivo *StockTicker.html* que instala el paquete en la carpeta *SignalR.Sample* estará sincronizado con la versión de jQuery que instala el paquete, pero si desea volver a ejecutar el archivo *StockTicker.html* original, es posible que primero tenga que actualizar la referencia de jQuery en la etiqueta de script.

### <a name="run-the-application"></a>Ejecución de la aplicación

 La tabla que vio en la primera aplicación tenía características útiles. La aplicación de ticker de stock completo muestra nuevas características: una ventana de desplazamiento horizontal que muestra los datos de stock y las existencias que cambian de color a medida que se elevan y bajan.

1. Presione **F5** para ejecutar la aplicación.

     Cuando ejecuta sin tientas la aplicación por primera vez, el "mercado" se "cierra" y ves una tabla estática y una ventana de ticker que no se está desplazando.

1. Seleccione **Mercado abierto**.

    ![Captura de pantalla del ticker en vivo.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * La casilla **Live Stock Ticker** comienza a desplazarse horizontalmente, y el servidor comienza a transmitir periódicamente los cambios en el precio de las acciones de forma aleatoria.

    * Cada vez que cambia el precio de una acción, la aplicación actualiza tanto la tabla de acciones en **vivo** como el **Live Stock Ticker**.

    * Cuando el cambio de precio de una acción es positivo, la aplicación muestra la acción con un fondo verde.

    * Cuando el cambio es negativo, la aplicación muestra el stock con un fondo rojo.

1. Seleccione **Cerrar mercado**.

    * Las actualizaciones de la tabla se detienen.

    * El ticker deja de desplazarse.

1. Seleccione **Restablecer**.

    * Se restablecen todos los datos de stock.

    * La aplicación restaura el estado inicial antes de que se inicien los cambios de precio.

1. Copie la dirección URL del explorador, abra otros dos exploradores y pegue las direcciones URL en las barras de direcciones.

1. Verá los mismos datos actualizados dinámicamente al mismo tiempo en cada navegador.

1. Al seleccionar cualquiera de los controles, todos los navegadores responden de la misma manera al mismo tiempo.

### <a name="live-stock-ticker-display"></a>Pantalla Live Stock Ticker

La pantalla **Live Stock Ticker** es `<div>` una lista desordenada en un elemento formateado en una sola línea por estilos CSS. La aplicación inicializa y actualiza el ticker de la misma manera `<li>` que la tabla: reemplazando marcadores de posición en una cadena de plantilla y agregando dinámicamente los `<li>` elementos al `<ul>` elemento. La aplicación incluye el desplazamiento `animate` mediante la función jQuery para variar `<div>`el margen izquierdo de la lista desordenada dentro de la .

#### <a name="signalrsample-stocktickerhtml"></a>SignalR.Sample StockTicker.html

El código HTML del ticker de stock:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a>SignalR.Sample StockTicker.css

El código CSS del ticker de stock:

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a>SignalR.Sample SignalR.StockTicker.js

El código jQuery que lo hace desplazarse:

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Métodos adicionales en el servidor que el cliente puede llamar

Para agregar flexibilidad a la aplicación, hay métodos adicionales a los que la aplicación puede llamar.

#### <a name="signalrsample-stocktickerhubcs"></a>SignalR.Sample StockTickerHub.cs

La `StockTickerHub` clase define cuatro métodos adicionales a los que el cliente puede llamar:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

La aplicación `OpenMarket` `CloseMarket`llama `Reset` , , y en respuesta a los botones en la parte superior de la página. Muestran el patrón de un cliente que desencadena un cambio de estado que se propaga inmediatamente a todos los clientes. Cada uno de estos métodos llama a un método en la `StockTicker` clase que provoca el cambio de estado de mercado y, a continuación, difunde el nuevo estado.

#### <a name="signalrsample-stocktickercs"></a>SignalR.Sample StockTicker.cs

En `StockTicker` la clase, la aplicación mantiene el `MarketState` estado del `MarketState` mercado con una propiedad que devuelve un valor de enumeración:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Cada uno de los métodos que cambian el `StockTicker` estado del mercado lo hace dentro de un bloque de bloqueo porque la clase tiene que ser segura para subprocesos:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

Para asegurarse de que este `_marketState` código es seguro `MarketState` para `volatile`subprocesos, el campo que respalda la propiedad designada:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

Los `BroadcastMarketStateChange` `BroadcastMarketReset` métodos y son similares al método BroadcastStockPrice que ya ha visto, excepto que llaman a diferentes métodos definidos en el cliente:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Funciones adicionales en el cliente a las que el servidor puede llamar

La `updateStockPrice` función ahora maneja tanto la tabla como `jQuery.Color` la pantalla del ticker, y se utiliza para parpadear los colores rojo y verde.

Las nuevas funciones de *SignalR.StockTicker.js* habilitan y desactivan los botones en función del estado del mercado. También detienen o inician el desplazamiento horizontal de **Live Stock Ticker.** Dado que se agregan `ticker.client`muchas funciones a , la aplicación utiliza la [función de extensión jQuery](http://api.jquery.com/jQuery.extend/) para agregarlas.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Configuración adicional del cliente después de establecer la conexión

Después de que el cliente establezca la conexión, tiene algo de trabajo adicional que hacer:

* Averiguar si el mercado está abierto o `marketOpened` `marketClosed` cerrado para llamar a la adecuada o función.

* Asocie las llamadas al método de servidor a los botones.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Los métodos de servidor no se conectan a los botones hasta después de que la aplicación establezca la conexión. Es para que el código no pueda llamar a los métodos de servidor antes de que estén disponibles.

## <a name="additional-resources"></a>Recursos adicionales

En este tutorial ha aprendido a programar una aplicación de SignalR que difunde mensajes desde el servidor a todos los clientes conectados. Ahora puede transmitir mensajes de forma periódica y en respuesta a notificaciones de cualquier cliente. Puede utilizar el concepto de instancia singleton multiproceso para mantener el estado del servidor en escenarios de juego en línea multijugador. Para ver un ejemplo, consulta [el juego ShootR basado en SignalR](https://github.com/NTaylorMullen/ShootR).

Para ver tutoriales que muestran escenarios de comunicación punto a punto, consulte [Introducción a SignalR](introduction-to-signalr.md) y [Actualización en tiempo real con SignalR](tutorial-high-frequency-realtime-with-signalr.md).

Para obtener más información acerca de SignalR, consulte los siguientes recursos:

* [ASP.NET SignalR](../../index.md)
* [Proyecto SignalR](http://signalr.net/)
* [SignalR GitHub y muestras](https://github.com/SignalR/SignalR)
* [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, hizo lo siguiente:

> [!div class="checklist"]
> * Creado el proyecto
> * Configuración del código de servidor
> * Examinó el código del servidor
> * Configurar el código de cliente
> * Examinó el código de cliente
> * Probar la aplicación
> * Registro habilitado

Avance al siguiente artículo para aprender a crear una aplicación web en tiempo real que use ASP.NET SignalR 2.
> [!div class="nextstepaction"]
> [Cree una aplicación web en tiempo real con SignalR](real-time-web-applications-with-signalr.md)
