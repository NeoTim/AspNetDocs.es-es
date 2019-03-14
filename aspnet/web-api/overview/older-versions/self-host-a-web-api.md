---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Autohospedar ASP.NET Web API 1 (C#) | Microsoft Docs
author: MikeWasson
description: ASP.NET Web API no requiere IIS. Puede probarlo internamente una API web en su propio proceso de host. Este tutorial muestra cómo hospedar una API web dentro de una aplicación de consola...
ms.author: riande
ms.date: 01/26/2012
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 63d192a6fa2aafef3770d5b0b97ec32e001b69db
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57040762"
---
<a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="b1545-105">Autohospedar ASP.NET Web API 1 (C#)</span><span class="sxs-lookup"><span data-stu-id="b1545-105">Self-Host ASP.NET Web API 1 (C#)</span></span>
====================
<span data-ttu-id="b1545-106">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b1545-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="b1545-107">ASP.NET Web API no requiere IIS.</span><span class="sxs-lookup"><span data-stu-id="b1545-107">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="b1545-108">Puede probarlo internamente una API web en su propio proceso de host.</span><span class="sxs-lookup"><span data-stu-id="b1545-108">You can self-host a web API in your own host process.</span></span> <span data-ttu-id="b1545-109">Este tutorial muestra cómo hospedar una API web dentro de una aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="b1545-109">This tutorial shows how to host a web API inside a console application.</span></span>
> 
> <span data-ttu-id="b1545-110">**Las nuevas aplicaciones deben usar OWIN para autohospedaje API Web.**</span><span class="sxs-lookup"><span data-stu-id="b1545-110">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="b1545-111">Consulte [Use OWIN para autohospedaje de ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="b1545-111">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b1545-112">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="b1545-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b1545-113">Web API 1</span><span class="sxs-lookup"><span data-stu-id="b1545-113">Web API 1</span></span>
> - <span data-ttu-id="b1545-114">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="b1545-114">Visual Studio 2012</span></span>


## <a name="create-the-console-application-project"></a><span data-ttu-id="b1545-115">Crear el proyecto de aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="b1545-115">Create the Console Application Project</span></span>

<span data-ttu-id="b1545-116">Inicie Visual Studio y seleccione **nuevo proyecto** desde el **iniciar** página.</span><span class="sxs-lookup"><span data-stu-id="b1545-116">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="b1545-117">O bien, en el **archivo** menú, seleccione **New** y, a continuación, **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="b1545-117">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="b1545-118">En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo.</span><span class="sxs-lookup"><span data-stu-id="b1545-118">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="b1545-119">En **Visual C#**, seleccione **Windows**.</span><span class="sxs-lookup"><span data-stu-id="b1545-119">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="b1545-120">En la lista de plantillas de proyecto, seleccione **aplicación de consola**.</span><span class="sxs-lookup"><span data-stu-id="b1545-120">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="b1545-121">Denomine el proyecto &quot;SelfHost&quot; y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="b1545-121">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="b1545-122">Establezca la plataforma de destino (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="b1545-122">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="b1545-123">Si utiliza Visual Studio 2010, cambie la plataforma de destino a .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="b1545-123">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="b1545-124">(De forma predeterminada, los destinos de la plantilla de proyecto la [perfil de .net Framework Client](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span><span class="sxs-lookup"><span data-stu-id="b1545-124">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="b1545-125">En el Explorador de soluciones, haga clic en el proyecto y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="b1545-125">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="b1545-126">En el **.NET framework de destino** lista desplegable Mostrar, cambiar la plataforma de destino a .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="b1545-126">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="b1545-127">Cuando se le pida para aplicar el cambio, haga clic en **Sí**.</span><span class="sxs-lookup"><span data-stu-id="b1545-127">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="b1545-128">Instalar a Administrador de paquetes de NuGet</span><span class="sxs-lookup"><span data-stu-id="b1545-128">Install NuGet Package Manager</span></span>

<span data-ttu-id="b1545-129">El Administrador de paquetes de NuGet es la manera más fácil de agregar los ensamblados de la API Web a un proyecto que no sean ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b1545-129">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="b1545-130">Para comprobar si está instalado el Administrador de paquetes de NuGet, haga clic en el **herramientas** menú en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b1545-130">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="b1545-131">Si ve un elemento de menú denominado **Administrador de paquetes de NuGet**, tendrá que Administrador de paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="b1545-131">If you see a menu item called **NuGet Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="b1545-132">Para instalar el Administrador de paquetes de NuGet:</span><span class="sxs-lookup"><span data-stu-id="b1545-132">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="b1545-133">Inicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b1545-133">Start Visual Studio.</span></span>
2. <span data-ttu-id="b1545-134">Desde el **herramientas** menú, seleccione **extensiones y actualizaciones**.</span><span class="sxs-lookup"><span data-stu-id="b1545-134">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="b1545-135">En el **extensiones y actualizaciones** cuadro de diálogo, seleccione **Online**.</span><span class="sxs-lookup"><span data-stu-id="b1545-135">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="b1545-136">Si no ve "Administrador de paquetes de NuGet", escriba "Administrador de paquetes de nuget" en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="b1545-136">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="b1545-137">Seleccione el Administrador de paquetes de NuGet y haga clic en **descargar**.</span><span class="sxs-lookup"><span data-stu-id="b1545-137">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="b1545-138">Una vez finalizada la descarga, se le pedirá que instale.</span><span class="sxs-lookup"><span data-stu-id="b1545-138">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="b1545-139">Una vez finalizada la instalación, se le podrían reiniciar Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b1545-139">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="b1545-140">Agregue el paquete de API NuGet Web</span><span class="sxs-lookup"><span data-stu-id="b1545-140">Add the Web API NuGet Package</span></span>

<span data-ttu-id="b1545-141">Después de instalar Administrador de paquetes de NuGet, agregue el paquete de Web API Self al proyecto.</span><span class="sxs-lookup"><span data-stu-id="b1545-141">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="b1545-142">Desde el **herramientas** menú, seleccione **Administrador de paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b1545-142">From the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="b1545-143">*Nota*: Si no se ve este menú de elemento, asegúrese de que ese administrador de paquetes de NuGet instalado correctamente.</span><span class="sxs-lookup"><span data-stu-id="b1545-143">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="b1545-144">Seleccione **administrar paquetes de NuGet para la solución**</span><span class="sxs-lookup"><span data-stu-id="b1545-144">Select **Manage NuGet Packages for Solution**</span></span>
3. <span data-ttu-id="b1545-145">En el **administrar paquetes de NuGet** cuadro de diálogo, seleccione **Online**.</span><span class="sxs-lookup"><span data-stu-id="b1545-145">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="b1545-146">En el cuadro de búsqueda, escriba &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="b1545-146">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="b1545-147">Seleccione el paquete de Host automático de ASP.NET Web API y haga clic en **instalar**.</span><span class="sxs-lookup"><span data-stu-id="b1545-147">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="b1545-148">Una vez instalado el paquete, haga clic en **cerrar** para cerrar el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="b1545-148">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="b1545-149">Asegúrese de instalar el paquete denominado Microsoft.AspNet.WebApi.SelfHost, no AspNetWebApi.SelfHost.</span><span class="sxs-lookup"><span data-stu-id="b1545-149">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="b1545-150">Crear el modelo y controlador</span><span class="sxs-lookup"><span data-stu-id="b1545-150">Create the Model and Controller</span></span>

<span data-ttu-id="b1545-151">Este tutorial usa las mismas clases de modelo y controlador como el [Introducción](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="b1545-151">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="b1545-152">Agregue una clase pública denominada `Product`.</span><span class="sxs-lookup"><span data-stu-id="b1545-152">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="b1545-153">Agregue una clase pública denominada `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="b1545-153">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="b1545-154">Derive esta clase de **System.Web.Http.ApiController**.</span><span class="sxs-lookup"><span data-stu-id="b1545-154">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="b1545-155">Para obtener más información sobre el código de este controlador, consulte el [Introducción](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="b1545-155">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="b1545-156">Este controlador define tres acciones GET:</span><span class="sxs-lookup"><span data-stu-id="b1545-156">This controller defines three GET actions:</span></span>

| <span data-ttu-id="b1545-157">Identificador URI</span><span class="sxs-lookup"><span data-stu-id="b1545-157">URI</span></span> | <span data-ttu-id="b1545-158">Descripción</span><span class="sxs-lookup"><span data-stu-id="b1545-158">Description</span></span> |
| --- | --- |
| <span data-ttu-id="b1545-159">productos/api /</span><span class="sxs-lookup"><span data-stu-id="b1545-159">/api/products</span></span> | <span data-ttu-id="b1545-160">Obtener una lista de todos los productos.</span><span class="sxs-lookup"><span data-stu-id="b1545-160">Get a list of all products.</span></span> |
| <span data-ttu-id="b1545-161">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="b1545-161">/api/products/*id*</span></span> | <span data-ttu-id="b1545-162">Obtener un producto por identificador.</span><span class="sxs-lookup"><span data-stu-id="b1545-162">Get a product by ID.</span></span> |
| <span data-ttu-id="b1545-163">/api/products/?category=*category*</span><span class="sxs-lookup"><span data-stu-id="b1545-163">/api/products/?category=*category*</span></span> | <span data-ttu-id="b1545-164">Obtener una lista de productos por categoría.</span><span class="sxs-lookup"><span data-stu-id="b1545-164">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="b1545-165">Hospedar la API Web</span><span class="sxs-lookup"><span data-stu-id="b1545-165">Host the Web API</span></span>

<span data-ttu-id="b1545-166">Abra el archivo Program.cs y agregue las siguientes instrucciones using:</span><span class="sxs-lookup"><span data-stu-id="b1545-166">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="b1545-167">Agregue el código siguiente a la **programa** clase.</span><span class="sxs-lookup"><span data-stu-id="b1545-167">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="b1545-168">(Opcional) Agregar una reserva de Namespace de direcciones URL de HTTP</span><span class="sxs-lookup"><span data-stu-id="b1545-168">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="b1545-169">Esta aplicación escucha a `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="b1545-169">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="b1545-170">De forma predeterminada, escucha en una dirección HTTP determinada requiere privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="b1545-170">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="b1545-171">Al ejecutar el tutorial, por lo tanto, puede obtener este error: "HTTP no pudo registrar la dirección URL http://+:8080/" hay dos formas de evitar este error:</span><span class="sxs-lookup"><span data-stu-id="b1545-171">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="b1545-172">Ejecutar Visual Studio con permisos de administrador con privilegios elevados, o</span><span class="sxs-lookup"><span data-stu-id="b1545-172">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="b1545-173">Utilice Netsh.exe para conceder los permisos de cuenta para reservar la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="b1545-173">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="b1545-174">Para usar Netsh.exe, abra un símbolo del sistema con privilegios de administrador y escriba el comando siguiente: comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="b1545-174">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="b1545-175">donde *máquina\nombredeusuario* es su cuenta de usuario.</span><span class="sxs-lookup"><span data-stu-id="b1545-175">where *machine\username* is your user account.</span></span>

<span data-ttu-id="b1545-176">Cuando haya terminado de autohospedaje, asegúrese de eliminar la reserva:</span><span class="sxs-lookup"><span data-stu-id="b1545-176">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="b1545-177">Llame a la API Web desde una aplicación cliente (C#)</span><span class="sxs-lookup"><span data-stu-id="b1545-177">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="b1545-178">Vamos a escribir una aplicación de consola simple que llama a la API web.</span><span class="sxs-lookup"><span data-stu-id="b1545-178">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="b1545-179">Agregue un nuevo proyecto de aplicación de consola a la solución:</span><span class="sxs-lookup"><span data-stu-id="b1545-179">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="b1545-180">En el Explorador de soluciones, haga clic en la solución y seleccione **Agregar nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="b1545-180">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="b1545-181">Crear una nueva aplicación de consola denominada &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="b1545-181">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="b1545-182">Use el Administrador de paquetes de NuGet para agregar el paquete de bibliotecas de núcleo de ASP.NET Web API:</span><span class="sxs-lookup"><span data-stu-id="b1545-182">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="b1545-183">En el menú Herramientas, seleccione **Administrador de paquetes de NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b1545-183">From the Tools menu, select **NuGet Package Manager**.</span></span>
- <span data-ttu-id="b1545-184">Seleccione **administrar paquetes de NuGet para la solución**</span><span class="sxs-lookup"><span data-stu-id="b1545-184">Select **Manage NuGet Packages for Solution**</span></span>
- <span data-ttu-id="b1545-185">En el **administrar paquetes de NuGet** cuadro de diálogo, seleccione **Online**.</span><span class="sxs-lookup"><span data-stu-id="b1545-185">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="b1545-186">En el cuadro de búsqueda, escriba &quot;Microsoft.AspNet.WebApi.Client&quot;.</span><span class="sxs-lookup"><span data-stu-id="b1545-186">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="b1545-187">Seleccione el paquete de bibliotecas de cliente de Microsoft ASP.NET Web API y haga clic en **instalar**.</span><span class="sxs-lookup"><span data-stu-id="b1545-187">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="b1545-188">Agregue una referencia en ClientApp al proyecto SelfHost:</span><span class="sxs-lookup"><span data-stu-id="b1545-188">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="b1545-189">En el Explorador de soluciones, haga clic en el proyecto ClientApp.</span><span class="sxs-lookup"><span data-stu-id="b1545-189">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="b1545-190">Seleccione **Agregar referencia**.</span><span class="sxs-lookup"><span data-stu-id="b1545-190">Select **Add Reference**.</span></span>
- <span data-ttu-id="b1545-191">En el **Administrador de referencias** cuadro de diálogo, en **solución**, seleccione **proyectos**.</span><span class="sxs-lookup"><span data-stu-id="b1545-191">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="b1545-192">Seleccione el proyecto SelfHost.</span><span class="sxs-lookup"><span data-stu-id="b1545-192">Select the SelfHost project.</span></span>
- <span data-ttu-id="b1545-193">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="b1545-193">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="b1545-194">Abra el archivo Client/Program.cs.</span><span class="sxs-lookup"><span data-stu-id="b1545-194">Open the Client/Program.cs file.</span></span> <span data-ttu-id="b1545-195">Agregue el siguiente **mediante** instrucción:</span><span class="sxs-lookup"><span data-stu-id="b1545-195">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="b1545-196">Agregar una variable static **HttpClient** instancia:</span><span class="sxs-lookup"><span data-stu-id="b1545-196">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="b1545-197">Agregue los métodos siguientes para obtener una lista de todos los productos, un producto por el identificador de la lista y lista de productos por categoría.</span><span class="sxs-lookup"><span data-stu-id="b1545-197">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="b1545-198">Cada uno de estos métodos sigue el mismo patrón:</span><span class="sxs-lookup"><span data-stu-id="b1545-198">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="b1545-199">Llame a **HttpClient.GetAsync** para enviar una solicitud GET al URI adecuado.</span><span class="sxs-lookup"><span data-stu-id="b1545-199">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="b1545-200">Llame a **HttpResponseMessage.EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="b1545-200">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="b1545-201">Este método produce una excepción si el estado de respuesta HTTP es un código de error.</span><span class="sxs-lookup"><span data-stu-id="b1545-201">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="b1545-202">Llame a **ReadAsAsync&lt;T&gt;**  para deserializar un tipo CLR de la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="b1545-202">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="b1545-203">Este método es un método de extensión definido en **System.Net.Http.HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="b1545-203">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="b1545-204">El **GetAsync** y **ReadAsAsync** métodos son asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="b1545-204">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="b1545-205">Devuelven **tarea** objetos que representan la operación asincrónica.</span><span class="sxs-lookup"><span data-stu-id="b1545-205">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="b1545-206">Obteniendo el **resultado** propiedad bloquea el subproceso hasta que se complete la operación.</span><span class="sxs-lookup"><span data-stu-id="b1545-206">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="b1545-207">Para obtener más información sobre el uso de HttpClient, incluida la forma de realizar llamadas sin bloqueo, consulte [una llamada a una Web API desde un cliente .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="b1545-207">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="b1545-208">Antes de llamar a estos métodos, establezca la propiedad BaseAddress en la instancia de HttpClient para "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="b1545-208">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="b1545-209">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b1545-209">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="b1545-210">Esto debería mostrar lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="b1545-210">This should output the following.</span></span> <span data-ttu-id="b1545-211">(Recuerde que se ejecute la aplicación SelfHost primero).</span><span class="sxs-lookup"><span data-stu-id="b1545-211">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
