---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Crear páginas de ayuda para ASP.NET Web API - ASP.NET 4.x
author: MikeWasson
description: Este tutorial con el código muestra cómo crear páginas de ayuda para ASP.NET Web API en ASP.NET 4.x.
ms.author: riande
ms.date: 04/01/2013
ms.custom: seoapril2019
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 8308dab8bd66aa8f5a3c5fb4133fc7a3df78f671
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65125241"
---
# <a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="aa0ff-103">Crear páginas de ayuda para ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="aa0ff-103">Creating Help Pages for ASP.NET Web API</span></span>

<span data-ttu-id="aa0ff-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="aa0ff-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="aa0ff-105">Este tutorial con el código muestra cómo crear páginas de ayuda para ASP.NET Web API en ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-105">This tutorial with code shows how to create help pages for ASP.NET Web API in ASP.NET 4.x.</span></span>

<span data-ttu-id="aa0ff-106">Cuando se crea una API web, a menudo es útil crear una página de ayuda, para que otros desarrolladores sepan cómo llamar a la API.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-106">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="aa0ff-107">Puede crear toda la documentación de forma manual, pero es mejor para generar automáticamente tanto como sea posible.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-107">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span> <span data-ttu-id="aa0ff-108">Para facilitar esta tarea, ASP.NET Web API proporciona una biblioteca para generar automáticamente las páginas de ayuda en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-108">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="aa0ff-109">Crear páginas de Ayuda de API</span><span class="sxs-lookup"><span data-stu-id="aa0ff-109">Creating API Help Pages</span></span>

