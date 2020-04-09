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
# <a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="7052f-103">Tutorial: Transmisión del servidor con SignalR 2</span><span class="sxs-lookup"><span data-stu-id="7052f-103">Tutorial: Server broadcast with SignalR 2</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="7052f-104">En este tutorial se muestra cómo crear una aplicación web que usa ASP.NET SignalR 2 para proporcionar funcionalidad de difusión del servidor.</span><span class="sxs-lookup"><span data-stu-id="7052f-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="7052f-105">La difusión del servidor significa que el servidor inicia las comunicaciones enviadas a los clientes.</span><span class="sxs-lookup"><span data-stu-id="7052f-105">Server broadcast means that the server starts the communications sent to clients.</span></span>

<span data-ttu-id="7052f-106">La aplicación que creará en este tutorial simula un ticker de acciones, un escenario típico para la funcionalidad de difusión del servidor.</span><span class="sxs-lookup"><span data-stu-id="7052f-106">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span> <span data-ttu-id="7052f-107">Periódicamente, el servidor actualiza aleatoriamente los precios de las acciones y transmite las actualizaciones a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="7052f-107">Periodically, the server randomly updates stock prices and broadcast the updates to all connected clients.</span></span> <span data-ttu-id="7052f-108">En el explorador, los números **%** y símbolos del **cambio** y las columnas cambian dinámicamente en respuesta a las notificaciones del servidor.</span><span class="sxs-lookup"><span data-stu-id="7052f-108">In the browser, the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="7052f-109">Si abre navegadores adicionales en la misma URL, todos mostrarán los mismos datos y los mismos cambios en los datos simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="7052f-109">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

