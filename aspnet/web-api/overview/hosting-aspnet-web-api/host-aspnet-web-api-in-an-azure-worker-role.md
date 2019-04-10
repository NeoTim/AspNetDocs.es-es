---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Hospedar ASP.NET Web API 2 en un rol de trabajo de Azure - ASP.NET 4.x
author: MikeWasson
description: 'Tutorial: Hospedar ASP.NET Web API en un rol de trabajador de Azure, usar OWIN para probar internamente el marco API Web.'
ms.author: riande
ms.date: 04/02/2014
ms.custom: seoapril2019
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: bfb23aafb814010e8651965dad91ca20a37fd786
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59404629"
---
# <a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="bc9b2-103">Hospedar ASP.NET Web API 2 en un rol de trabajo de Azure</span><span class="sxs-lookup"><span data-stu-id="bc9b2-103">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>

<span data-ttu-id="bc9b2-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bc9b2-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="bc9b2-105">Este tutorial muestra cómo hospedar ASP.NET Web API en un rol de trabajador de Azure, mediante OWIN para probar internamente el marco API Web.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-105">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="bc9b2-106">[Interfaz Web abierta para .NET](http://owin.org/) (OWIN) define una abstracción entre los servidores web de .NET y aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-106">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="bc9b2-107">OWIN desacopla la aplicación web desde el servidor, lo que hace que OWIN ideal para el autohospedaje de una aplicación web en su propio proceso, fuera de IIS: por ejemplo, dentro de un rol de trabajo de Azure.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-107">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
>
> <span data-ttu-id="bc9b2-108">En este tutorial, usará el paquete Microsoft.Owin.Host.HttpListener, que proporciona un servidor HTTP que se utiliza para autohospedar aplicaciones OWIN.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-108">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bc9b2-109">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="bc9b2-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="bc9b2-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="bc9b2-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="bc9b2-111">Web API 2</span><span class="sxs-lookup"><span data-stu-id="bc9b2-111">Web API 2</span></span>
> - [<span data-ttu-id="bc9b2-112">Azure SDK para .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="bc9b2-112">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="bc9b2-113">Crear un proyecto de Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="bc9b2-113">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="bc9b2-114">Inicie Visual Studio con privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-114">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="bc9b2-115">Se necesitan privilegios de administrador para depurar la aplicación localmente, mediante el emulador de proceso de Azure.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-115">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="bc9b2-116">En el **archivo** menú, haga clic en **New**, a continuación, haga clic en **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-116">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="bc9b2-117">Desde **plantillas instaladas**, bajo Visual C#, haga clic en **en la nube** y, a continuación, haga clic en **Windows Azure Cloud Service**.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-117">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="bc9b2-118">Denomine el proyecto "AzureApp" y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-118">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="bc9b2-119">En el **nuevo servicio de nube de Windows Azure** cuadro de diálogo, haga doble clic en **rol de trabajo**.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-119">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="bc9b2-120">Deje el nombre predeterminado ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="bc9b2-120">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="bc9b2-121">Este paso agrega un rol de trabajo a la solución.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-121">This step adds a worker role to the solution.</span></span> <span data-ttu-id="bc9b2-122">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-122">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="bc9b2-123">La solución de Visual Studio que se crea contiene dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="bc9b2-123">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="bc9b2-124">&quot;AzureApp&quot; define los roles y la configuración de la aplicación de Azure.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-124">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="bc9b2-125">&quot;WorkerRole1&quot; contiene el código para el rol de trabajo.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-125">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="bc9b2-126">En general, una aplicación de Azure puede contener varios roles, aunque este tutorial utiliza una única función.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-126">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="bc9b2-127">Agregar la API Web y paquetes OWIN</span><span class="sxs-lookup"><span data-stu-id="bc9b2-127">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="bc9b2-128">Desde el **herramientas** menú, haga clic en **Administrador de paquetes de NuGet**, a continuación, haga clic en **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-128">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="bc9b2-129">En la ventana de consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="bc9b2-129">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="bc9b2-130">Agregar un extremo HTTP</span><span class="sxs-lookup"><span data-stu-id="bc9b2-130">Add an HTTP Endpoint</span></span>

<span data-ttu-id="bc9b2-131">En el Explorador de soluciones, expanda el proyecto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-131">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="bc9b2-132">Expanda el nodo Roles, haga clic en WorkerRole1 y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-132">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="bc9b2-133">Haga clic en **extremos**y, a continuación, haga clic en **agregar punto de conexión**.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-133">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="bc9b2-134">En el **protocolo** lista desplegable, seleccione "http".</span><span class="sxs-lookup"><span data-stu-id="bc9b2-134">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="bc9b2-135">En **puerto público** y **puerto privado**, escriba 80.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-135">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="bc9b2-136">Estos números de puerto pueden ser diferentes.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-136">These port numbers can be different.</span></span> <span data-ttu-id="bc9b2-137">El puerto público es lo que los clientes usan cuando envía una solicitud a la función.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-137">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="bc9b2-138">Configuración de Web API para autohospedar</span><span class="sxs-lookup"><span data-stu-id="bc9b2-138">Configure Web API for Self-Host</span></span>

<span data-ttu-id="bc9b2-139">En el Explorador de soluciones, haga clic en el proyecto WorkerRole1 y seleccione **agregar** / **clase** para agregar una nueva clase.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-139">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="bc9b2-140">Asigne a la clase el nombre `Startup`.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-140">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="bc9b2-141">Reemplace todo el código repetitivo en este archivo por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc9b2-141">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="bc9b2-142">Agregar un controlador de API Web</span><span class="sxs-lookup"><span data-stu-id="bc9b2-142">Add a Web API Controller</span></span>

<span data-ttu-id="bc9b2-143">A continuación, agregue una clase de controlador Web API.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-143">Next, add a Web API controller class.</span></span> <span data-ttu-id="bc9b2-144">Haga clic en el proyecto WorkerRole1 y seleccione **agregar** / **clase**.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-144">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="bc9b2-145">Nombre de la clase TestController.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-145">Name the class TestController.</span></span> <span data-ttu-id="bc9b2-146">Reemplace todo el código repetitivo en este archivo por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc9b2-146">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="bc9b2-147">Por motivos de simplicidad, este controlador solo define dos métodos GET que devuelven texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-147">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="bc9b2-148">Iniciar el Host OWIN</span><span class="sxs-lookup"><span data-stu-id="bc9b2-148">Start the OWIN Host</span></span>

<span data-ttu-id="bc9b2-149">Abra el archivo WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-149">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="bc9b2-150">Esta clase define el código que se ejecuta cuando se inicia y detiene el rol de trabajo.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-150">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="bc9b2-151">Agregue la siguiente instrucción using:</span><span class="sxs-lookup"><span data-stu-id="bc9b2-151">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="bc9b2-152">Agregar un **IDisposable** miembro a la `WorkerRole` clase:</span><span class="sxs-lookup"><span data-stu-id="bc9b2-152">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="bc9b2-153">En el `OnStart` método, agregue el código siguiente para iniciar el host:</span><span class="sxs-lookup"><span data-stu-id="bc9b2-153">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="bc9b2-154">El **WebApp.Start** método inicia el host OWIN.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-154">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="bc9b2-155">El nombre de la `Startup` clase es un parámetro de tipo para el método.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-155">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="bc9b2-156">Por convención, el host llamará el `Configure` método de esta clase.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-156">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="bc9b2-157">Invalidar el `OnStop` para desechar el  *\_aplicación* instancia:</span><span class="sxs-lookup"><span data-stu-id="bc9b2-157">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="bc9b2-158">Este es el código completo de WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="bc9b2-158">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="bc9b2-159">Compile la solución y presione F5 para ejecutar la aplicación localmente en el emulador de proceso de Azure.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-159">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="bc9b2-160">Dependiendo de la configuración del firewall debe permitir que el emulador a través del firewall.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-160">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="bc9b2-161">Si se produce una excepción similar al siguiente, vea [esta entrada de blog](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) para encontrar una solución.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-161">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="bc9b2-162">"No se pudo cargar el archivo o ensamblado ' Microsoft.Owin, Version = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' o uno de sus dependencias.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-162">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="bc9b2-163">Definición del manifiesto de ensamblado encontrada no coincide con la referencia de ensamblado.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-163">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="bc9b2-164">(Exception from HRESULT: 0x80131040)"</span><span class="sxs-lookup"><span data-stu-id="bc9b2-164">(Exception from HRESULT: 0x80131040)"</span></span>


<span data-ttu-id="bc9b2-165">El emulador de proceso asigna una dirección IP local para el punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-165">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="bc9b2-166">Puede encontrar la dirección IP mediante la visualización de la interfaz de usuario del emulador de proceso.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-166">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="bc9b2-167">Haga clic en el icono del emulador en la barra del área de notificación de tareas y seleccione **Mostrar IU del emulador de proceso**.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-167">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="bc9b2-168">Buscar la dirección IP en implementaciones de servicios de implementación [id], detalles del servicio.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-168">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="bc9b2-169">Abra un explorador web y vaya a http://<em>dirección</em>/test/1, donde <em>dirección</em> es la dirección IP asignada por el emulador de proceso; por ejemplo, `http://127.0.0.1:80/test/1`.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-169">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="bc9b2-170">Debería ver la respuesta desde el controlador de API Web:</span><span class="sxs-lookup"><span data-stu-id="bc9b2-170">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="bc9b2-171">Implementar en Azure</span><span class="sxs-lookup"><span data-stu-id="bc9b2-171">Deploy to Azure</span></span>

<span data-ttu-id="bc9b2-172">Este paso, debe tener una cuenta de Azure.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-172">For this step, you must have an Azure account.</span></span> <span data-ttu-id="bc9b2-173">Si aún no tiene uno, puede crear una cuenta de evaluación gratuita en tan solo unos minutos.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-173">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="bc9b2-174">Para obtener más información, consulte [evaluación gratuita de Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="bc9b2-174">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="bc9b2-175">En el Explorador de soluciones, haga clic en el proyecto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-175">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="bc9b2-176">Seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-176">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="bc9b2-177">Si no inició sesión en su cuenta de Azure, haga clic en **sesión**.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-177">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="bc9b2-178">Una vez que haya iniciado sesión, elija una suscripción y haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-178">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="bc9b2-179">Escriba un nombre para el servicio en la nube y elija una región.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-179">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="bc9b2-180">Haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-180">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="bc9b2-181">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-181">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="bc9b2-182">La ventana de registro de actividad de Azure muestra el progreso de la implementación.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-182">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="bc9b2-183">Cuando se implementa la aplicación, vaya a http://appname.cloudapp.net/test/1.</span><span class="sxs-lookup"><span data-stu-id="bc9b2-183">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="bc9b2-184">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="bc9b2-184">Additional Resources</span></span>

- [<span data-ttu-id="bc9b2-185">Información general del proyecto Katana</span><span class="sxs-lookup"><span data-stu-id="bc9b2-185">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="bc9b2-186">Proyecto Katana en GitHub</span><span class="sxs-lookup"><span data-stu-id="bc9b2-186">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