<span data-ttu-id="aa0ff-110">Instalar [ASP.NET y Web Tools 2012.2 actualización](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="aa0ff-110">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="aa0ff-111">Esta actualización integra páginas de ayuda en la plantilla de proyecto Web API.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-111">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="aa0ff-112">A continuación, cree un nuevo proyecto de ASP.NET MVC 4 y seleccione la plantilla de proyecto Web API.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-112">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="aa0ff-113">La plantilla de proyecto crea un controlador de API de ejemplo denominado `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-113">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="aa0ff-114">La plantilla también crea las páginas de Ayuda de API.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-114">The template also creates the API help pages.</span></span> <span data-ttu-id="aa0ff-115">Todos los archivos de código para la página de ayuda se colocan en la carpeta áreas del proyecto.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-115">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="aa0ff-116">Al ejecutar la aplicación, la página principal contiene un vínculo a la página de Ayuda de API.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-116">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="aa0ff-117">En la página principal, la ruta de acceso relativa es Help.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-117">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="aa0ff-118">Este vínculo le lleva a una página de resumen de API.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-118">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="aa0ff-119">La vista MVC de esta página se define en Areas/HelpPage/Views/Help/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-119">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="aa0ff-120">Puede editar esta página para modificar el diseño, introducción, título, estilos y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-120">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="aa0ff-121">La parte principal de la página es una tabla de API, agrupadas por controlador.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-121">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="aa0ff-122">Las entradas de tabla se generan de forma dinámica, mediante el **IApiExplorer** interfaz.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-122">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="aa0ff-123">(Hablaré más sobre esta interfaz más adelante.) Si agrega un nuevo controlador de API, la tabla se actualiza automáticamente en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-123">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="aa0ff-124">La columna "API" muestra el método HTTP y el URI relativo.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-124">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="aa0ff-125">La columna "Description" contiene documentación para cada API.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-125">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="aa0ff-126">Inicialmente, la documentación es simplemente texto de marcador de posición.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-126">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="aa0ff-127">En la sección siguiente, mostraré cómo agregar la documentación de los comentarios XML.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-127">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="aa0ff-128">Cada API tiene un vínculo a una página con información más detallada, incluidos los cuerpos de solicitud y respuesta de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-128">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="aa0ff-129">Adición de páginas de ayuda a un proyecto existente</span><span class="sxs-lookup"><span data-stu-id="aa0ff-129">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="aa0ff-130">Puede agregar páginas de ayuda a un proyecto Web API existente mediante el uso de administrador de paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-130">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="aa0ff-131">Esta opción es útil que empezar con una plantilla de proyecto diferente de la plantilla "API Web".</span><span class="sxs-lookup"><span data-stu-id="aa0ff-131">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="aa0ff-132">Desde el **herramientas** menú, seleccione **Administrador de paquetes de NuGet**y, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-132">From the **Tools** menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="aa0ff-133">En el [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ventana, escriba uno de los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="aa0ff-133">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="aa0ff-134">Para un **C#** aplicación: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="aa0ff-134">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="aa0ff-135">Para un **Visual Basic** aplicación: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="aa0ff-135">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="aa0ff-136">Hay dos paquetes, uno para C# y otro para Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-136">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="aa0ff-137">Asegúrese de usar que coincida con el proyecto.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-137">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="aa0ff-138">Este comando instala a los ensamblados necesarios y agrega las vistas MVC para las páginas de ayuda (ubicadas en la carpeta áreas/HelpPage).</span><span class="sxs-lookup"><span data-stu-id="aa0ff-138">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="aa0ff-139">Deberá agregar manualmente un vínculo a la página de ayuda.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-139">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="aa0ff-140">El URI es/Help.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-140">The URI is /Help.</span></span> <span data-ttu-id="aa0ff-141">Para crear un vínculo en una vista de razor, agregue lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="aa0ff-141">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="aa0ff-142">Además, asegúrese de registrar las áreas.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-142">Also, make sure to register areas.</span></span> <span data-ttu-id="aa0ff-143">En el archivo Global.asax, agregue el código siguiente a la **aplicación\_iniciar** método, si aún no está allí:</span><span class="sxs-lookup"><span data-stu-id="aa0ff-143">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="aa0ff-144">Adición de documentación de API</span><span class="sxs-lookup"><span data-stu-id="aa0ff-144">Adding API Documentation</span></span>

<span data-ttu-id="aa0ff-145">De forma predeterminada, la Ayuda de las páginas tienen cadenas de marcador de posición para la documentación.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-145">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="aa0ff-146">Puede usar [comentarios de documentación XML](https://msdn.microsoft.com/library/b2s063f7.aspx) para crear la documentación.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-146">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="aa0ff-147">Para habilitar esta característica, abra el archivo áreas/aplicación/HelpPage\_Start/HelpPageConfig.cs y elimine la línea siguiente:</span><span class="sxs-lookup"><span data-stu-id="aa0ff-147">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="aa0ff-148">Ahora, habilite la documentación XML.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-148">Now enable XML documentation.</span></span> <span data-ttu-id="aa0ff-149">En el Explorador de soluciones, haga clic en el proyecto y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-149">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="aa0ff-150">Seleccione el **compilar** página.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-150">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="aa0ff-151">En **salida**, comprobar **archivo de documentación XML**.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-151">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="aa0ff-152">En el cuadro de edición, escriba "App\_Data/XmlDocument.xml".</span><span class="sxs-lookup"><span data-stu-id="aa0ff-152">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="aa0ff-153">A continuación, abra el código para el `ValuesController` controlador de API, que se define en /Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-153">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesController.cs.</span></span> <span data-ttu-id="aa0ff-154">Agregar algunos comentarios de documentación a los métodos del controlador.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-154">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="aa0ff-155">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="aa0ff-155">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="aa0ff-156">Sugerencia: Si se sitúe el símbolo de intercalación en la línea antes del método y escribe tres barras diagonales, Visual Studio inserta automáticamente los elementos XML.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-156">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="aa0ff-157">A continuación, puede rellenar los espacios en blanco.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-157">Then you can fill in the blanks.</span></span>

<span data-ttu-id="aa0ff-158">Ahora compilar y ejecutar la aplicación de nuevo y vaya a las páginas de ayuda.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-158">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="aa0ff-159">Las cadenas de documentación deben aparecer en la tabla de API.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-159">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="aa0ff-160">La página de ayuda lee las cadenas del archivo XML en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-160">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="aa0ff-161">(Cuando se implementa la aplicación, asegúrese de que implementar el archivo XML.)</span><span class="sxs-lookup"><span data-stu-id="aa0ff-161">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="aa0ff-162">En segundo plano</span><span class="sxs-lookup"><span data-stu-id="aa0ff-162">Under the Hood</span></span>

<span data-ttu-id="aa0ff-163">Se compilan las páginas de ayuda en la parte superior de la **ApiExplorer** (clase), que forma parte del marco Web API.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-163">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="aa0ff-164">El **ApiExplorer** clase proporciona la materia prima para la creación de una página de ayuda.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-164">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="aa0ff-165">Para cada API, **ApiExplorer** contiene un **ApiDescription** que describe la API.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-165">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="aa0ff-166">Para ello, se define una "API" como la combinación de método HTTP y el URI relativo.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-166">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="aa0ff-167">Por ejemplo, estas son algunas API distintas:</span><span class="sxs-lookup"><span data-stu-id="aa0ff-167">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="aa0ff-168">OBTENER /api/Products</span><span class="sxs-lookup"><span data-stu-id="aa0ff-168">GET /api/Products</span></span>
- <span data-ttu-id="aa0ff-169">GET /api/Products/{id}</span><span class="sxs-lookup"><span data-stu-id="aa0ff-169">GET /api/Products/{id}</span></span>
- <span data-ttu-id="aa0ff-170">REGISTRAR/api/productos</span><span class="sxs-lookup"><span data-stu-id="aa0ff-170">POST /api/Products</span></span>

<span data-ttu-id="aa0ff-171">Si una acción de controlador es compatible con varios métodos HTTP, el **ApiExplorer** trata cada método como una API diferente.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-171">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="aa0ff-172">Para ocultar una API desde el **ApiExplorer**, agregar el **ApiExplorerSettings** a la acción y establezca el atributo *IgnoreApi* en true.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-172">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="aa0ff-173">También puede agregar este atributo al controlador, para excluir todo el controlador.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-173">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="aa0ff-174">La clase ApiExplorer Obtiene las cadenas de documentación de la **IDocumentationProvider** interfaz.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-174">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="aa0ff-175">Como vimos anteriormente, la biblioteca de páginas de ayuda se proporciona un **IDocumentationProvider** que obtiene la documentación de las cadenas de documentación XML.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-175">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="aa0ff-176">El código se encuentra en /Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-176">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="aa0ff-177">Puede obtener documentación de otro origen escribiendo su propio **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-177">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="aa0ff-178">Para asociarlo, llame a la **SetDocumentationProvider** método de extensión, definido en **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="aa0ff-178">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="aa0ff-179">**ApiExplorer** llama automáticamente a la **IDocumentationProvider** interfaz para obtener las cadenas de documentación para cada API.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-179">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="aa0ff-180">Almacena en el **documentación** propiedad de la **ApiDescription** y **ApiParameterDescription** objetos.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-180">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aa0ff-181">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="aa0ff-181">Next Steps</span></span>

<span data-ttu-id="aa0ff-182">No se limita a las páginas de ayuda que se muestra aquí.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-182">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="aa0ff-183">De hecho, **ApiExplorer** no se limita a la creación de páginas de ayuda.</span><span class="sxs-lookup"><span data-stu-id="aa0ff-183">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="aa0ff-184">Yao Huang Lin ha escrito que algunos estupendo blog publica para ayudarlo a pensar en fábrica:</span><span class="sxs-lookup"><span data-stu-id="aa0ff-184">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="aa0ff-185">Agregar a un cliente de prueba simple a la página de Ayuda de ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="aa0ff-185">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="aa0ff-186">Que funcione en los servicios alojados en sí mismos la página de Ayuda de ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="aa0ff-186">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="aa0ff-187">Generación de tiempo de diseño de página de ayuda (o cliente) para ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="aa0ff-187">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="aa0ff-188">Personalizaciones avanzadas de página de ayuda</span><span class="sxs-lookup"><span data-stu-id="aa0ff-188">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
