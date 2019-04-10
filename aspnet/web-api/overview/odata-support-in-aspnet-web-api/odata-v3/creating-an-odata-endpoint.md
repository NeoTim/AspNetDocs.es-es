---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Creación de un punto de conexión de OData v3 con Web API 2 | Microsoft Docs
author: MikeWasson
description: Open Data Protocol (OData) es un protocolo de acceso a datos para la web. OData proporciona una manera uniforme para estructurar datos, consultar los datos y manipular los datos...
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: fa0573738fee8f1decc13c9797f644002931e09d
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59381502"
---
# <a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="5193d-104">Creación de un punto de conexión de OData v3 con Web API 2</span><span class="sxs-lookup"><span data-stu-id="5193d-104">Creating an OData v3 Endpoint with Web API 2</span></span>

<span data-ttu-id="5193d-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5193d-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="5193d-106">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="5193d-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="5193d-107">El [Open Data Protocol](http://www.odata.org/) (OData) es un protocolo de acceso a datos para la web.</span><span class="sxs-lookup"><span data-stu-id="5193d-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="5193d-108">OData proporciona una manera uniforme para estructurar datos, consultar los datos y manipular el conjunto de datos a través de las operaciones CRUD (crear, leer, actualizar y eliminar).</span><span class="sxs-lookup"><span data-stu-id="5193d-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="5193d-109">OData admite formatos JSON y AtomPub (XML).</span><span class="sxs-lookup"><span data-stu-id="5193d-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="5193d-110">OData también define un método para exponer metadatos sobre los datos.</span><span class="sxs-lookup"><span data-stu-id="5193d-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="5193d-111">Los clientes pueden usar los metadatos para detectar la información de tipos y relaciones para el conjunto de datos.</span><span class="sxs-lookup"><span data-stu-id="5193d-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
>
> <span data-ttu-id="5193d-112">ASP.NET Web API facilita la creación de un extremo de OData para un conjunto de datos.</span><span class="sxs-lookup"><span data-stu-id="5193d-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="5193d-113">Puede controlar exactamente qué operaciones de OData en el extremo admite.</span><span class="sxs-lookup"><span data-stu-id="5193d-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="5193d-114">Puede hospedar varios puntos de conexión de OData, junto con los puntos de conexión no OData.</span><span class="sxs-lookup"><span data-stu-id="5193d-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="5193d-115">Tiene control total sobre su modelo de datos, la lógica de negocios de back-end y la capa de datos.</span><span class="sxs-lookup"><span data-stu-id="5193d-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5193d-116">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="5193d-116">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="5193d-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5193d-117">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="5193d-118">Web API 2</span><span class="sxs-lookup"><span data-stu-id="5193d-118">Web API 2</span></span>
> - <span data-ttu-id="5193d-119">OData versión 3</span><span class="sxs-lookup"><span data-stu-id="5193d-119">OData Version 3</span></span>
> - <span data-ttu-id="5193d-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="5193d-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="5193d-121">(Opcional) de Proxy de depuración de Web de Fiddler</span><span class="sxs-lookup"><span data-stu-id="5193d-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
>
> <span data-ttu-id="5193d-122">Se agregó compatibilidad con la Web API OData en [ASP.NET y Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="5193d-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="5193d-123">Sin embargo, en este tutorial se usa scaffolding que se agregó en Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="5193d-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>


<span data-ttu-id="5193d-124">En este tutorial, creará un extremo OData simple que los clientes pueden consultar.</span><span class="sxs-lookup"><span data-stu-id="5193d-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="5193d-125">También creará a un cliente de C# para el punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="5193d-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="5193d-126">Después de completar este tutorial, el siguiente conjunto de tutoriales se muestra cómo agregar más funcionalidad, incluidas las relaciones de entidad, acciones, y seleccione Expandir $/ $.</span><span class="sxs-lookup"><span data-stu-id="5193d-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="5193d-127">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5193d-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="5193d-128">Agregar un modelo de entidades</span><span class="sxs-lookup"><span data-stu-id="5193d-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="5193d-129">Agregar un controlador de OData</span><span class="sxs-lookup"><span data-stu-id="5193d-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="5193d-130">Agregue el EDM y ruta</span><span class="sxs-lookup"><span data-stu-id="5193d-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="5193d-131">Valor de inicialización de la base de datos (opcional)</span><span class="sxs-lookup"><span data-stu-id="5193d-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="5193d-132">Explorar el extremo de OData</span><span class="sxs-lookup"><span data-stu-id="5193d-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="5193d-133">Formatos de serialización de OData</span><span class="sxs-lookup"><span data-stu-id="5193d-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="5193d-134">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5193d-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="5193d-135">En este tutorial, creará un extremo de OData que admite operaciones CRUD básicas.</span><span class="sxs-lookup"><span data-stu-id="5193d-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="5193d-136">El punto de conexión expondrá un único recurso, una lista de productos.</span><span class="sxs-lookup"><span data-stu-id="5193d-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="5193d-137">Los tutoriales posteriores agregarán más características.</span><span class="sxs-lookup"><span data-stu-id="5193d-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="5193d-138">Inicie Visual Studio y seleccione **nuevo proyecto** desde la página de inicio.</span><span class="sxs-lookup"><span data-stu-id="5193d-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="5193d-139">O bien, en el **archivo** menú, seleccione **New** y, a continuación, **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="5193d-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="5193d-140">En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el nodo Visual C#.</span><span class="sxs-lookup"><span data-stu-id="5193d-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="5193d-141">En **Visual C#**, seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="5193d-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="5193d-142">Seleccione **la aplicación Web ASP.NET** plantilla.</span><span class="sxs-lookup"><span data-stu-id="5193d-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="5193d-143">En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione el **vacía** plantilla.</span><span class="sxs-lookup"><span data-stu-id="5193d-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="5193d-144">Bajo &quot;agregar carpetas y referencias centrales para... &quot;, comprobar **API Web**.</span><span class="sxs-lookup"><span data-stu-id="5193d-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="5193d-145">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="5193d-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="5193d-146">Agregar un modelo de entidades</span><span class="sxs-lookup"><span data-stu-id="5193d-146">Add an Entity Model</span></span>

<span data-ttu-id="5193d-147">Un *modelo* es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5193d-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="5193d-148">Para este tutorial, se necesita un modelo que representa un producto.</span><span class="sxs-lookup"><span data-stu-id="5193d-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="5193d-149">El modelo corresponde a nuestro tipo de entidad de OData.</span><span class="sxs-lookup"><span data-stu-id="5193d-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="5193d-150">En el Explorador de soluciones, haga clic en la carpeta Models.</span><span class="sxs-lookup"><span data-stu-id="5193d-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="5193d-151">En el menú contextual, seleccione **agregar** , a continuación, seleccione **clase**.</span><span class="sxs-lookup"><span data-stu-id="5193d-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="5193d-152">En el **Agregar nuevo** elemento de cuadro de diálogo, asigne el nombre de la clase &quot;producto&quot;.</span><span class="sxs-lookup"><span data-stu-id="5193d-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="5193d-153">Por convención, las clases de modelo se colocan en la carpeta Models.</span><span class="sxs-lookup"><span data-stu-id="5193d-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="5193d-154">No tiene que seguir esta convención en sus propios proyectos, pero vamos a usar para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="5193d-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>


<span data-ttu-id="5193d-155">En el archivo Product.cs, agregue la siguiente definición de clase:</span><span class="sxs-lookup"><span data-stu-id="5193d-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="5193d-156">La propiedad de Id. será la clave de entidad.</span><span class="sxs-lookup"><span data-stu-id="5193d-156">The ID property will be the entity key.</span></span> <span data-ttu-id="5193d-157">Los clientes pueden consultar los productos por identificador.</span><span class="sxs-lookup"><span data-stu-id="5193d-157">Clients can query products by ID.</span></span> <span data-ttu-id="5193d-158">Este campo también sería la clave principal de la base de datos back-end.</span><span class="sxs-lookup"><span data-stu-id="5193d-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="5193d-159">Compile el proyecto ahora.</span><span class="sxs-lookup"><span data-stu-id="5193d-159">Build the project now.</span></span> <span data-ttu-id="5193d-160">En el paso siguiente, vamos a usar algunos scaffolding de Visual Studio que usa la reflexión para encontrar el tipo de producto.</span><span class="sxs-lookup"><span data-stu-id="5193d-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="5193d-161">Agregar un controlador de OData</span><span class="sxs-lookup"><span data-stu-id="5193d-161">Add an OData Controller</span></span>

<span data-ttu-id="5193d-162">Un *controlador* es una clase que controla las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="5193d-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="5193d-163">Definir un controlador independiente para cada conjunto de entidades en que el servicio de OData.</span><span class="sxs-lookup"><span data-stu-id="5193d-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="5193d-164">En este tutorial, vamos a crear un único controlador.</span><span class="sxs-lookup"><span data-stu-id="5193d-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="5193d-165">En el Explorador de soluciones, haga clic en la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="5193d-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="5193d-166">Seleccione **agregar** y, a continuación, seleccione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="5193d-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="5193d-167">En el **agregar Scaffold** cuadro de diálogo, seleccione &quot;controlador Web API 2 OData con acciones mediante Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="5193d-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="5193d-168">En el **Agregar controlador** cuadro de diálogo, el nombre del controlador "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="5193d-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="5193d-169">Seleccione el &quot;usar acciones de controlador asincrónicas&quot; casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="5193d-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="5193d-170">En el **modelo** lista desplegable, seleccione la clase de producto.</span><span class="sxs-lookup"><span data-stu-id="5193d-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="5193d-171">Haga clic en el **nuevo contexto de datos...**  botón.</span><span class="sxs-lookup"><span data-stu-id="5193d-171">Click the **New data context...** button.</span></span> <span data-ttu-id="5193d-172">Deje el nombre predeterminado para el tipo de contexto de datos y haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="5193d-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="5193d-173">Haga clic en Agregar en el cuadro de diálogo Agregar controlador para agregar el controlador.</span><span class="sxs-lookup"><span data-stu-id="5193d-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="5193d-174">Nota: Si recibe un mensaje de error que dice &quot;se produjo un error al obtener el tipo... &quot;, asegúrese de que ha generado el proyecto de Visual Studio después de agregar la clase de producto.</span><span class="sxs-lookup"><span data-stu-id="5193d-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="5193d-175">El scaffolding usa la reflexión para encontrar la clase.</span><span class="sxs-lookup"><span data-stu-id="5193d-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="5193d-176">El scaffolding agrega dos archivos de código al proyecto:</span><span class="sxs-lookup"><span data-stu-id="5193d-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="5193d-177">Products.cs define el controlador de API Web que implementa el extremo de OData.</span><span class="sxs-lookup"><span data-stu-id="5193d-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="5193d-178">ProductServiceContext.cs proporciona métodos para consultar la base de datos subyacente mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5193d-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="5193d-179">Agregue el EDM y ruta</span><span class="sxs-lookup"><span data-stu-id="5193d-179">Add the EDM and Route</span></span>

<span data-ttu-id="5193d-180">En el Explorador de soluciones, expanda la aplicación\_carpeta Inicio y abra el archivo WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="5193d-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="5193d-181">Esta clase contiene el código de configuración de Web API.</span><span class="sxs-lookup"><span data-stu-id="5193d-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="5193d-182">Reemplace este código con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5193d-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="5193d-183">Este código hace dos cosas:</span><span class="sxs-lookup"><span data-stu-id="5193d-183">This code does two things:</span></span>

- <span data-ttu-id="5193d-184">Crea un Entity Data Model (EDM) para el extremo de OData.</span><span class="sxs-lookup"><span data-stu-id="5193d-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="5193d-185">Agrega una ruta para el punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="5193d-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="5193d-186">Un EDM es un modelo abstracto de los datos.</span><span class="sxs-lookup"><span data-stu-id="5193d-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="5193d-187">El EDM se usa para crear el documento de metadatos y definir a los URI para el servicio.</span><span class="sxs-lookup"><span data-stu-id="5193d-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="5193d-188">El **ODataConventionModelBuilder** crea un modelo EDM utilizando un conjunto de convenciones de nomenclatura predeterminadas EDM.</span><span class="sxs-lookup"><span data-stu-id="5193d-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="5193d-189">Este enfoque requiere menos código.</span><span class="sxs-lookup"><span data-stu-id="5193d-189">This approach requires the least code.</span></span> <span data-ttu-id="5193d-190">Si desea más control sobre el modelo EDM, puede usar el **ODataModelBuilder** clase para crear el EDM agregando explícitamente las propiedades, claves y las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="5193d-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="5193d-191">El **EntitySet** método agrega un conjunto de entidades al EDM:</span><span class="sxs-lookup"><span data-stu-id="5193d-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="5193d-192">La cadena "Productos" define el nombre del conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="5193d-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="5193d-193">El nombre del controlador debe coincidir con el nombre del conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="5193d-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="5193d-194">En este tutorial, el conjunto de entidades se denomina "Productos" y el controlador se denomina `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="5193d-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="5193d-195">Si el nombre de conjunto de entidades "Conjunto de productos", podría denominar el controlador `ProductSetController`.</span><span class="sxs-lookup"><span data-stu-id="5193d-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="5193d-196">Tenga en cuenta que un punto de conexión puede tener varios conjuntos de entidades.</span><span class="sxs-lookup"><span data-stu-id="5193d-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="5193d-197">Llame a **EntitySet&lt;T&gt;**  para cada entidad establecer y, a continuación, defina un controlador correspondiente.</span><span class="sxs-lookup"><span data-stu-id="5193d-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="5193d-198">El **MapODataRoute** método agrega una ruta para el extremo de OData.</span><span class="sxs-lookup"><span data-stu-id="5193d-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="5193d-199">El primer parámetro es un nombre descriptivo para la ruta.</span><span class="sxs-lookup"><span data-stu-id="5193d-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="5193d-200">Los clientes del servicio no ven este nombre.</span><span class="sxs-lookup"><span data-stu-id="5193d-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="5193d-201">El segundo parámetro es el prefijo URI para el punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="5193d-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="5193d-202">Dado que este código, el URI para el conjunto de entidades de productos es http://<em>hostname</em>  /odata o productos.</span><span class="sxs-lookup"><span data-stu-id="5193d-202">Given this code, the URI for the Products entity set is http://<em>hostname</em>/odata/Products.</span></span> <span data-ttu-id="5193d-203">La aplicación puede tener más de un extremo de OData.</span><span class="sxs-lookup"><span data-stu-id="5193d-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="5193d-204">Para cada punto de conexión, llame a <strong>MapODataRoute</strong> y proporcione un nombre de ruta único y un prefijo de identificador URI único.</span><span class="sxs-lookup"><span data-stu-id="5193d-204">For each endpoint, call <strong>MapODataRoute</strong> and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="5193d-205">Valor de inicialización de la base de datos (opcional)</span><span class="sxs-lookup"><span data-stu-id="5193d-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="5193d-206">En este paso, usará Entity Framework para inicializar la base de datos con algunos datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="5193d-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="5193d-207">Este paso es opcional, pero le permite probar el punto de conexión de OData de inmediato.</span><span class="sxs-lookup"><span data-stu-id="5193d-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="5193d-208">Desde el **herramientas** menú, seleccione **Administrador de paquetes de NuGet**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="5193d-208">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="5193d-209">En la ventana de consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="5193d-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="5193d-210">Esto agrega una carpeta denominada migraciones y el archivo de código Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="5193d-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="5193d-211">Abra este archivo y agregue el código siguiente a la `Configuration.Seed` método.</span><span class="sxs-lookup"><span data-stu-id="5193d-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="5193d-212">En la ventana de consola de administrador de paquetes, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="5193d-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="5193d-213">Estos comandos generan código que crea la base de datos y, a continuación, ejecuta ese código.</span><span class="sxs-lookup"><span data-stu-id="5193d-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="5193d-214">Explorar el extremo de OData</span><span class="sxs-lookup"><span data-stu-id="5193d-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="5193d-215">En esta sección, vamos a usar el [Fiddler Web Debugging Proxy](http://www.fiddler2.com) para enviar solicitudes al punto de conexión y examinar los mensajes de respuesta.</span><span class="sxs-lookup"><span data-stu-id="5193d-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="5193d-216">Esto le ayudará a comprender las capacidades de un extremo de OData.</span><span class="sxs-lookup"><span data-stu-id="5193d-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="5193d-217">En Visual Studio, presione F5 para iniciar la depuración.</span><span class="sxs-lookup"><span data-stu-id="5193d-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="5193d-218">De forma predeterminada, Visual Studio abre el explorador en `http://localhost:*port*`, donde *puerto* es el número de puerto configurado en la configuración del proyecto.</span><span class="sxs-lookup"><span data-stu-id="5193d-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="5193d-219">Puede cambiar el número de puerto en la configuración del proyecto.</span><span class="sxs-lookup"><span data-stu-id="5193d-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="5193d-220">En el Explorador de soluciones, haga clic en el proyecto y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="5193d-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="5193d-221">En la ventana Propiedades, seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="5193d-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="5193d-222">Escriba el número de puerto en **dirección Url del proyecto**.</span><span class="sxs-lookup"><span data-stu-id="5193d-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="5193d-223">Documento de servicio</span><span class="sxs-lookup"><span data-stu-id="5193d-223">Service Document</span></span>

<span data-ttu-id="5193d-224">El *documento de servicio* contiene una lista de los conjuntos de entidades para el extremo de OData.</span><span class="sxs-lookup"><span data-stu-id="5193d-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="5193d-225">Para obtener el documento de servicio, envía una solicitud GET al URI del servicio en la raíz.</span><span class="sxs-lookup"><span data-stu-id="5193d-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="5193d-226">Uso de Fiddler, escriba el siguiente URI en el **Composer** ficha: `http://localhost:port/odata/`, donde *puerto* es el número de puerto.</span><span class="sxs-lookup"><span data-stu-id="5193d-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="5193d-227">Haga clic en el **Execute** botón.</span><span class="sxs-lookup"><span data-stu-id="5193d-227">Click the **Execute** button.</span></span> <span data-ttu-id="5193d-228">Fiddler envía una solicitud HTTP GET a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5193d-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="5193d-229">Debería ver la respuesta en la lista de sesiones de la Web.</span><span class="sxs-lookup"><span data-stu-id="5193d-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="5193d-230">Si todo funciona, el código de estado será de 200.</span><span class="sxs-lookup"><span data-stu-id="5193d-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="5193d-231">Haga doble clic en la respuesta de la lista de sesiones Web para ver los detalles del mensaje de respuesta en la pestaña inspectores.</span><span class="sxs-lookup"><span data-stu-id="5193d-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="5193d-232">El mensaje de respuesta HTTP sin formato debe ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="5193d-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="5193d-233">De forma predeterminada, API Web devuelve el documento de servicio en formato AtomPub.</span><span class="sxs-lookup"><span data-stu-id="5193d-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="5193d-234">Para solicitar JSON, agregue el siguiente encabezado a la solicitud HTTP:</span><span class="sxs-lookup"><span data-stu-id="5193d-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="5193d-235">Ahora, la respuesta HTTP contiene una carga JSON:</span><span class="sxs-lookup"><span data-stu-id="5193d-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="5193d-236">Documento de metadatos de servicio</span><span class="sxs-lookup"><span data-stu-id="5193d-236">Service Metadata Document</span></span>

<span data-ttu-id="5193d-237">El *documento de metadatos del servicio* describe el modelo de datos del servicio, mediante un lenguaje XML llamado el lenguaje de definición de esquemas conceptuales (CSDL).</span><span class="sxs-lookup"><span data-stu-id="5193d-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="5193d-238">El documento de metadatos muestra la estructura de los datos en el servicio y puede usarse para generar código de cliente.</span><span class="sxs-lookup"><span data-stu-id="5193d-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="5193d-239">Para obtener el documento de metadatos, envíe una solicitud GET a `http://localhost:port/odata/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="5193d-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="5193d-240">Estos los metadatos para el extremo que se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="5193d-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="5193d-241">Conjunto de entidades</span><span class="sxs-lookup"><span data-stu-id="5193d-241">Entity Set</span></span>

<span data-ttu-id="5193d-242">Para obtener el conjunto de entidades de productos, envíe una solicitud GET a `http://localhost:port/odata/Products`.</span><span class="sxs-lookup"><span data-stu-id="5193d-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="5193d-243">Entity</span><span class="sxs-lookup"><span data-stu-id="5193d-243">Entity</span></span>

<span data-ttu-id="5193d-244">Para obtener un producto individual, envíe una solicitud GET a `http://localhost:port/odata/Products(1)`, donde "1" es el identificador de producto.</span><span class="sxs-lookup"><span data-stu-id="5193d-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="5193d-245">Formatos de serialización de OData</span><span class="sxs-lookup"><span data-stu-id="5193d-245">OData Serialization Formats</span></span>

<span data-ttu-id="5193d-246">OData admite varios formatos de serialización:</span><span class="sxs-lookup"><span data-stu-id="5193d-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="5193d-247">Publicación de Atom (XML)</span><span class="sxs-lookup"><span data-stu-id="5193d-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="5193d-248">JSON "light" (introducida en OData v3)</span><span class="sxs-lookup"><span data-stu-id="5193d-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="5193d-249">JSON "detallado" (OData v2)</span><span class="sxs-lookup"><span data-stu-id="5193d-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="5193d-250">De forma predeterminada, API Web usa el formato de AtomPubJSON "light".</span><span class="sxs-lookup"><span data-stu-id="5193d-250">By default, Web API uses AtomPubJSON "light" format.</span></span>

<span data-ttu-id="5193d-251">Para obtener el formato AtomPub, establezca el encabezado Accept "aplicación/Atom+XML".</span><span class="sxs-lookup"><span data-stu-id="5193d-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="5193d-252">Este es un cuerpo de respuesta de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5193d-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="5193d-253">Puede ver una desventaja obvia del formato de Atom: Es mucho más detallado que la luz JSON.</span><span class="sxs-lookup"><span data-stu-id="5193d-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="5193d-254">Sin embargo, si tiene un cliente que entiende AtomPub, el cliente puede preferir ese formato JSON.</span><span class="sxs-lookup"><span data-stu-id="5193d-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="5193d-255">Esta es la versión ligera de JSON de la misma entidad:</span><span class="sxs-lookup"><span data-stu-id="5193d-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="5193d-256">El formato JSON light se introdujo en la versión 3 del Protocolo OData.</span><span class="sxs-lookup"><span data-stu-id="5193d-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="5193d-257">Por compatibilidad con versiones anteriores, un cliente puede solicitar el formato JSON "detallado" anterior.</span><span class="sxs-lookup"><span data-stu-id="5193d-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="5193d-258">Para solicitar JSON detallado, establezca el encabezado Accept en `application/json;odata=verbose`.</span><span class="sxs-lookup"><span data-stu-id="5193d-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="5193d-259">Esta es la versión detallada:</span><span class="sxs-lookup"><span data-stu-id="5193d-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="5193d-260">Este formato transmite más metadatos en el cuerpo de respuesta, que puede agregar una sobrecarga considerable a través de una sesión completa.</span><span class="sxs-lookup"><span data-stu-id="5193d-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="5193d-261">Además, agrega un nivel de direccionamiento indirecto ajustando el objeto en una propiedad denominada "d".</span><span class="sxs-lookup"><span data-stu-id="5193d-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="5193d-262">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="5193d-262">Next Steps</span></span>

- [<span data-ttu-id="5193d-263">Agregar las relaciones de entidad</span><span class="sxs-lookup"><span data-stu-id="5193d-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="5193d-264">Agregar acciones de OData</span><span class="sxs-lookup"><span data-stu-id="5193d-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="5193d-265">Llame al servicio de OData desde un cliente .NET</span><span class="sxs-lookup"><span data-stu-id="5193d-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