![Crear web](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="7052f-111">En este tutorial, hizo lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7052f-111">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7052f-112">Creación del proyecto</span><span class="sxs-lookup"><span data-stu-id="7052f-112">Create the project</span></span>
> * <span data-ttu-id="7052f-113">Configuración del código de servidor</span><span class="sxs-lookup"><span data-stu-id="7052f-113">Set up the server code</span></span>
> * <span data-ttu-id="7052f-114">Examine el código del servidor</span><span class="sxs-lookup"><span data-stu-id="7052f-114">Examine the server code</span></span>
> * <span data-ttu-id="7052f-115">Configurar el código de cliente</span><span class="sxs-lookup"><span data-stu-id="7052f-115">Set up the client code</span></span>
> * <span data-ttu-id="7052f-116">Examine el código de cliente</span><span class="sxs-lookup"><span data-stu-id="7052f-116">Examine the client code</span></span>
> * <span data-ttu-id="7052f-117">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="7052f-117">Test the application</span></span>
> * <span data-ttu-id="7052f-118">Habilitación del registro</span><span class="sxs-lookup"><span data-stu-id="7052f-118">Enable logging</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7052f-119">Si no desea trabajar en los pasos de creación de la aplicación, puede instalar el paquete SignalR.Sample en un nuevo proyecto de aplicación web de ASP.NET vacía.</span><span class="sxs-lookup"><span data-stu-id="7052f-119">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="7052f-120">Si instala el paquete NuGet sin realizar los pasos de este tutorial, debe seguir las instrucciones del archivo *readme.txt.*</span><span class="sxs-lookup"><span data-stu-id="7052f-120">If you install the NuGet package without performing the steps in this tutorial, you must follow the instructions in the *readme.txt* file.</span></span> <span data-ttu-id="7052f-121">Para ejecutar el paquete, debe agregar una clase `ConfigureSignalR` de inicio OWIN que llame al método en el paquete instalado.</span><span class="sxs-lookup"><span data-stu-id="7052f-121">To run the package you need to add an OWIN startup class which calls the `ConfigureSignalR` method in the installed package.</span></span> <span data-ttu-id="7052f-122">Recibirá un error si no agrega la clase de inicio OWIN.</span><span class="sxs-lookup"><span data-stu-id="7052f-122">You will receive an error if you do not add the OWIN startup class.</span></span> <span data-ttu-id="7052f-123">Consulte la sección [de ejemplo Instalar StockTicker](#install-the-stockticker-sample) de este artículo.</span><span class="sxs-lookup"><span data-stu-id="7052f-123">See the [Install the StockTicker sample](#install-the-stockticker-sample) section of this article.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7052f-124">Prerrequisitos</span><span class="sxs-lookup"><span data-stu-id="7052f-124">Prerequisites</span></span>

* <span data-ttu-id="7052f-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con la carga de trabajo de **ASP.NET y desarrollo web**.</span><span class="sxs-lookup"><span data-stu-id="7052f-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="7052f-126">Creación del proyecto</span><span class="sxs-lookup"><span data-stu-id="7052f-126">Create the project</span></span>

<span data-ttu-id="7052f-127">En esta sección se muestra cómo usar Visual Studio 2017 para crear una aplicación web de ASP.NET vacía.</span><span class="sxs-lookup"><span data-stu-id="7052f-127">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application.</span></span>

1. <span data-ttu-id="7052f-128">En Visual Studio, cree una aplicación web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7052f-128">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Crear web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. <span data-ttu-id="7052f-130">En la ventana **Nueva aplicación web ASP.NET - SignalR.StockTicker,** deje **vacío** seleccionado y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="7052f-130">In the **New ASP.NET Web Application - SignalR.StockTicker** window, leave **Empty** selected and select **OK**.</span></span>

## <a name="set-up-the-server-code"></a><span data-ttu-id="7052f-131">Configuración del código de servidor</span><span class="sxs-lookup"><span data-stu-id="7052f-131">Set up the server code</span></span>

<span data-ttu-id="7052f-132">En esta sección, configurará el código que se ejecuta en el servidor.</span><span class="sxs-lookup"><span data-stu-id="7052f-132">In this section, you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="7052f-133">Crear la clase Stock</span><span class="sxs-lookup"><span data-stu-id="7052f-133">Create the Stock class</span></span>

<span data-ttu-id="7052f-134">Para empezar, cree la clase de modelo *Stock* que utilizará para almacenar y transmitir información sobre un stock.</span><span class="sxs-lookup"><span data-stu-id="7052f-134">You begin by creating the *Stock* model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="7052f-135">En **el Explorador**de soluciones , haga clic con el botón secundario en el proyecto y seleccione **Agregar** > **clase**.</span><span class="sxs-lookup"><span data-stu-id="7052f-135">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="7052f-136">Asigne un nombre a la clase *Stock* y agréguela al proyecto.</span><span class="sxs-lookup"><span data-stu-id="7052f-136">Name the class *Stock* and add it to the project.</span></span>

1. <span data-ttu-id="7052f-137">Reemplace el código del archivo *Stock.cs* por este código:</span><span class="sxs-lookup"><span data-stu-id="7052f-137">Replace the code in the *Stock.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="7052f-138">Las dos propiedades que establecerá al crear `Symbol` stocks son (por ejemplo, `Price`MSFT para Microsoft) y .</span><span class="sxs-lookup"><span data-stu-id="7052f-138">The two properties that you'll set when you create stocks are `Symbol` (for example, MSFT for Microsoft) and `Price`.</span></span> <span data-ttu-id="7052f-139">Las otras propiedades dependen de `Price`cómo y cuándo se establece .</span><span class="sxs-lookup"><span data-stu-id="7052f-139">The other properties depend on how and when you set `Price`.</span></span> <span data-ttu-id="7052f-140">La primera vez `Price`que se establece , `DayOpen`el valor se propaga a .</span><span class="sxs-lookup"><span data-stu-id="7052f-140">The first time you set `Price`, the value gets propagated to `DayOpen`.</span></span> <span data-ttu-id="7052f-141">Después de eso, cuando se `Price` `Change` establece `PercentChange` , la aplicación `Price` calcula `DayOpen`los valores de propiedad y en función de la diferencia entre y .</span><span class="sxs-lookup"><span data-stu-id="7052f-141">After that, when you set `Price`, the app calculates the `Change` and `PercentChange` property values based on the difference between `Price` and `DayOpen`.</span></span>

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a><span data-ttu-id="7052f-142">Cree las clases StockTickerHub y StockTicker</span><span class="sxs-lookup"><span data-stu-id="7052f-142">Create the StockTickerHub and StockTicker classes</span></span>

<span data-ttu-id="7052f-143">Usará la API de SignalR Hub para controlar la interacción de servidor a cliente.</span><span class="sxs-lookup"><span data-stu-id="7052f-143">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="7052f-144">Una `StockTickerHub` clase que deriva de `Hub` la SignalR clase controlará la recepción de conexiones y llamadas de método de los clientes.</span><span class="sxs-lookup"><span data-stu-id="7052f-144">A `StockTickerHub` class that derives from the SignalR `Hub` class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="7052f-145">También debe actualizar los datos `Timer` de stock y ejecutar un objeto.</span><span class="sxs-lookup"><span data-stu-id="7052f-145">You also need to maintain stock data and run a `Timer` object.</span></span> <span data-ttu-id="7052f-146">El `Timer` objeto desencadenará periódicamente actualizaciones de precios independientemente de las conexiones de cliente.</span><span class="sxs-lookup"><span data-stu-id="7052f-146">The `Timer` object will periodically trigger price updates independent of client connections.</span></span> <span data-ttu-id="7052f-147">No se pueden colocar estas `Hub` funciones en una clase, porque los concentradores son transitorios.</span><span class="sxs-lookup"><span data-stu-id="7052f-147">You can't put these functions in a `Hub` class, because Hubs are transient.</span></span> <span data-ttu-id="7052f-148">La aplicación `Hub` crea una instancia de clase para cada tarea en el concentrador, como conexiones y llamadas desde el cliente al servidor.</span><span class="sxs-lookup"><span data-stu-id="7052f-148">The app creates a `Hub` class instance for each task on the hub, like connections and calls from the client to the server.</span></span> <span data-ttu-id="7052f-149">Por lo tanto, el mecanismo que mantiene los datos de stock, actualiza los precios y difunde las actualizaciones de precios tiene que ejecutarse en una clase independiente.</span><span class="sxs-lookup"><span data-stu-id="7052f-149">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class.</span></span> <span data-ttu-id="7052f-150">Nombrará la clase `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="7052f-150">You'll name the class `StockTicker`.</span></span>

![Radiodifusión desde StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

<span data-ttu-id="7052f-152">Solo desea que una `StockTicker` instancia de la clase se ejecute en el servidor, `StockTickerHub` por lo que `StockTicker` deberá configurar una referencia de cada instancia a la instancia singleton.</span><span class="sxs-lookup"><span data-stu-id="7052f-152">You only want one instance of the `StockTicker` class to run on the server, so you'll need to set up a reference from each `StockTickerHub` instance to the singleton `StockTicker` instance.</span></span> <span data-ttu-id="7052f-153">La `StockTicker` clase tiene que transmitir a los clientes porque `StockTicker` tiene los `Hub` datos de stock y desencadena actualizaciones, pero no es una clase.</span><span class="sxs-lookup"><span data-stu-id="7052f-153">The `StockTicker` class has to broadcast to clients because it has the stock data and triggers updates, but `StockTicker` isn't a `Hub` class.</span></span> <span data-ttu-id="7052f-154">La `StockTicker` clase tiene que obtener una referencia al objeto de contexto de conexión de concentrador de SignalR.</span><span class="sxs-lookup"><span data-stu-id="7052f-154">The `StockTicker` class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="7052f-155">A continuación, puede utilizar el objeto de contexto de conexión SignalR para difundir a los clientes.</span><span class="sxs-lookup"><span data-stu-id="7052f-155">It can then use the SignalR connection context object to broadcast to clients.</span></span>

#### <a name="create-stocktickerhubcs"></a><span data-ttu-id="7052f-156">Crear StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="7052f-156">Create StockTickerHub.cs</span></span>

1. <span data-ttu-id="7052f-157">En **el Explorador**de soluciones , haga clic con el botón secundario en el proyecto y seleccione **Agregar** > nuevo**elemento**.</span><span class="sxs-lookup"><span data-stu-id="7052f-157">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="7052f-158">En **Agregar nuevo elemento - SignalR.StockTicker**, seleccione **Instalado** > **Visual C-** > **Web** > **SignalR** y, a continuación, seleccione **SignalR Hub Class (v2)**.</span><span class="sxs-lookup"><span data-stu-id="7052f-158">In **Add New Item - SignalR.StockTicker**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="7052f-159">Asigne a la clase el nombre *StockTickerHub* y agréguela al proyecto.</span><span class="sxs-lookup"><span data-stu-id="7052f-159">Name the class *StockTickerHub* and add it to the project.</span></span>

    <span data-ttu-id="7052f-160">Este paso crea el archivo de clase *StockTickerHub.cs.*</span><span class="sxs-lookup"><span data-stu-id="7052f-160">This step creates the *StockTickerHub.cs* class file.</span></span> <span data-ttu-id="7052f-161">Simultáneamente, agrega un conjunto de archivos de script y referencias de ensamblado que admite SignalR al proyecto.</span><span class="sxs-lookup"><span data-stu-id="7052f-161">Simultaneously, it adds a set of script files and assembly references that supports SignalR to the project.</span></span>

1. <span data-ttu-id="7052f-162">Reemplace el código del archivo *StockTickerHub.cs* por este código:</span><span class="sxs-lookup"><span data-stu-id="7052f-162">Replace the code in the *StockTickerHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="7052f-163">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="7052f-163">Save the file.</span></span>

<span data-ttu-id="7052f-164">La aplicación usa la clase [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) para definir métodos a los que los clientes pueden llamar en el servidor.</span><span class="sxs-lookup"><span data-stu-id="7052f-164">The app uses the [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class to define methods the clients can call on the server.</span></span> <span data-ttu-id="7052f-165">Estás definiendo un método: `GetAllStocks()`.</span><span class="sxs-lookup"><span data-stu-id="7052f-165">You're defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="7052f-166">Cuando un cliente se conecta inicialmente al servidor, llamará a este método para obtener una lista de todas las acciones con sus precios actuales.</span><span class="sxs-lookup"><span data-stu-id="7052f-166">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="7052f-167">El método se puede `IEnumerable<Stock>` ejecutar sincrónicamente y devolver porque devuelve datos de la memoria.</span><span class="sxs-lookup"><span data-stu-id="7052f-167">The method can run synchronously and return `IEnumerable<Stock>` because it's returning data from memory.</span></span>

<span data-ttu-id="7052f-168">Si el método tuviera que obtener los datos haciendo algo que implicaría esperar, como `Task<IEnumerable<Stock>>` una búsqueda de base de datos o una llamada de servicio web, especificaría como valor devuelto para habilitar el procesamiento asincrónico.</span><span class="sxs-lookup"><span data-stu-id="7052f-168">If the method had to get the data by doing something that would involve waiting, like a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="7052f-169">Para obtener más información, consulte [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span><span class="sxs-lookup"><span data-stu-id="7052f-169">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

<span data-ttu-id="7052f-170">El `HubName` atributo especifica cómo la aplicación hará referencia al concentrador en código JavaScript en el cliente.</span><span class="sxs-lookup"><span data-stu-id="7052f-170">The `HubName` attribute specifies how the app will reference the Hub in JavaScript code on the client.</span></span> <span data-ttu-id="7052f-171">El nombre predeterminado en el cliente si no utiliza este atributo, es una versión camelCase del nombre de clase, que en este caso sería `stockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="7052f-171">The default name on the client if you don't use this attribute, is a camelCase version of the class name, which in this case would be `stockTickerHub`.</span></span>

<span data-ttu-id="7052f-172">Como verá más adelante cuando `StockTicker` cree la clase, la aplicación crea una instancia `Instance` singleton de esa clase en su propiedad estática.</span><span class="sxs-lookup"><span data-stu-id="7052f-172">As you'll see later when you create the `StockTicker` class, the app creates a singleton instance of that class in its static `Instance` property.</span></span> <span data-ttu-id="7052f-173">Esa instancia singleton de `StockTicker` está en la memoria no importa cuántos clientes se conecten o desconecten.</span><span class="sxs-lookup"><span data-stu-id="7052f-173">That singleton instance of `StockTicker` is in memory no matter how many clients connect or disconnect.</span></span> <span data-ttu-id="7052f-174">Esa instancia `GetAllStocks()` es lo que el método utiliza para devolver la información de stock actual.</span><span class="sxs-lookup"><span data-stu-id="7052f-174">That instance is what the `GetAllStocks()` method uses to return current stock information.</span></span>

#### <a name="create-stocktickercs"></a><span data-ttu-id="7052f-175">Crear StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="7052f-175">Create StockTicker.cs</span></span>

1. <span data-ttu-id="7052f-176">En **el Explorador**de soluciones , haga clic con el botón secundario en el proyecto y seleccione **Agregar** > **clase**.</span><span class="sxs-lookup"><span data-stu-id="7052f-176">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="7052f-177">Asigne un nombre a la clase *StockTicker* y agréguela al proyecto.</span><span class="sxs-lookup"><span data-stu-id="7052f-177">Name the class *StockTicker* and add it to the project.</span></span>

1. <span data-ttu-id="7052f-178">Reemplace el código del archivo *StockTicker.cs* por este código:</span><span class="sxs-lookup"><span data-stu-id="7052f-178">Replace the code in the *StockTicker.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

<span data-ttu-id="7052f-179">Dado que todos los subprocesos van a ejecutar la misma instancia de código StockTicker, la clase StockTicker tiene que ser segura para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="7052f-179">Since all threads will be running the same instance of StockTicker code, the StockTicker class has to be thread-safe.</span></span>

### <a name="examine-the-server-code"></a><span data-ttu-id="7052f-180">Examine el código del servidor</span><span class="sxs-lookup"><span data-stu-id="7052f-180">Examine the server code</span></span>

<span data-ttu-id="7052f-181">Si examina el código del servidor, le ayudará a comprender cómo funciona la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7052f-181">If you examine the server code, it will help you understand how the app works.</span></span>

#### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="7052f-182">Almacenamiento de la instancia singleton en un campo estático</span><span class="sxs-lookup"><span data-stu-id="7052f-182">Storing the singleton instance in a static field</span></span>

<span data-ttu-id="7052f-183">El código inicializa `_instance` el campo estático `Instance` que respalda la propiedad con una instancia de la clase.</span><span class="sxs-lookup"><span data-stu-id="7052f-183">The code initializes the static `_instance` field that backs the `Instance` property with an instance of the class.</span></span> <span data-ttu-id="7052f-184">Dado que el constructor es privado, es la única instancia de la clase que la aplicación puede crear.</span><span class="sxs-lookup"><span data-stu-id="7052f-184">Because the constructor is private, it's the only instance of the class that the app can create.</span></span> <span data-ttu-id="7052f-185">La aplicación usa la `_instance` [inicialización diferida](/dotnet/framework/performance/lazy-initialization) para el campo.</span><span class="sxs-lookup"><span data-stu-id="7052f-185">The app uses [Lazy initialization](/dotnet/framework/performance/lazy-initialization) for the `_instance` field.</span></span> <span data-ttu-id="7052f-186">No es por razones de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="7052f-186">It's not for performance reasons.</span></span> <span data-ttu-id="7052f-187">Es para asegurarse de que la creación de la instancia es segura para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="7052f-187">It's to make sure the instance creation is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

<span data-ttu-id="7052f-188">Cada vez que un cliente se conecta al servidor, una nueva instancia de la clase StockTickerHub `StockTicker.Instance` que se ejecuta en un `StockTickerHub` subproceso independiente obtiene la instancia singleton de StockTicker de la propiedad estática, como se vio anteriormente en la clase.</span><span class="sxs-lookup"><span data-stu-id="7052f-188">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the `StockTicker.Instance` static property, as you saw earlier in the `StockTickerHub` class.</span></span>

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="7052f-189">Almacenamiento de datos de stock en un ConcurrentDictionary</span><span class="sxs-lookup"><span data-stu-id="7052f-189">Storing stock data in a ConcurrentDictionary</span></span>

<span data-ttu-id="7052f-190">El constructor inicializa `_stocks` la colección con `GetAllStocks` algunos datos de stock de ejemplo y devuelve las existencias.</span><span class="sxs-lookup"><span data-stu-id="7052f-190">The constructor initializes the `_stocks` collection with some sample stock data, and `GetAllStocks` returns the stocks.</span></span> <span data-ttu-id="7052f-191">Como ha visto anteriormente, esta colección `StockTickerHub.GetAllStocks`de acciones es devuelta por , que es un método de servidor en la `Hub` clase que los clientes pueden llamar.</span><span class="sxs-lookup"><span data-stu-id="7052f-191">As you saw earlier, this collection of stocks is returned by `StockTickerHub.GetAllStocks`, which is a server method in the `Hub` class that clients can call.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

<span data-ttu-id="7052f-192">La colección stocks se define como un tipo [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) para la seguridad de subprocesos.</span><span class="sxs-lookup"><span data-stu-id="7052f-192">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="7052f-193">Como alternativa, puede usar un [objeto Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) y bloquear explícitamente el diccionario cuando realice cambios en él.</span><span class="sxs-lookup"><span data-stu-id="7052f-193">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

<span data-ttu-id="7052f-194">Para esta aplicación de ejemplo, está bien almacenar los datos de la aplicación en `StockTicker` la memoria y perder los datos cuando la aplicación elimina la instancia.</span><span class="sxs-lookup"><span data-stu-id="7052f-194">For this sample application, it's OK to store application data in memory and to lose the data when the app disposes of the  `StockTicker` instance.</span></span> <span data-ttu-id="7052f-195">En una aplicación real, trabajaría con un almacén de datos back-end como una base de datos.</span><span class="sxs-lookup"><span data-stu-id="7052f-195">In a real application, you would work with a back-end data store like a database.</span></span>

#### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="7052f-196">Actualización periódica de los precios de las acciones</span><span class="sxs-lookup"><span data-stu-id="7052f-196">Periodically updating stock prices</span></span>

<span data-ttu-id="7052f-197">El constructor inicia `Timer` un objeto que llama periódicamente a métodos que actualizan los precios de las acciones de forma aleatoria.</span><span class="sxs-lookup"><span data-stu-id="7052f-197">The constructor starts up a `Timer` object that periodically calls methods that update stock prices on a random basis.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 <span data-ttu-id="7052f-198">`Timer`llama `UpdateStockPrices`, que pasa null en el parámetro state.</span><span class="sxs-lookup"><span data-stu-id="7052f-198">`Timer` calls `UpdateStockPrices`, which passes in null in the state parameter.</span></span> <span data-ttu-id="7052f-199">Antes de actualizar los precios, `_updateStockPricesLock` la aplicación toma un bloqueo en el objeto.</span><span class="sxs-lookup"><span data-stu-id="7052f-199">Before updating prices, the app takes a lock on the `_updateStockPricesLock` object.</span></span> <span data-ttu-id="7052f-200">El código comprueba si otro subproceso ya está `TryUpdateStockPrice` actualizando los precios y, a continuación, llama a cada acción de la lista.</span><span class="sxs-lookup"><span data-stu-id="7052f-200">The code checks if another thread is already updating prices, and then it calls `TryUpdateStockPrice` on each stock in the list.</span></span> <span data-ttu-id="7052f-201">El `TryUpdateStockPrice` método decide si se debe cambiar el precio de la acción y cuánto cambiarlo.</span><span class="sxs-lookup"><span data-stu-id="7052f-201">The `TryUpdateStockPrice` method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="7052f-202">Si el precio de la `BroadcastStockPrice` acción cambia, la aplicación llama para transmitir el cambio de precio de la acción a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="7052f-202">If the stock price changes, the app calls `BroadcastStockPrice` to broadcast the stock price change to all connected clients.</span></span>

<span data-ttu-id="7052f-203">La `_updatingStockPrices` marca designada [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) para asegurarse de que es seguro para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="7052f-203">The `_updatingStockPrices` flag designated [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to make sure it is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

<span data-ttu-id="7052f-204">En una aplicación `TryUpdateStockPrice` real, el método llamaría a un servicio web para buscar el precio.</span><span class="sxs-lookup"><span data-stu-id="7052f-204">In a real application, the `TryUpdateStockPrice` method would call a web service to look up the price.</span></span> <span data-ttu-id="7052f-205">En este código, la aplicación utiliza un generador de números aleatorios para realizar cambios al azar.</span><span class="sxs-lookup"><span data-stu-id="7052f-205">In this code, the app uses a random number generator to make changes randomly.</span></span>

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="7052f-206">Obtener el contexto SignalR para que la clase StockTicker pueda transmitir a los clientes</span><span class="sxs-lookup"><span data-stu-id="7052f-206">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

<span data-ttu-id="7052f-207">Dado que los cambios `StockTicker` de precio se originan aquí `updateStockPrice` en el objeto, es el objeto que necesita llamar a un método en todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="7052f-207">Because the price changes originate here in the `StockTicker` object, it's the object that needs to call an `updateStockPrice` method on all connected clients.</span></span> <span data-ttu-id="7052f-208">En `Hub` una clase, tiene una API para `StockTicker` llamar a métodos de cliente, pero no deriva de la `Hub` clase y no tiene una referencia a ningún `Hub` objeto.</span><span class="sxs-lookup"><span data-stu-id="7052f-208">In a `Hub` class, you have an API for calling client methods, but `StockTicker` doesn't derive from the `Hub` class and doesn't have a reference to any `Hub` object.</span></span> <span data-ttu-id="7052f-209">Para transmitir a clientes conectados, la `StockTicker` clase tiene `StockTickerHub` que obtener la instancia de contexto de SignalR para la clase y usarla para llamar a métodos en los clientes.</span><span class="sxs-lookup"><span data-stu-id="7052f-209">To broadcast to connected clients, the `StockTicker` class has to get the SignalR context instance for the `StockTickerHub` class and use that to call methods on clients.</span></span>

<span data-ttu-id="7052f-210">El código obtiene una referencia al contexto de SignalR cuando crea la instancia de clase singleton, pasa `Clients` esa referencia al constructor y el constructor lo coloca en la propiedad.</span><span class="sxs-lookup"><span data-stu-id="7052f-210">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the `Clients` property.</span></span>

<span data-ttu-id="7052f-211">Hay dos razones por las que desea obtener el contexto una sola vez: obtener el contexto es una tarea costosa y obtenerlo una vez se asegura de que la aplicación conserva el orden previsto de los mensajes enviados a los clientes.</span><span class="sxs-lookup"><span data-stu-id="7052f-211">There are two reasons why you want to get the context only once: getting the context is an expensive task, and getting it once makes sure the app preserves the intended order of messages sent to the clients.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

<span data-ttu-id="7052f-212">Obtener `Clients` la propiedad del contexto y `StockTickerClient` colocarla en la propiedad le permite escribir código `Hub` para llamar a métodos de cliente que tiene el mismo aspecto que en una clase.</span><span class="sxs-lookup"><span data-stu-id="7052f-212">Getting the `Clients` property of the context and putting it in the `StockTickerClient` property lets you write code to call client methods that looks the same as it would in a `Hub` class.</span></span> <span data-ttu-id="7052f-213">Por ejemplo, para transmitir a `Clients.All.updateStockPrice(stock)`todos los clientes puede escribir .</span><span class="sxs-lookup"><span data-stu-id="7052f-213">For instance, to broadcast to all clients you can write `Clients.All.updateStockPrice(stock)`.</span></span>

<span data-ttu-id="7052f-214">El `updateStockPrice` método al `BroadcastStockPrice` que llamas aún no existe.</span><span class="sxs-lookup"><span data-stu-id="7052f-214">The `updateStockPrice` method that you're calling in `BroadcastStockPrice` doesn't exist yet.</span></span> <span data-ttu-id="7052f-215">Lo agregará más adelante cuando escriba código que se ejecute en el cliente.</span><span class="sxs-lookup"><span data-stu-id="7052f-215">You'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="7052f-216">Puede hacer `updateStockPrice` referencia `Clients.All` aquí porque es dinámico, lo que significa que la aplicación evaluará la expresión en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="7052f-216">You can refer to `updateStockPrice` here because `Clients.All` is dynamic, which means the app will evaluate the expression at runtime.</span></span> <span data-ttu-id="7052f-217">Cuando se ejecuta esta llamada al método, SignalR enviará el nombre del método y `updateStockPrice`el valor del parámetro al cliente, y si el cliente tiene un método denominado , la aplicación llamará a ese método y le pasará el valor del parámetro.</span><span class="sxs-lookup"><span data-stu-id="7052f-217">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named `updateStockPrice`, the app will call that method and pass the parameter value to it.</span></span>

<span data-ttu-id="7052f-218">`Clients.All`significa enviar a todos los clientes.</span><span class="sxs-lookup"><span data-stu-id="7052f-218">`Clients.All` means send to all clients.</span></span> <span data-ttu-id="7052f-219">SignalR le ofrece otras opciones para especificar a qué clientes o grupos de clientes enviar.</span><span class="sxs-lookup"><span data-stu-id="7052f-219">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="7052f-220">Para obtener más información, vea [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="7052f-220">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="7052f-221">Registre la ruta SignalR</span><span class="sxs-lookup"><span data-stu-id="7052f-221">Register the SignalR route</span></span>

<span data-ttu-id="7052f-222">El servidor necesita saber qué dirección URL interceptar y dirigir a SignalR.</span><span class="sxs-lookup"><span data-stu-id="7052f-222">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="7052f-223">Para ello, agregue una clase de inicio OWIN:</span><span class="sxs-lookup"><span data-stu-id="7052f-223">To do that, add an OWIN startup class:</span></span>

1. <span data-ttu-id="7052f-224">En **el Explorador**de soluciones , haga clic con el botón secundario en el proyecto y seleccione **Agregar** > nuevo**elemento**.</span><span class="sxs-lookup"><span data-stu-id="7052f-224">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="7052f-225">En **Agregar nuevo elemento - SignalR.StockTicker** seleccione **Instalado** > **Visual C-** > **Web** y, a continuación, seleccione **OWIN Startup Class**.</span><span class="sxs-lookup"><span data-stu-id="7052f-225">In **Add New Item - SignalR.StockTicker** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="7052f-226">Asigne un nombre a la clase *Inicio* y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="7052f-226">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="7052f-227">Reemplace el código predeterminado del archivo *Startup.cs* por este código:</span><span class="sxs-lookup"><span data-stu-id="7052f-227">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="7052f-228">Ya ha terminado de configurar el código del servidor.</span><span class="sxs-lookup"><span data-stu-id="7052f-228">You have now finished setting up the server code.</span></span> <span data-ttu-id="7052f-229">En la siguiente sección, configurará el cliente.</span><span class="sxs-lookup"><span data-stu-id="7052f-229">In the next section, you'll set up the client.</span></span>

## <a name="set-up-the-client-code"></a><span data-ttu-id="7052f-230">Configurar el código de cliente</span><span class="sxs-lookup"><span data-stu-id="7052f-230">Set up the client code</span></span>

<span data-ttu-id="7052f-231">En esta sección, configurará el código que se ejecuta en el cliente.</span><span class="sxs-lookup"><span data-stu-id="7052f-231">In this section, you set up the code that runs on the client.</span></span>

### <a name="create-the-html-page-and-javascript-file"></a><span data-ttu-id="7052f-232">Cree la página HTML y el archivo JavaScript</span><span class="sxs-lookup"><span data-stu-id="7052f-232">Create the HTML page and JavaScript file</span></span>

<span data-ttu-id="7052f-233">La página HTML mostrará los datos y el archivo JavaScript organizará los datos.</span><span class="sxs-lookup"><span data-stu-id="7052f-233">The HTML page will display the data and the JavaScript file will organize the data.</span></span>

#### <a name="create-stocktickerhtml"></a><span data-ttu-id="7052f-234">Crear StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="7052f-234">Create StockTicker.html</span></span>

<span data-ttu-id="7052f-235">En primer lugar, agregará el cliente HTML.</span><span class="sxs-lookup"><span data-stu-id="7052f-235">First, you'll add the HTML client.</span></span>

1. <span data-ttu-id="7052f-236">En **el Explorador**de soluciones , haga clic con el botón secundario en el proyecto y seleccione **Agregar** > página**HTML**.</span><span class="sxs-lookup"><span data-stu-id="7052f-236">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="7052f-237">Asigne al archivo el nombre *StockTicker* y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="7052f-237">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="7052f-238">Reemplace el código predeterminado en el archivo *StockTicker.html* por este código:</span><span class="sxs-lookup"><span data-stu-id="7052f-238">Replace the default code in the *StockTicker.html* file with this code:</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    <span data-ttu-id="7052f-239">El código HTML crea una tabla con cinco columnas, una fila de encabezado y una fila de datos con una sola celda que abarca las cinco columnas.</span><span class="sxs-lookup"><span data-stu-id="7052f-239">The HTML creates a table with five columns, a header row, and a data row with a single cell that spans all five columns.</span></span> <span data-ttu-id="7052f-240">La fila de datos muestra "carga..." momentáneamente cuando se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7052f-240">The data row shows "loading..." momentarily when the app starts.</span></span> <span data-ttu-id="7052f-241">El código JavaScript eliminará esa fila y agregará en su lugar filas con datos de stock recuperados del servidor.</span><span class="sxs-lookup"><span data-stu-id="7052f-241">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="7052f-242">Las etiquetas de script especifican:</span><span class="sxs-lookup"><span data-stu-id="7052f-242">The script tags specify:</span></span>

    * <span data-ttu-id="7052f-243">El archivo de script jQuery.</span><span class="sxs-lookup"><span data-stu-id="7052f-243">The jQuery script file.</span></span>

    * <span data-ttu-id="7052f-244">El archivo de script principal de SignalR.</span><span class="sxs-lookup"><span data-stu-id="7052f-244">The SignalR core script file.</span></span>

    * <span data-ttu-id="7052f-245">El archivo de script de proxies de SignalR.</span><span class="sxs-lookup"><span data-stu-id="7052f-245">The SignalR proxies script file.</span></span>

    * <span data-ttu-id="7052f-246">Un archivo de script StockTicker que creará más adelante.</span><span class="sxs-lookup"><span data-stu-id="7052f-246">A StockTicker script file that you'll create later.</span></span>

    <span data-ttu-id="7052f-247">La aplicación genera dinámicamente el archivo de script de proxies de SignalR.</span><span class="sxs-lookup"><span data-stu-id="7052f-247">The app dynamically generates the SignalR proxies script file.</span></span> <span data-ttu-id="7052f-248">Especifica la dirección URL "/signalr/hubs" y define métodos proxy para los `StockTickerHub.GetAllStocks`métodos de la clase Hub, en este caso, para .</span><span class="sxs-lookup"><span data-stu-id="7052f-248">It specifies the "/signalr/hubs" URL and defines proxy methods for the methods on the Hub class, in this case, for `StockTickerHub.GetAllStocks`.</span></span> <span data-ttu-id="7052f-249">Si lo prefiere, puede generar este archivo JavaScript manualmente mediante [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span><span class="sxs-lookup"><span data-stu-id="7052f-249">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span></span> <span data-ttu-id="7052f-250">No olvide deshabilitar la creación dinámica `MapHubs` de archivos en la llamada al método.</span><span class="sxs-lookup"><span data-stu-id="7052f-250">Don't forget to disable dynamic file creation in the `MapHubs` method call.</span></span>

1. <span data-ttu-id="7052f-251">En **el Explorador**de soluciones , expanda **scripts**.</span><span class="sxs-lookup"><span data-stu-id="7052f-251">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="7052f-252">Las bibliotecas de scripts para jQuery y SignalR son visibles en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7052f-252">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="7052f-253">El administrador de paquetes instalará una versión posterior de los scripts de SignalR.</span><span class="sxs-lookup"><span data-stu-id="7052f-253">The package manager will install a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="7052f-254">Actualice las referencias de script en el bloque de código para que correspondan a las versiones de los archivos de script del proyecto.</span><span class="sxs-lookup"><span data-stu-id="7052f-254">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

1. <span data-ttu-id="7052f-255">En **el Explorador**de soluciones , haga clic con el botón secundario en *StockTicker.html*y, a continuación, seleccione **Establecer como página de inicio**.</span><span class="sxs-lookup"><span data-stu-id="7052f-255">In **Solution Explorer**, right-click *StockTicker.html*, and then select **Set as Start Page**.</span></span>

#### <a name="create-stocktickerjs"></a><span data-ttu-id="7052f-256">Crear StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="7052f-256">Create StockTicker.js</span></span>

<span data-ttu-id="7052f-257">Ahora cree el archivo JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7052f-257">Now create the JavaScript file.</span></span>

1. <span data-ttu-id="7052f-258">En **el Explorador**de soluciones , haga clic con el botón secundario en el proyecto y seleccione **Agregar** > **archivo JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="7052f-258">In **Solution Explorer**, right-click the project and select **Add** > **JavaScript File**.</span></span>

1. <span data-ttu-id="7052f-259">Asigne al archivo el nombre *StockTicker* y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="7052f-259">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="7052f-260">Agregue este código al archivo *StockTicker.js:*</span><span class="sxs-lookup"><span data-stu-id="7052f-260">Add this code to the *StockTicker.js* file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a><span data-ttu-id="7052f-261">Examine el código de cliente</span><span class="sxs-lookup"><span data-stu-id="7052f-261">Examine the client code</span></span>

<span data-ttu-id="7052f-262">Si examina el código de cliente, le ayudará a aprender cómo interactúa el código de cliente con el código de servidor para que la aplicación funcione.</span><span class="sxs-lookup"><span data-stu-id="7052f-262">If you examine the client code, it will help you learn how the client code interacts with the server code to make the app work.</span></span>

#### <a name="starting-the-connection"></a><span data-ttu-id="7052f-263">Inicio de la conexión</span><span class="sxs-lookup"><span data-stu-id="7052f-263">Starting the connection</span></span>

<span data-ttu-id="7052f-264">`$.connection`se refiere a los proxies de SignalR.</span><span class="sxs-lookup"><span data-stu-id="7052f-264">`$.connection` refers to the SignalR proxies.</span></span> <span data-ttu-id="7052f-265">El código obtiene una referencia al `StockTickerHub` proxy de la `ticker` clase y lo coloca en la variable.</span><span class="sxs-lookup"><span data-stu-id="7052f-265">The code gets a reference to the proxy for the `StockTickerHub` class and puts it in the `ticker` variable.</span></span> <span data-ttu-id="7052f-266">El nombre de proxy es el `HubName` nombre establecido por el atributo:</span><span class="sxs-lookup"><span data-stu-id="7052f-266">The proxy name is the name that was set by the `HubName` attribute:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

<span data-ttu-id="7052f-267">Después de definir todas las variables y funciones, la última línea de código `start` del archivo inicializa la conexión de SignalR llamando a la función SignalR.</span><span class="sxs-lookup"><span data-stu-id="7052f-267">After you define all the variables and functions, the last line of code in the file initializes the SignalR connection by calling the SignalR `start` function.</span></span> <span data-ttu-id="7052f-268">La `start` función se ejecuta de forma asincrónica y devuelve un [objeto jQuery Deferred](http://api.jquery.com/category/deferred-object/).</span><span class="sxs-lookup"><span data-stu-id="7052f-268">The `start` function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/).</span></span> <span data-ttu-id="7052f-269">Puede llamar a la función done para especificar la función a la que se llamará cuando la aplicación finalice la acción asincrónica.</span><span class="sxs-lookup"><span data-stu-id="7052f-269">You can call the done function to specify the function to call when the app finishes the asynchronous action.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a><span data-ttu-id="7052f-270">Conseguir todas las existencias</span><span class="sxs-lookup"><span data-stu-id="7052f-270">Getting all the stocks</span></span>

<span data-ttu-id="7052f-271">La `init` función `getAllStocks` llama a la función en el servidor y utiliza la información que el servidor devuelve para actualizar la tabla de valores.</span><span class="sxs-lookup"><span data-stu-id="7052f-271">The `init` function calls the `getAllStocks` function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="7052f-272">Tenga en cuenta que, de forma predeterminada, debe usar camelCasing en el cliente aunque el nombre del método esté en mayúsculas y minúsculas en el servidor.</span><span class="sxs-lookup"><span data-stu-id="7052f-272">Notice that, by default, you have to use camelCasing on the client even though the method name is pascal-cased on the server.</span></span> <span data-ttu-id="7052f-273">La regla camelCasing solo se aplica a los métodos, no a los objetos.</span><span class="sxs-lookup"><span data-stu-id="7052f-273">The camelCasing rule only applies to methods, not objects.</span></span> <span data-ttu-id="7052f-274">Por ejemplo, hace `stock.Symbol` `stock.Price`referencia `stock.symbol` a `stock.price`y , no o .</span><span class="sxs-lookup"><span data-stu-id="7052f-274">For example, you refer to `stock.Symbol` and `stock.Price`, not `stock.symbol` or `stock.price`.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

<span data-ttu-id="7052f-275">En `init` el método, la aplicación crea HTML para una fila de `formatStock` tabla para `stock` cada objeto de `supplant` stock recibido del `rowTemplate` servidor llamando `stock` a las propiedades de formato del objeto y, a continuación, llamando para reemplazar los marcadores de posición de la variable con los valores de propiedad del objeto.</span><span class="sxs-lookup"><span data-stu-id="7052f-275">In the `init` method, the app creates HTML for a table row for each stock object received from the server by calling `formatStock` to format properties of the `stock` object, and then by calling `supplant` to replace placeholders in the `rowTemplate` variable with the `stock` object property values.</span></span> <span data-ttu-id="7052f-276">A continuación, el HTML resultante se anexa a la tabla de valores.</span><span class="sxs-lookup"><span data-stu-id="7052f-276">The resulting HTML is then appended to the stock table.</span></span>

> [!NOTE]
> <span data-ttu-id="7052f-277">Se `init` llama pasándolo como `callback` una función que `start` se ejecuta una vez finalizada la función asincrónica.</span><span class="sxs-lookup"><span data-stu-id="7052f-277">You call `init` by passing it in as a `callback` function that executes after the asynchronous `start` function finishes.</span></span> <span data-ttu-id="7052f-278">Si llamó `init` como una instrucción `start`JavaScript independiente después de llamar , la función produciría un error porque se ejecutaría inmediatamente sin esperar a que la función de inicio termine de establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="7052f-278">If you called `init` as a separate JavaScript statement after calling `start`, the function would fail because it would run immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="7052f-279">En ese caso, la `init` función `getAllStocks` intentaría llamar a la función antes de que la aplicación establezca una conexión de servidor.</span><span class="sxs-lookup"><span data-stu-id="7052f-279">In that case, the `init` function would try to call the `getAllStocks` function before the app establishes a server connection.</span></span>

#### <a name="getting-updated-stock-prices"></a><span data-ttu-id="7052f-280">Obtener precios actualizados de las acciones</span><span class="sxs-lookup"><span data-stu-id="7052f-280">Getting updated stock prices</span></span>

<span data-ttu-id="7052f-281">Cuando el servidor cambia el precio de `updateStockPrice` una acción, llama a los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="7052f-281">When the server changes a stock's price, it calls the `updateStockPrice` on connected clients.</span></span> <span data-ttu-id="7052f-282">La aplicación agrega la función a `stockTicker` la propiedad client del proxy para que esté disponible para las llamadas desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="7052f-282">The app adds the function to the client property of the `stockTicker` proxy to make it available to calls from the server.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

<span data-ttu-id="7052f-283">La `updateStockPrice` función da formato a un objeto de stock recibido `init` del servidor en una fila de tabla de la misma manera que en la función.</span><span class="sxs-lookup"><span data-stu-id="7052f-283">The `updateStockPrice` function formats a stock object received from the server into a table row the same way as in the `init` function.</span></span> <span data-ttu-id="7052f-284">En lugar de anexar la fila a la tabla, busca la fila actual del stock en la tabla y reemplaza esa fila por la nueva.</span><span class="sxs-lookup"><span data-stu-id="7052f-284">Instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

## <a name="test-the-application"></a><span data-ttu-id="7052f-285">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="7052f-285">Test the application</span></span>

<span data-ttu-id="7052f-286">Puedeprobar la aplicación para asegurarse de que funciona.</span><span class="sxs-lookup"><span data-stu-id="7052f-286">You can test the app to make sure it's working.</span></span> <span data-ttu-id="7052f-287">Verá que todas las ventanas del navegador muestran la tabla de acciones en vivo con los precios de las acciones fluctuando.</span><span class="sxs-lookup"><span data-stu-id="7052f-287">You'll see all browser windows display the live stock table with stock prices fluctuating.</span></span>

1. <span data-ttu-id="7052f-288">En la barra de herramientas, active **Depuración** de scripts y, a continuación, seleccione el botón de reproducción para ejecutar la aplicación en modo Depurar.</span><span class="sxs-lookup"><span data-stu-id="7052f-288">In the toolbar, turn on **Script Debugging** and then select the play button to run the app in Debug mode.</span></span>

    ![Captura de pantalla del usuario activando el modo de depuración y seleccionando play.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    <span data-ttu-id="7052f-290">Se abrirá una ventana del navegador que muestra la **Tabla de acciones**en vivo .</span><span class="sxs-lookup"><span data-stu-id="7052f-290">A browser window will open displaying the **Live Stock Table**.</span></span> <span data-ttu-id="7052f-291">La tabla de valores muestra inicialmente la "carga..." línea, luego, después de un corto tiempo, la aplicación muestra los datos de stock iniciales y, a continuación, los precios de las acciones comienzan a cambiar.</span><span class="sxs-lookup"><span data-stu-id="7052f-291">The stock table initially shows the "loading..." line, then, after a short time, the app shows the initial stock data, and then the stock prices start to change.</span></span>

1. <span data-ttu-id="7052f-292">Copie la dirección URL del explorador, abra otros dos exploradores y pegue las direcciones URL en las barras de direcciones.</span><span class="sxs-lookup"><span data-stu-id="7052f-292">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

    <span data-ttu-id="7052f-293">La visualización inicial del stock es la misma que la del primer navegador y los cambios se producen simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="7052f-293">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>

1. <span data-ttu-id="7052f-294">Cierre todos los navegadores, abra un nuevo navegador y vaya a la misma URL.</span><span class="sxs-lookup"><span data-stu-id="7052f-294">Close all browsers, open a new browser, and go to the same URL.</span></span>

    <span data-ttu-id="7052f-295">El objeto singleton StockTicker continuó ejecutándose en el servidor.</span><span class="sxs-lookup"><span data-stu-id="7052f-295">The StockTicker singleton object continued to run in the server.</span></span> <span data-ttu-id="7052f-296">La **Tabla de Acciones En Vivo** muestra que las acciones han seguido cambiando.</span><span class="sxs-lookup"><span data-stu-id="7052f-296">The **Live Stock Table** shows that the stocks have continued to change.</span></span> <span data-ttu-id="7052f-297">No verá la tabla inicial con cifras de cambio cero.</span><span class="sxs-lookup"><span data-stu-id="7052f-297">You don't see the initial table with zero change figures.</span></span>

1. <span data-ttu-id="7052f-298">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="7052f-298">Close the browser.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="7052f-299">Habilitación del registro</span><span class="sxs-lookup"><span data-stu-id="7052f-299">Enable logging</span></span>

<span data-ttu-id="7052f-300">SignalR tiene una función de registro integrada que puede habilitar en el cliente para ayudar en la solución de problemas.</span><span class="sxs-lookup"><span data-stu-id="7052f-300">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="7052f-301">En esta sección, habilitará el registro y verá ejemplos que muestran cómo los registros le indican cuál de los siguientes métodos de transporte utiliza SignalR:</span><span class="sxs-lookup"><span data-stu-id="7052f-301">In this section, you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

* <span data-ttu-id="7052f-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), compatible con IIS 8 y los exploradores actuales.</span><span class="sxs-lookup"><span data-stu-id="7052f-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>

* <span data-ttu-id="7052f-303">[Eventos enviados por](http://en.wikipedia.org/wiki/Server-sent_events)el servidor , compatibles con exploradores distintos de Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="7052f-303">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>

* <span data-ttu-id="7052f-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), compatible con Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="7052f-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>

* <span data-ttu-id="7052f-305">[Ajax sondeo largo](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), compatible con todos los navegadores.</span><span class="sxs-lookup"><span data-stu-id="7052f-305">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="7052f-306">Para cualquier conexión dada, SignalR elige el mejor método de transporte que tanto el servidor como el cliente admiten.</span><span class="sxs-lookup"><span data-stu-id="7052f-306">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="7052f-307">Abra *StockTicker.js*.</span><span class="sxs-lookup"><span data-stu-id="7052f-307">Open *StockTicker.js*.</span></span>

1. <span data-ttu-id="7052f-308">Agregue esta línea de código resaltada para habilitar el registro inmediatamente antes del código que inicializa la conexión al final del archivo:</span><span class="sxs-lookup"><span data-stu-id="7052f-308">Add this highlighted line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. <span data-ttu-id="7052f-309">Presione **F5** para ejecutar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7052f-309">Press **F5** to run the project.</span></span>

1. <span data-ttu-id="7052f-310">Abra la ventana de herramientas para desarrolladores del explorador y seleccione la consola para ver los registros.</span><span class="sxs-lookup"><span data-stu-id="7052f-310">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="7052f-311">Es posible que tenga que actualizar la página para ver los registros de SignalR negociando el método de transporte para una nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="7052f-311">You might have to refresh the page to see the logs of SignalR negotiating the transport method for a new connection.</span></span>

    * <span data-ttu-id="7052f-312">Si está ejecutando Internet Explorer 10 en Windows 8 (IIS 8), el método de transporte es **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="7052f-312">If you're running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

    * <span data-ttu-id="7052f-313">Si está ejecutando Internet Explorer 10 en Windows 7 (IIS 7.5), el método de transporte es **iframe**.</span><span class="sxs-lookup"><span data-stu-id="7052f-313">If you're running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is **iframe**.</span></span>

    * <span data-ttu-id="7052f-314">Si está ejecutando Firefox 19 en Windows 8 (IIS 8), el método de transporte es **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="7052f-314">If you're running Firefox 19 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

        > [!TIP]
        > <span data-ttu-id="7052f-315">En Firefox, instala el complemento Firebug para obtener una ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="7052f-315">In Firefox, install the Firebug add-in to get a Console window.</span></span>

    * <span data-ttu-id="7052f-316">Si está ejecutando Firefox 19 en Windows 7 (IIS 7.5), el método de transporte es eventos **enviados por el servidor.**</span><span class="sxs-lookup"><span data-stu-id="7052f-316">If you're running Firefox 19 on Windows 7 (IIS 7.5), the transport method is **server-sent** events.</span></span>

## <a name="install-the-stockticker-sample"></a><span data-ttu-id="7052f-317">Instale el ejemplo StockTicker</span><span class="sxs-lookup"><span data-stu-id="7052f-317">Install the StockTicker sample</span></span>

<span data-ttu-id="7052f-318">[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) instala la aplicación StockTicker.</span><span class="sxs-lookup"><span data-stu-id="7052f-318">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installs the StockTicker application.</span></span> <span data-ttu-id="7052f-319">El paquete NuGet incluye más características que la versión simplificada que creó desde cero.</span><span class="sxs-lookup"><span data-stu-id="7052f-319">The NuGet package includes more features than the simplified version that you created from scratch.</span></span> <span data-ttu-id="7052f-320">En esta sección del tutorial, instalará el paquete NuGet y revisará las nuevas características y el código que las implementa.</span><span class="sxs-lookup"><span data-stu-id="7052f-320">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7052f-321">Si instala el paquete sin realizar los pasos anteriores de este tutorial, debe agregar una clase de inicio OWIN al proyecto.</span><span class="sxs-lookup"><span data-stu-id="7052f-321">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="7052f-322">Este archivo readme.txt para el paquete NuGet explica este paso.</span><span class="sxs-lookup"><span data-stu-id="7052f-322">This readme.txt file for the NuGet package explains this step.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="7052f-323">Instale el paquete NuGet SignalR.Sample</span><span class="sxs-lookup"><span data-stu-id="7052f-323">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="7052f-324">En **el Explorador**de soluciones , haga clic con el botón secundario en el proyecto y seleccione Administrar **paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7052f-324">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="7052f-325">En **el Administrador de paquetes NuGet: SignalR.StockTicker**, seleccione **Examinar**.</span><span class="sxs-lookup"><span data-stu-id="7052f-325">In **NuGet Package manager: SignalR.StockTicker**, select **Browse**.</span></span>

1. <span data-ttu-id="7052f-326">En **Origen del paquete**, seleccione **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="7052f-326">From **Package source**, select **nuget.org**.</span></span>

1. <span data-ttu-id="7052f-327">Escriba *SignalR.Sample* en el cuadro de búsqueda y seleccione **Microsoft.AspNet.SignalR.Sample** > **Install**.</span><span class="sxs-lookup"><span data-stu-id="7052f-327">Enter *SignalR.Sample* in the search box and select **Microsoft.AspNet.SignalR.Sample** > **Install**.</span></span>

1. <span data-ttu-id="7052f-328">En el **Explorador**de soluciones , expanda la carpeta *SignalR.Sample.*</span><span class="sxs-lookup"><span data-stu-id="7052f-328">In **Solution Explorer**, expand the *SignalR.Sample* folder.</span></span>

    <span data-ttu-id="7052f-329">La instalación del paquete SignalR.Sample creó la carpeta y su contenido.</span><span class="sxs-lookup"><span data-stu-id="7052f-329">Installing the SignalR.Sample package created the folder and its contents.</span></span>

1. <span data-ttu-id="7052f-330">En la carpeta *SignalR.Sample* , haga clic con el botón derecho en *StockTicker.html*y, a continuación, seleccione **Establecer como página**de inicio .</span><span class="sxs-lookup"><span data-stu-id="7052f-330">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then select **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7052f-331">La instalación del paquete NuGet SignalR.Sample podría cambiar la versión de jQuery que tiene en la carpeta *Scripts.*</span><span class="sxs-lookup"><span data-stu-id="7052f-331">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="7052f-332">El nuevo archivo *StockTicker.html* que instala el paquete en la carpeta *SignalR.Sample* estará sincronizado con la versión de jQuery que instala el paquete, pero si desea volver a ejecutar el archivo *StockTicker.html* original, es posible que primero tenga que actualizar la referencia de jQuery en la etiqueta de script.</span><span class="sxs-lookup"><span data-stu-id="7052f-332">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="7052f-333">Ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="7052f-333">Run the application</span></span>

 <span data-ttu-id="7052f-334">La tabla que vio en la primera aplicación tenía características útiles.</span><span class="sxs-lookup"><span data-stu-id="7052f-334">The table that you saw in the first app had useful features.</span></span> <span data-ttu-id="7052f-335">La aplicación de ticker de stock completo muestra nuevas características: una ventana de desplazamiento horizontal que muestra los datos de stock y las existencias que cambian de color a medida que se elevan y bajan.</span><span class="sxs-lookup"><span data-stu-id="7052f-335">The full stock ticker application shows new features: a horizontally scrolling window that shows the stock data and stocks that change color as they rise and fall.</span></span>

1. <span data-ttu-id="7052f-336">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7052f-336">Press **F5** to run the app.</span></span>

     <span data-ttu-id="7052f-337">Cuando ejecuta sin tientas la aplicación por primera vez, el "mercado" se "cierra" y ves una tabla estática y una ventana de ticker que no se está desplazando.</span><span class="sxs-lookup"><span data-stu-id="7052f-337">When you run the app for the first time, the "market" is "closed" and you see a static table and a ticker window that isn't scrolling.</span></span>

1. <span data-ttu-id="7052f-338">Seleccione **Mercado abierto**.</span><span class="sxs-lookup"><span data-stu-id="7052f-338">Select **Open Market**.</span></span>

    ![Captura de pantalla del ticker en vivo.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * <span data-ttu-id="7052f-340">La casilla **Live Stock Ticker** comienza a desplazarse horizontalmente, y el servidor comienza a transmitir periódicamente los cambios en el precio de las acciones de forma aleatoria.</span><span class="sxs-lookup"><span data-stu-id="7052f-340">The **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span>

    * <span data-ttu-id="7052f-341">Cada vez que cambia el precio de una acción, la aplicación actualiza tanto la tabla de acciones en **vivo** como el **Live Stock Ticker**.</span><span class="sxs-lookup"><span data-stu-id="7052f-341">Each time a stock price changes, the app updates both the **Live Stock Table** and the **Live Stock Ticker**.</span></span>

    * <span data-ttu-id="7052f-342">Cuando el cambio de precio de una acción es positivo, la aplicación muestra la acción con un fondo verde.</span><span class="sxs-lookup"><span data-stu-id="7052f-342">When a stock's price change is positive, the app shows the stock with a green background.</span></span>

    * <span data-ttu-id="7052f-343">Cuando el cambio es negativo, la aplicación muestra el stock con un fondo rojo.</span><span class="sxs-lookup"><span data-stu-id="7052f-343">When the change is negative, the app shows the stock with a red background.</span></span>

1. <span data-ttu-id="7052f-344">Seleccione **Cerrar mercado**.</span><span class="sxs-lookup"><span data-stu-id="7052f-344">Select **Close Market**.</span></span>

    * <span data-ttu-id="7052f-345">Las actualizaciones de la tabla se detienen.</span><span class="sxs-lookup"><span data-stu-id="7052f-345">The table updates stop.</span></span>

    * <span data-ttu-id="7052f-346">El ticker deja de desplazarse.</span><span class="sxs-lookup"><span data-stu-id="7052f-346">The ticker stops scrolling.</span></span>

1. <span data-ttu-id="7052f-347">Seleccione **Restablecer**.</span><span class="sxs-lookup"><span data-stu-id="7052f-347">Select **Reset**.</span></span>

    * <span data-ttu-id="7052f-348">Se restablecen todos los datos de stock.</span><span class="sxs-lookup"><span data-stu-id="7052f-348">All stock data is reset.</span></span>

    * <span data-ttu-id="7052f-349">La aplicación restaura el estado inicial antes de que se inicien los cambios de precio.</span><span class="sxs-lookup"><span data-stu-id="7052f-349">The app restores the initial state before price changes started.</span></span>

1. <span data-ttu-id="7052f-350">Copie la dirección URL del explorador, abra otros dos exploradores y pegue las direcciones URL en las barras de direcciones.</span><span class="sxs-lookup"><span data-stu-id="7052f-350">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="7052f-351">Verá los mismos datos actualizados dinámicamente al mismo tiempo en cada navegador.</span><span class="sxs-lookup"><span data-stu-id="7052f-351">You see the same data dynamically updated at the same time in each browser.</span></span>

1. <span data-ttu-id="7052f-352">Al seleccionar cualquiera de los controles, todos los navegadores responden de la misma manera al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="7052f-352">When you select any of the controls, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="7052f-353">Pantalla Live Stock Ticker</span><span class="sxs-lookup"><span data-stu-id="7052f-353">Live Stock Ticker display</span></span>

<span data-ttu-id="7052f-354">La pantalla **Live Stock Ticker** es `<div>` una lista desordenada en un elemento formateado en una sola línea por estilos CSS.</span><span class="sxs-lookup"><span data-stu-id="7052f-354">The **Live Stock Ticker** display is an unordered list in a `<div>` element formatted into a single line by CSS styles.</span></span> <span data-ttu-id="7052f-355">La aplicación inicializa y actualiza el ticker de la misma manera `<li>` que la tabla: reemplazando marcadores de posición en una cadena de plantilla y agregando dinámicamente los `<li>` elementos al `<ul>` elemento.</span><span class="sxs-lookup"><span data-stu-id="7052f-355">The app initializes and updates the ticker the same way as the table: by replacing placeholders in an `<li>` template string and dynamically adding the `<li>` elements to the `<ul>` element.</span></span> <span data-ttu-id="7052f-356">La aplicación incluye el desplazamiento `animate` mediante la función jQuery para variar `<div>`el margen izquierdo de la lista desordenada dentro de la .</span><span class="sxs-lookup"><span data-stu-id="7052f-356">The app includes  scrolling by using the jQuery `animate` function to vary the margin-left of the unordered list within the `<div>`.</span></span>

#### <a name="signalrsample-stocktickerhtml"></a><span data-ttu-id="7052f-357">SignalR.Sample StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="7052f-357">SignalR.Sample StockTicker.html</span></span>

<span data-ttu-id="7052f-358">El código HTML del ticker de stock:</span><span class="sxs-lookup"><span data-stu-id="7052f-358">The stock ticker HTML code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a><span data-ttu-id="7052f-359">SignalR.Sample StockTicker.css</span><span class="sxs-lookup"><span data-stu-id="7052f-359">SignalR.Sample StockTicker.css</span></span>

<span data-ttu-id="7052f-360">El código CSS del ticker de stock:</span><span class="sxs-lookup"><span data-stu-id="7052f-360">The stock ticker CSS code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a><span data-ttu-id="7052f-361">SignalR.Sample SignalR.StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="7052f-361">SignalR.Sample SignalR.StockTicker.js</span></span>

<span data-ttu-id="7052f-362">El código jQuery que lo hace desplazarse:</span><span class="sxs-lookup"><span data-stu-id="7052f-362">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="7052f-363">Métodos adicionales en el servidor que el cliente puede llamar</span><span class="sxs-lookup"><span data-stu-id="7052f-363">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="7052f-364">Para agregar flexibilidad a la aplicación, hay métodos adicionales a los que la aplicación puede llamar.</span><span class="sxs-lookup"><span data-stu-id="7052f-364">To add flexibility to the app, there are additional methods the app can call.</span></span>

#### <a name="signalrsample-stocktickerhubcs"></a><span data-ttu-id="7052f-365">SignalR.Sample StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="7052f-365">SignalR.Sample StockTickerHub.cs</span></span>

<span data-ttu-id="7052f-366">La `StockTickerHub` clase define cuatro métodos adicionales a los que el cliente puede llamar:</span><span class="sxs-lookup"><span data-stu-id="7052f-366">The `StockTickerHub` class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="7052f-367">La aplicación `OpenMarket` `CloseMarket`llama `Reset` , , y en respuesta a los botones en la parte superior de la página.</span><span class="sxs-lookup"><span data-stu-id="7052f-367">The app calls `OpenMarket`, `CloseMarket`, and `Reset` in response to the buttons at the top of the page.</span></span> <span data-ttu-id="7052f-368">Muestran el patrón de un cliente que desencadena un cambio de estado que se propaga inmediatamente a todos los clientes.</span><span class="sxs-lookup"><span data-stu-id="7052f-368">They demonstrate the pattern of one client triggering a change in state immediately propagated to all clients.</span></span> <span data-ttu-id="7052f-369">Cada uno de estos métodos llama a un método en la `StockTicker` clase que provoca el cambio de estado de mercado y, a continuación, difunde el nuevo estado.</span><span class="sxs-lookup"><span data-stu-id="7052f-369">Each of these methods calls a method in the `StockTicker` class that causes the market state change and then broadcasts the new state.</span></span>

#### <a name="signalrsample-stocktickercs"></a><span data-ttu-id="7052f-370">SignalR.Sample StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="7052f-370">SignalR.Sample StockTicker.cs</span></span>

<span data-ttu-id="7052f-371">En `StockTicker` la clase, la aplicación mantiene el `MarketState` estado del `MarketState` mercado con una propiedad que devuelve un valor de enumeración:</span><span class="sxs-lookup"><span data-stu-id="7052f-371">In the `StockTicker` class, the app maintains the state of the market with a `MarketState` property that returns a `MarketState` enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="7052f-372">Cada uno de los métodos que cambian el `StockTicker` estado del mercado lo hace dentro de un bloque de bloqueo porque la clase tiene que ser segura para subprocesos:</span><span class="sxs-lookup"><span data-stu-id="7052f-372">Each of the methods that change the market state do so inside a lock block because the `StockTicker` class has to be thread-safe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="7052f-373">Para asegurarse de que este `_marketState` código es seguro `MarketState` para `volatile`subprocesos, el campo que respalda la propiedad designada:</span><span class="sxs-lookup"><span data-stu-id="7052f-373">To make sure this code is thread-safe, the `_marketState` field that backs the `MarketState` property designated `volatile`:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="7052f-374">Los `BroadcastMarketStateChange` `BroadcastMarketReset` métodos y son similares al método BroadcastStockPrice que ya ha visto, excepto que llaman a diferentes métodos definidos en el cliente:</span><span class="sxs-lookup"><span data-stu-id="7052f-374">The `BroadcastMarketStateChange` and `BroadcastMarketReset` methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="7052f-375">Funciones adicionales en el cliente a las que el servidor puede llamar</span><span class="sxs-lookup"><span data-stu-id="7052f-375">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="7052f-376">La `updateStockPrice` función ahora maneja tanto la tabla como `jQuery.Color` la pantalla del ticker, y se utiliza para parpadear los colores rojo y verde.</span><span class="sxs-lookup"><span data-stu-id="7052f-376">The `updateStockPrice` function now handles both the table and the ticker display, and it uses `jQuery.Color` to flash red and green colors.</span></span>

<span data-ttu-id="7052f-377">Las nuevas funciones de *SignalR.StockTicker.js* habilitan y desactivan los botones en función del estado del mercado.</span><span class="sxs-lookup"><span data-stu-id="7052f-377">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state.</span></span> <span data-ttu-id="7052f-378">También detienen o inician el desplazamiento horizontal de **Live Stock Ticker.**</span><span class="sxs-lookup"><span data-stu-id="7052f-378">They also stop or start the **Live Stock Ticker** horizontal scrolling.</span></span> <span data-ttu-id="7052f-379">Dado que se agregan `ticker.client`muchas funciones a , la aplicación utiliza la [función de extensión jQuery](http://api.jquery.com/jQuery.extend/) para agregarlas.</span><span class="sxs-lookup"><span data-stu-id="7052f-379">Since many functions are being added to `ticker.client`, the app uses the [jQuery extend function](http://api.jquery.com/jQuery.extend/) to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="7052f-380">Configuración adicional del cliente después de establecer la conexión</span><span class="sxs-lookup"><span data-stu-id="7052f-380">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="7052f-381">Después de que el cliente establezca la conexión, tiene algo de trabajo adicional que hacer:</span><span class="sxs-lookup"><span data-stu-id="7052f-381">After the client establishes the connection, it has some additional work to do:</span></span>

* <span data-ttu-id="7052f-382">Averiguar si el mercado está abierto o `marketOpened` `marketClosed` cerrado para llamar a la adecuada o función.</span><span class="sxs-lookup"><span data-stu-id="7052f-382">Find out if the market is open or closed to call the appropriate `marketOpened` or `marketClosed` function.</span></span>

* <span data-ttu-id="7052f-383">Asocie las llamadas al método de servidor a los botones.</span><span class="sxs-lookup"><span data-stu-id="7052f-383">Attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="7052f-384">Los métodos de servidor no se conectan a los botones hasta después de que la aplicación establezca la conexión.</span><span class="sxs-lookup"><span data-stu-id="7052f-384">The server methods aren't wired up to the buttons until after the app establishes the connection.</span></span> <span data-ttu-id="7052f-385">Es para que el código no pueda llamar a los métodos de servidor antes de que estén disponibles.</span><span class="sxs-lookup"><span data-stu-id="7052f-385">It's so the code can't call the server methods before they're available.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7052f-386">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7052f-386">Additional resources</span></span>

<span data-ttu-id="7052f-387">En este tutorial ha aprendido a programar una aplicación de SignalR que difunde mensajes desde el servidor a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="7052f-387">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients.</span></span> <span data-ttu-id="7052f-388">Ahora puede transmitir mensajes de forma periódica y en respuesta a notificaciones de cualquier cliente.</span><span class="sxs-lookup"><span data-stu-id="7052f-388">Now you can broadcast messages on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="7052f-389">Puede utilizar el concepto de instancia singleton multiproceso para mantener el estado del servidor en escenarios de juego en línea multijugador.</span><span class="sxs-lookup"><span data-stu-id="7052f-389">You can use the concept of multi-threaded singleton instance to maintain server state in multi-player online game scenarios.</span></span> <span data-ttu-id="7052f-390">Para ver un ejemplo, consulta [el juego ShootR basado en SignalR](https://github.com/NTaylorMullen/ShootR).</span><span class="sxs-lookup"><span data-stu-id="7052f-390">For an example, see [the ShootR game based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="7052f-391">Para ver tutoriales que muestran escenarios de comunicación punto a punto, consulte [Introducción a SignalR](introduction-to-signalr.md) y [Actualización en tiempo real con SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="7052f-391">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="7052f-392">Para obtener más información acerca de SignalR, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="7052f-392">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="7052f-393">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="7052f-393">ASP.NET SignalR</span></span>](../../index.md)
* [<span data-ttu-id="7052f-394">Proyecto SignalR</span><span class="sxs-lookup"><span data-stu-id="7052f-394">SignalR Project</span></span>](http://signalr.net/)
* [<span data-ttu-id="7052f-395">SignalR GitHub y muestras</span><span class="sxs-lookup"><span data-stu-id="7052f-395">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)
* [<span data-ttu-id="7052f-396">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="7052f-396">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="7052f-397">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="7052f-397">Next steps</span></span>

<span data-ttu-id="7052f-398">En este tutorial, hizo lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7052f-398">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7052f-399">Creado el proyecto</span><span class="sxs-lookup"><span data-stu-id="7052f-399">Created the project</span></span>
> * <span data-ttu-id="7052f-400">Configuración del código de servidor</span><span class="sxs-lookup"><span data-stu-id="7052f-400">Set up the server code</span></span>
> * <span data-ttu-id="7052f-401">Examinó el código del servidor</span><span class="sxs-lookup"><span data-stu-id="7052f-401">Examined the server code</span></span>
> * <span data-ttu-id="7052f-402">Configurar el código de cliente</span><span class="sxs-lookup"><span data-stu-id="7052f-402">Set up the client code</span></span>
> * <span data-ttu-id="7052f-403">Examinó el código de cliente</span><span class="sxs-lookup"><span data-stu-id="7052f-403">Examined the client code</span></span>
> * <span data-ttu-id="7052f-404">Probar la aplicación</span><span class="sxs-lookup"><span data-stu-id="7052f-404">Tested the application</span></span>
> * <span data-ttu-id="7052f-405">Registro habilitado</span><span class="sxs-lookup"><span data-stu-id="7052f-405">Enabled logging</span></span>

<span data-ttu-id="7052f-406">Avance al siguiente artículo para aprender a crear una aplicación web en tiempo real que use ASP.NET SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="7052f-406">Advance to the next article to learn how to create a real-time web application that uses ASP.NET SignalR 2.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="7052f-407">Cree una aplicación web en tiempo real con SignalR</span><span class="sxs-lookup"><span data-stu-id="7052f-407">Create real-time web app with SignalR</span></span>](real-time-web-applications-with-signalr.md)
