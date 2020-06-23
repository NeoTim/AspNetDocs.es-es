---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Tutorial: autohospedaje de Signalr | Microsoft Docs'
author: bradygaster
description: En este tutorial se muestra cómo crear un servidor de Signalr con autohospedado y cómo conectarse a él con un cliente de JavaScript. Versiones de software usadas en el tutorial...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 7470e0d6b68772ccfd979c834b7db81fbba3ca78
ms.sourcegitcommit: 0cf7d06071a8ff986e6c028ac9daf0c0e7490412
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2020
ms.locfileid: "85240611"
---
# <a name="tutorial-signalr-self-host"></a><span data-ttu-id="ea4e0-104">Tutorial: Prueba interna de SignalR</span><span class="sxs-lookup"><span data-stu-id="ea4e0-104">Tutorial: SignalR Self-Host</span></span>

<span data-ttu-id="ea4e0-105">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="ea4e0-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="ea4e0-106">En este tutorial se muestra cómo crear un servidor de Signalr con autohospedado y cómo conectarse a él con un cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-106">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ea4e0-107">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="ea4e0-107">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="ea4e0-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="ea4e0-108">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="ea4e0-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="ea4e0-109">.NET 4.5</span></span>
> - <span data-ttu-id="ea4e0-110">Signalr versión 2</span><span class="sxs-lookup"><span data-stu-id="ea4e0-110">SignalR version 2</span></span>
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="ea4e0-111">Uso de Visual Studio 2012 con este tutorial</span><span class="sxs-lookup"><span data-stu-id="ea4e0-111">Using Visual Studio 2012 with this tutorial</span></span>
>
>
> <span data-ttu-id="ea4e0-112">Para usar Visual Studio 2012 con este tutorial, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea4e0-112">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
>
> - <span data-ttu-id="ea4e0-113">Actualice el [Administrador de paquetes](http://docs.nuget.org/docs/start-here/installing-nuget) a la versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-113">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="ea4e0-114">Instale el [instalador de plataforma web](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="ea4e0-114">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="ea4e0-115">En el instalador de plataforma web, busque e instale **ASP.NET and Web Tools 2013,1 para Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-115">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="ea4e0-116">Se instalarán las plantillas de Visual Studio para las clases de Signalr como **Hub**.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-116">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="ea4e0-117">Algunas plantillas (como la **clase de inicio OWIN**) no estarán disponibles; para ello, use un archivo de clase en su lugar.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-117">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="ea4e0-118">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="ea4e0-118">Questions and comments</span></span>
>
> <span data-ttu-id="ea4e0-119">Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="ea4e0-120">Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="ea4e0-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="ea4e0-121">Introducción</span><span class="sxs-lookup"><span data-stu-id="ea4e0-121">Overview</span></span>

<span data-ttu-id="ea4e0-122">Un servidor de Signalr se hospeda normalmente en una aplicación ASP.NET en IIS, pero también puede ser autohospedado (por ejemplo, en una aplicación de consola o un servicio de Windows) mediante la biblioteca de autohospedaje.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-122">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="ea4e0-123">Esta biblioteca, al igual que la de Signalr 2, se basa en OWIN ([Open web interface para .net](http://owin.org)).</span><span class="sxs-lookup"><span data-stu-id="ea4e0-123">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="ea4e0-124">OWIN define una abstracción entre servidores Web .NET y aplicaciones Web.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-124">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="ea4e0-125">OWIN desacopla la aplicación web del servidor, lo que hace que OWIN sea ideal para hospedar automáticamente una aplicación web en su propio proceso, fuera de IIS.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-125">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="ea4e0-126">Entre los motivos para no hospedar en IIS se incluyen:</span><span class="sxs-lookup"><span data-stu-id="ea4e0-126">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="ea4e0-127">Entornos en los que IIS no está disponible o deseable, como una granja de servidores existente sin IIS.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-127">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="ea4e0-128">La sobrecarga de rendimiento de IIS debe evitarse.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-128">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="ea4e0-129">La funcionalidad de signalr se agregará a una aplicación existente que se ejecuta en un servicio de Windows, un rol de trabajo de Azure u otro proceso.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-129">SignalR functionality is to be added to an existing application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="ea4e0-130">Si una solución se está desarrollando como autohospedada por motivos de rendimiento, se recomienda probar también la aplicación hospedada en IIS para determinar la ventaja de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-130">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="ea4e0-131">Este tutorial contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="ea4e0-131">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="ea4e0-132">Crear el servidor</span><span class="sxs-lookup"><span data-stu-id="ea4e0-132">Creating the server</span></span>](#server)
- [<span data-ttu-id="ea4e0-133">Acceso al servidor con un cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="ea4e0-133">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="ea4e0-134">Crear el servidor</span><span class="sxs-lookup"><span data-stu-id="ea4e0-134">Creating the server</span></span>

<span data-ttu-id="ea4e0-135">En este tutorial, creará un servidor que se hospeda en una aplicación de consola, pero el servidor se puede hospedar en cualquier tipo de proceso, como un servicio de Windows o un rol de trabajo de Azure.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-135">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="ea4e0-136">Para ver el código de ejemplo para hospedar un servidor de Signalr en un servicio de Windows, consulte [signalr Autohospedaje en un servicio de Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span><span class="sxs-lookup"><span data-stu-id="ea4e0-136">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="ea4e0-137">Abra Visual Studio 2013 con privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-137">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="ea4e0-138">Seleccione **archivo**, **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-138">Select **File**, **New Project**.</span></span> <span data-ttu-id="ea4e0-139">Seleccione **Windows** en el nodo **Visual C#** en el panel **plantillas** y seleccione la plantilla **aplicación de consola** .</span><span class="sxs-lookup"><span data-stu-id="ea4e0-139">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="ea4e0-140">Asigne al nuevo proyecto el nombre "SignalRSelfHost" y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-140">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="ea4e0-141">Abra la consola del administrador de paquetes Nuget; para ello, seleccione **herramientas**  >  **Administrador de paquetes Nuget**  >  **consola del administrador**de paquetes.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-141">Open the NuGet package manager console by selecting **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
3. <span data-ttu-id="ea4e0-142">En la consola del administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="ea4e0-142">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="ea4e0-143">Este comando agrega las bibliotecas de autohospedamiento Signalr 2 al proyecto.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-143">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="ea4e0-144">En la consola del administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="ea4e0-144">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="ea4e0-145">Este comando agrega la biblioteca Microsoft. Owin. CORS al proyecto.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-145">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="ea4e0-146">Esta biblioteca se usará para la compatibilidad entre dominios, que es necesaria para las aplicaciones que hospedan Signalr y un cliente de páginas web en dominios diferentes.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-146">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="ea4e0-147">Puesto que va a hospedar el servidor de Signalr y el cliente web en distintos puertos, esto significa que entre dominios debe estar habilitado para la comunicación entre estos componentes.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-147">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="ea4e0-148">Reemplace el contenido de Program.cs por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-148">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="ea4e0-149">El código anterior incluye tres clases:</span><span class="sxs-lookup"><span data-stu-id="ea4e0-149">The above code includes three classes:</span></span>

    - <span data-ttu-id="ea4e0-150">**Programa**, incluido el método **Main** que define la ruta de acceso principal de la ejecución.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-150">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="ea4e0-151">En este método, una aplicación Web de tipo **Startup** se inicia en la dirección URL especificada ( `http://localhost:8080` ).</span><span class="sxs-lookup"><span data-stu-id="ea4e0-151">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="ea4e0-152">Si se requiere seguridad en el punto de conexión, se puede implementar SSL.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-152">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="ea4e0-153">Consulte [Cómo: configurar un puerto con un certificado SSL](https://msdn.microsoft.com/library/ms733791.aspx) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-153">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="ea4e0-154">**Startup**, la clase que contiene la configuración para el servidor de signalr (la única configuración que usa este tutorial es la llamada a `UseCors` ) y la llamada a `MapSignalR` , que crea rutas para cualquier objeto de concentrador en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-154">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="ea4e0-155">**MyHub**, la clase de concentrador de signalr que la aplicación proporcionará a los clientes.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-155">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="ea4e0-156">Esta clase tiene un único método, **send**, al que los clientes llamarán para difundir un mensaje a todos los demás clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-156">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="ea4e0-157">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-157">Compile and run the application.</span></span> <span data-ttu-id="ea4e0-158">La dirección en la que se ejecuta el servidor debería mostrarse en una ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-158">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="ea4e0-159">Si se produce un error de ejecución con la excepción `System.Reflection.TargetInvocationException was unhandled` , deberá reiniciar Visual Studio con privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-159">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="ea4e0-160">Detenga la aplicación antes de continuar con la siguiente sección.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-160">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="ea4e0-161">Acceso al servidor con un cliente de JavaScript</span><span class="sxs-lookup"><span data-stu-id="ea4e0-161">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="ea4e0-162">En esta sección, usará el mismo cliente de JavaScript del tutorial de [Introducción](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="ea4e0-162">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="ea4e0-163">Solo realizaremos una modificación en el cliente, que es definir explícitamente la dirección URL del centro.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-163">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="ea4e0-164">Con una aplicación autohospedada, es posible que el servidor no esté necesariamente en la misma dirección que la dirección URL de conexión (debido a los servidores proxy inversos y los equilibradores de carga), por lo que la dirección URL debe definirse explícitamente.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-164">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="ea4e0-165">En **Explorador de soluciones**, haga clic con el botón derecho en la solución y seleccione **Agregar**, **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-165">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="ea4e0-166">Seleccione el nodo **Web** y seleccione la plantilla **aplicación Web ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="ea4e0-166">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="ea4e0-167">Asigne al proyecto el nombre "JavascriptClient" y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-167">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="ea4e0-168">Seleccione la plantilla **vacía** y deje las opciones restantes desactivadas.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-168">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="ea4e0-169">Seleccione **crear proyecto**.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-169">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="ea4e0-170">En la consola del administrador de paquetes, seleccione el proyecto "JavascriptClient" en la lista desplegable **proyecto predeterminado** y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="ea4e0-170">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="ea4e0-171">Este comando instala las bibliotecas Signalr y JQuery que necesitará en el cliente.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-171">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="ea4e0-172">Haga clic con el botón derecho en el proyecto y seleccione **Agregar**, **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-172">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="ea4e0-173">Seleccione el nodo **Web** y la página HTML.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-173">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="ea4e0-174">Asigne un nombre a la página **Default.html**.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-174">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="ea4e0-175">Reemplace el contenido de la nueva página HTML por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-175">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="ea4e0-176">Compruebe que las referencias del script aquí coinciden con los scripts de la carpeta scripts del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-176">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="ea4e0-177">El código siguiente (resaltado en el ejemplo de código anterior) es la adición que ha realizado al cliente usado en el tutorial de introducción (además de actualizar el código a Signalr versión 2 beta).</span><span class="sxs-lookup"><span data-stu-id="ea4e0-177">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="ea4e0-178">Esta línea de código establece explícitamente la dirección URL de conexión base para Signalr en el servidor.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-178">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="ea4e0-179">Haga clic con el botón derecho en la solución y seleccione **establecer proyectos de inicio..**.. Seleccione el botón de radio **proyectos de inicio múltiples** y establezca la **acción** de ambos proyectos en **iniciar**.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-179">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="ea4e0-180">Haga clic con el botón derecho en "Default.html" y seleccione **establecer como página de inicio**.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-180">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="ea4e0-181">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-181">Run the application.</span></span> <span data-ttu-id="ea4e0-182">Se iniciará el servidor y la página.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-182">The server and page will launch.</span></span> <span data-ttu-id="ea4e0-183">Es posible que tenga que volver a cargar la página web (o seleccionar **continuar** en el depurador) si la página se carga antes de que se inicie el servidor.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-183">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="ea4e0-184">En el explorador, proporcione un nombre de usuario cuando se le solicite.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-184">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="ea4e0-185">Copie la dirección URL de la página en otra pestaña o ventana del explorador y proporcione un nombre de usuario diferente.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-185">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="ea4e0-186">Podrá enviar mensajes desde un panel del explorador al otro, como en el tutorial de Introducción.</span><span class="sxs-lookup"><span data-stu-id="ea4e0-186">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
