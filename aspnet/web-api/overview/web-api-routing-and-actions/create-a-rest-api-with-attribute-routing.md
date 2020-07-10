---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Creación de una API de REST con enrutamiento de atributos en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 6eac36767bf34857d5341188d0653e7fec7cade2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "86188918"
---
# <a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="89472-102">Creación de una API de REST con enrutamiento de atributos en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="89472-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>

<span data-ttu-id="89472-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="89472-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="89472-104">Web API 2 admite un nuevo tipo de enrutamiento, denominado *enrutamiento de atributos*.</span><span class="sxs-lookup"><span data-stu-id="89472-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="89472-105">Para obtener información general sobre el enrutamiento de atributos, consulte [enrutamiento de atributos en Web API 2](attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="89472-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="89472-106">En este tutorial, usará el enrutamiento de atributos para crear una API de REST para una colección de libros.</span><span class="sxs-lookup"><span data-stu-id="89472-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="89472-107">La API admitirá las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="89472-107">The API will support the following actions:</span></span>

| <span data-ttu-id="89472-108">Acción</span><span class="sxs-lookup"><span data-stu-id="89472-108">Action</span></span> | <span data-ttu-id="89472-109">URI de ejemplo</span><span class="sxs-lookup"><span data-stu-id="89472-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="89472-110">Obtiene una lista de todos los libros.</span><span class="sxs-lookup"><span data-stu-id="89472-110">Get a list of all books.</span></span> | <span data-ttu-id="89472-111">/api/books</span><span class="sxs-lookup"><span data-stu-id="89472-111">/api/books</span></span> |
| <span data-ttu-id="89472-112">Obtiene un libro por identificador.</span><span class="sxs-lookup"><span data-stu-id="89472-112">Get a book by ID.</span></span> | <span data-ttu-id="89472-113">/api/books/1</span><span class="sxs-lookup"><span data-stu-id="89472-113">/api/books/1</span></span> |
| <span data-ttu-id="89472-114">Obtener los detalles de un libro.</span><span class="sxs-lookup"><span data-stu-id="89472-114">Get the details of a book.</span></span> | <span data-ttu-id="89472-115">/api/books/1/details</span><span class="sxs-lookup"><span data-stu-id="89472-115">/api/books/1/details</span></span> |
| <span data-ttu-id="89472-116">Obtiene una lista de libros por género.</span><span class="sxs-lookup"><span data-stu-id="89472-116">Get a list of books by genre.</span></span> | <span data-ttu-id="89472-117">/api/books/fantasy</span><span class="sxs-lookup"><span data-stu-id="89472-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="89472-118">Obtiene una lista de libros por fecha de publicación.</span><span class="sxs-lookup"><span data-stu-id="89472-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="89472-119">/API/Books/Date/2013-02-16/API/Books/Date/2013/02/16 (forma alternativa)</span><span class="sxs-lookup"><span data-stu-id="89472-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="89472-120">Obtener una lista de libros por un autor determinado.</span><span class="sxs-lookup"><span data-stu-id="89472-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="89472-121">/api/authors/1/books</span><span class="sxs-lookup"><span data-stu-id="89472-121">/api/authors/1/books</span></span> |

<span data-ttu-id="89472-122">Todos los métodos son de solo lectura (solicitudes HTTP GET).</span><span class="sxs-lookup"><span data-stu-id="89472-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="89472-123">En el nivel de datos, usaremos Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="89472-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="89472-124">Los registros del libro tendrán los campos siguientes:</span><span class="sxs-lookup"><span data-stu-id="89472-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="89472-125">Id.</span><span class="sxs-lookup"><span data-stu-id="89472-125">ID</span></span>
- <span data-ttu-id="89472-126">Título</span><span class="sxs-lookup"><span data-stu-id="89472-126">Title</span></span>
- <span data-ttu-id="89472-127">Género</span><span class="sxs-lookup"><span data-stu-id="89472-127">Genre</span></span>
- <span data-ttu-id="89472-128">fecha de publicación</span><span class="sxs-lookup"><span data-stu-id="89472-128">Publication date</span></span>
- <span data-ttu-id="89472-129">Precio</span><span class="sxs-lookup"><span data-stu-id="89472-129">Price</span></span>
- <span data-ttu-id="89472-130">Descripción</span><span class="sxs-lookup"><span data-stu-id="89472-130">Description</span></span>
- <span data-ttu-id="89472-131">AuthorID (clave externa a una tabla de autores)</span><span class="sxs-lookup"><span data-stu-id="89472-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="89472-132">Sin embargo, para la mayoría de las solicitudes, la API devolverá un subconjunto de estos datos (título, autor y género).</span><span class="sxs-lookup"><span data-stu-id="89472-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="89472-133">Para obtener el registro completo, el cliente solicita `/api/books/{id}/details` .</span><span class="sxs-lookup"><span data-stu-id="89472-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="89472-134">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="89472-134">Prerequisites</span></span>

<span data-ttu-id="89472-135">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise Edition.</span><span class="sxs-lookup"><span data-stu-id="89472-135">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="89472-136">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="89472-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="89472-137">En primer lugar, ejecuta Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="89472-137">Start by running Visual Studio.</span></span> <span data-ttu-id="89472-138">En el menú **Archivo**, seleccione **Nuevo** y haga clic en **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="89472-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="89472-139">Expanda la categoría **instalado**de  >  **Visual C#** .</span><span class="sxs-lookup"><span data-stu-id="89472-139">Expand the **Installed** > **Visual C#** category.</span></span> <span data-ttu-id="89472-140">En **Visual C#**, seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="89472-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="89472-141">En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.net (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="89472-141">In the list of project templates, select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="89472-142">Asigne al proyecto el nombre &quot; BooksAPI &quot; .</span><span class="sxs-lookup"><span data-stu-id="89472-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="89472-143">En el cuadro de diálogo **nueva aplicación Web de ASP.net** , seleccione la plantilla **vacía** .</span><span class="sxs-lookup"><span data-stu-id="89472-143">In the **New ASP.NET Web Application** dialog, select the **Empty** template.</span></span> <span data-ttu-id="89472-144">En "Agregar carpetas y referencias principales para", active la casilla **API Web** .</span><span class="sxs-lookup"><span data-stu-id="89472-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="89472-145">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="89472-145">Click **OK**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="89472-146">Esto crea un proyecto de esqueleto que está configurado para la funcionalidad de la API Web.</span><span class="sxs-lookup"><span data-stu-id="89472-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="89472-147">Modelos de dominio</span><span class="sxs-lookup"><span data-stu-id="89472-147">Domain Models</span></span>

<span data-ttu-id="89472-148">A continuación, agregue clases para los modelos de dominio.</span><span class="sxs-lookup"><span data-stu-id="89472-148">Next, add classes for domain models.</span></span> <span data-ttu-id="89472-149">En el Explorador de soluciones, haga clic con el botón derecho en la carpeta Models.</span><span class="sxs-lookup"><span data-stu-id="89472-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="89472-150">Seleccione **Agregar**y, a continuación, seleccione **clase**.</span><span class="sxs-lookup"><span data-stu-id="89472-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="89472-151">Asigne `Author` como nombre de la clase.</span><span class="sxs-lookup"><span data-stu-id="89472-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="89472-152">Reemplace el código de Author.cs por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="89472-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="89472-153">Ahora, agregue otra clase denominada `Book` .</span><span class="sxs-lookup"><span data-stu-id="89472-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="89472-154">Incorporación de un controlador de API Web</span><span class="sxs-lookup"><span data-stu-id="89472-154">Add a Web API Controller</span></span>

<span data-ttu-id="89472-155">En este paso, vamos a agregar un controlador de API Web que usa Entity Framework como capa de datos.</span><span class="sxs-lookup"><span data-stu-id="89472-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="89472-156">Presione Ctrl+Mayús+B para compilar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="89472-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="89472-157">Entity Framework usa la reflexión para detectar las propiedades de los modelos, por lo que requiere un ensamblado compilado para crear el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="89472-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="89472-158">En el Explorador de soluciones, haga clic con el botón derecho en la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="89472-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="89472-159">Seleccione **Agregar**y, a continuación, seleccione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="89472-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="89472-160">En el cuadro de diálogo **Agregar scaffold** , seleccione **controlador de Web API 2 con acciones mediante Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="89472-160">In the **Add Scaffold** dialog, select **Web API 2 Controller with actions, using Entity Framework**.</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="89472-161">En el cuadro de diálogo **Agregar controlador** , en **nombre del controlador**, escriba &quot; BooksController &quot; .</span><span class="sxs-lookup"><span data-stu-id="89472-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="89472-162">Active la &quot; casilla usar acciones de controlador Async &quot; .</span><span class="sxs-lookup"><span data-stu-id="89472-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="89472-163">En **clase de modelo**, seleccione &quot; libro &quot; .</span><span class="sxs-lookup"><span data-stu-id="89472-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="89472-164">(Si no ve la `Book` clase que aparece en la lista desplegable, asegúrese de que ha compilado el proyecto). A continuación, haga clic en el botón "+".</span><span class="sxs-lookup"><span data-stu-id="89472-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="89472-165">Haga clic en **Agregar** en el cuadro de diálogo **nuevo contexto de datos** .</span><span class="sxs-lookup"><span data-stu-id="89472-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="89472-166">Haga clic en **Agregar** en el cuadro de diálogo **Agregar controlador** .</span><span class="sxs-lookup"><span data-stu-id="89472-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="89472-167">El scaffolding agrega una clase denominada `BooksController` que define el controlador de API.</span><span class="sxs-lookup"><span data-stu-id="89472-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="89472-168">También agrega una clase denominada `BooksAPIContext` en la carpeta models, que define el contexto de datos para Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="89472-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="89472-169">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="89472-169">Seed the Database</span></span>

<span data-ttu-id="89472-170">En el menú herramientas, seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="89472-170">From the Tools menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="89472-171">En la ventana Package Manager Console, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="89472-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="89472-172">Este comando crea una carpeta Migrations y agrega un nuevo archivo de código denominado Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="89472-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="89472-173">Abra este archivo y agregue el código siguiente al `Configuration.Seed` método.</span><span class="sxs-lookup"><span data-stu-id="89472-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="89472-174">En la ventana de la consola del administrador de paquetes, escriba los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="89472-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="89472-175">Estos comandos crean una base de datos local e invocan el método de inicialización para rellenar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="89472-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="89472-176">Agregar clases DTO</span><span class="sxs-lookup"><span data-stu-id="89472-176">Add DTO Classes</span></span>

<span data-ttu-id="89472-177">Si ejecuta la aplicación ahora y envía una solicitud GET a/API/Books/1, la respuesta tendrá un aspecto similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="89472-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="89472-178">(He agregado sangría para mejorar la legibilidad).</span><span class="sxs-lookup"><span data-stu-id="89472-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="89472-179">En su lugar, deseo que esta solicitud devuelva un subconjunto de los campos.</span><span class="sxs-lookup"><span data-stu-id="89472-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="89472-180">Además, quiero devolver el nombre del autor, en lugar del identificador del autor.</span><span class="sxs-lookup"><span data-stu-id="89472-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="89472-181">Para ello, modificaremos los métodos de controlador para devolver un *objeto de transferencia de datos* (DTO) en lugar del modelo EF.</span><span class="sxs-lookup"><span data-stu-id="89472-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="89472-182">Un DTO es un objeto que está diseñado únicamente para transportar datos.</span><span class="sxs-lookup"><span data-stu-id="89472-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="89472-183">En explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **Agregar**  |  **nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="89472-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="89472-184">Asigne a la carpeta el nombre &quot; Dto &quot; .</span><span class="sxs-lookup"><span data-stu-id="89472-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="89472-185">Agregue una clase denominada `BookDto` a la carpeta dto, con la siguiente definición:</span><span class="sxs-lookup"><span data-stu-id="89472-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="89472-186">Agregue otra clase llamada `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="89472-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="89472-187">A continuación, actualice la `BooksController` clase para devolver `BookDto` las instancias.</span><span class="sxs-lookup"><span data-stu-id="89472-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="89472-188">Usaremos el método [Queryable. Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) para proyectar `Book` instancias de en `BookDto` instancias de.</span><span class="sxs-lookup"><span data-stu-id="89472-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="89472-189">Este es el código actualizado para la clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="89472-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="89472-190">Eliminé los `PutBook` `PostBook` métodos, y `DeleteBook` , porque no son necesarios para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="89472-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>

<span data-ttu-id="89472-191">Ahora, si ejecuta la aplicación y solicita/API/Books/1, el cuerpo de la respuesta debe ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="89472-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="89472-192">Agregar atributos de ruta</span><span class="sxs-lookup"><span data-stu-id="89472-192">Add Route Attributes</span></span>

<span data-ttu-id="89472-193">A continuación, convertiremos el controlador para usar el enrutamiento de atributos.</span><span class="sxs-lookup"><span data-stu-id="89472-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="89472-194">En primer lugar, agregue un atributo **RoutePrefix** al controlador.</span><span class="sxs-lookup"><span data-stu-id="89472-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="89472-195">Este atributo define los segmentos de URI iniciales para todos los métodos de este controlador.</span><span class="sxs-lookup"><span data-stu-id="89472-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="89472-196">A continuación, agregue los atributos **[Route]** a las acciones del controlador, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="89472-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="89472-197">La plantilla de ruta de cada método de controlador es el prefijo más la cadena especificada en el atributo **Route** .</span><span class="sxs-lookup"><span data-stu-id="89472-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="89472-198">Para el `GetBook` método, la plantilla de ruta incluye la cadena parametrizada &quot; {ID: int} &quot; , que coincide si el segmento del URI contiene un valor entero.</span><span class="sxs-lookup"><span data-stu-id="89472-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="89472-199">Método</span><span class="sxs-lookup"><span data-stu-id="89472-199">Method</span></span> | <span data-ttu-id="89472-200">Plantilla de ruta</span><span class="sxs-lookup"><span data-stu-id="89472-200">Route Template</span></span> | <span data-ttu-id="89472-201">URI de ejemplo</span><span class="sxs-lookup"><span data-stu-id="89472-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="89472-202">"API/libros"</span><span class="sxs-lookup"><span data-stu-id="89472-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="89472-203">"API/books/{id: int}"</span><span class="sxs-lookup"><span data-stu-id="89472-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="89472-204">Obtener detalles del libro</span><span class="sxs-lookup"><span data-stu-id="89472-204">Get Book Details</span></span>

<span data-ttu-id="89472-205">Para obtener detalles del libro, el cliente enviará una solicitud GET a `/api/books/{id}/details` , donde *{ID}* es el identificador del libro.</span><span class="sxs-lookup"><span data-stu-id="89472-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="89472-206">Agregue el siguiente método a la clase `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="89472-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="89472-207">Si solicita `/api/books/1/details` , la respuesta tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="89472-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="89472-208">Obtener libros por género</span><span class="sxs-lookup"><span data-stu-id="89472-208">Get Books By Genre</span></span>

<span data-ttu-id="89472-209">Para obtener una lista de libros en un género específico, el cliente enviará una solicitud GET a `/api/books/genre` , donde *Genre* es el nombre del género.</span><span class="sxs-lookup"><span data-stu-id="89472-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="89472-210">(Por ejemplo, `/api/books/fantasy`).</span><span class="sxs-lookup"><span data-stu-id="89472-210">(For example, `/api/books/fantasy`.)</span></span>

<span data-ttu-id="89472-211">Agregue el método siguiente a `BooksController` .</span><span class="sxs-lookup"><span data-stu-id="89472-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="89472-212">Aquí estamos definiendo una ruta que contiene un parámetro {Genre} en la plantilla URI.</span><span class="sxs-lookup"><span data-stu-id="89472-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="89472-213">Tenga en cuenta que la API Web puede distinguir estos dos URI y enrutarlos a distintos métodos:</span><span class="sxs-lookup"><span data-stu-id="89472-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="89472-214">Esto se debe a que el `GetBook` método incluye una restricción de que el segmento "ID" debe ser un valor entero:</span><span class="sxs-lookup"><span data-stu-id="89472-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="89472-215">Si solicita/API/Books/Fantasy, la respuesta tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="89472-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="89472-216">Obtener libros por autor</span><span class="sxs-lookup"><span data-stu-id="89472-216">Get Books By Author</span></span>

<span data-ttu-id="89472-217">Para obtener una lista de los libros de un autor determinado, el cliente enviará una solicitud GET a `/api/authors/id/books` , donde *ID* es el identificador del autor.</span><span class="sxs-lookup"><span data-stu-id="89472-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="89472-218">Agregue el método siguiente a `BooksController` .</span><span class="sxs-lookup"><span data-stu-id="89472-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="89472-219">Este ejemplo es interesante porque los &quot; libros &quot; se tratan como un recurso secundario de los &quot; autores &quot; .</span><span class="sxs-lookup"><span data-stu-id="89472-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="89472-220">Este patrón es bastante común en las API de RESTful.</span><span class="sxs-lookup"><span data-stu-id="89472-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="89472-221">La tilde (~) de la plantilla de ruta invalida el prefijo de ruta en el atributo **RoutePrefix** .</span><span class="sxs-lookup"><span data-stu-id="89472-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="89472-222">Obtener libros por fecha de publicación</span><span class="sxs-lookup"><span data-stu-id="89472-222">Get Books By Publication Date</span></span>

<span data-ttu-id="89472-223">Para obtener una lista de libros por fecha de publicación, el cliente enviará una solicitud GET a `/api/books/date/yyyy-mm-dd` , donde *AAAA-MM-DD* es la fecha.</span><span class="sxs-lookup"><span data-stu-id="89472-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="89472-224">Esta es una manera de hacerlo:</span><span class="sxs-lookup"><span data-stu-id="89472-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="89472-225">El `{pubdate:datetime}` parámetro está restringido para que coincida con un valor **DateTime** .</span><span class="sxs-lookup"><span data-stu-id="89472-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="89472-226">Esto funciona, pero en realidad es más permisivo de lo que nos gustaría.</span><span class="sxs-lookup"><span data-stu-id="89472-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="89472-227">Por ejemplo, estos URI también coincidirán con la ruta:</span><span class="sxs-lookup"><span data-stu-id="89472-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="89472-228">No hay ningún problema al permitir estos URI.</span><span class="sxs-lookup"><span data-stu-id="89472-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="89472-229">Sin embargo, puede restringir la ruta a un formato determinado agregando una restricción de expresión regular a la plantilla de ruta:</span><span class="sxs-lookup"><span data-stu-id="89472-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="89472-230">Ahora solo coincidirán las fechas con el formato &quot; AAAA-MM-DD &quot; .</span><span class="sxs-lookup"><span data-stu-id="89472-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="89472-231">Tenga en cuenta que no usamos el regex para validar que obtuvimos una fecha real.</span><span class="sxs-lookup"><span data-stu-id="89472-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="89472-232">Esto se controla cuando Web API intenta convertir el segmento del URI en una instancia de **DateTime** .</span><span class="sxs-lookup"><span data-stu-id="89472-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="89472-233">Una fecha no válida, como ' 2012-47-99 ', no se convertirá y el cliente obtendrá un error 404.</span><span class="sxs-lookup"><span data-stu-id="89472-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="89472-234">También puede admitir un separador de barra diagonal ( `/api/books/date/yyyy/mm/dd` ) agregando otro atributo **[Route]** con una regex diferente.</span><span class="sxs-lookup"><span data-stu-id="89472-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="89472-235">Hay un detalle sutil pero importante aquí.</span><span class="sxs-lookup"><span data-stu-id="89472-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="89472-236">La segunda plantilla de ruta tiene un carácter comodín ( \* ) al principio del parámetro {pubDate}:</span><span class="sxs-lookup"><span data-stu-id="89472-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

<span data-ttu-id="89472-237">Esto indica al motor de enrutamiento que {pubDate} debe coincidir con el resto del URI.</span><span class="sxs-lookup"><span data-stu-id="89472-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="89472-238">De forma predeterminada, un parámetro de plantilla coincide con un único segmento de URI.</span><span class="sxs-lookup"><span data-stu-id="89472-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="89472-239">En este caso, queremos que {pubDate} abarque varios segmentos de URI:</span><span class="sxs-lookup"><span data-stu-id="89472-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="89472-240">Código del controlador</span><span class="sxs-lookup"><span data-stu-id="89472-240">Controller Code</span></span>

<span data-ttu-id="89472-241">Este es el código completo de la clase BooksController.</span><span class="sxs-lookup"><span data-stu-id="89472-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="89472-242">Resumen</span><span class="sxs-lookup"><span data-stu-id="89472-242">Summary</span></span>

<span data-ttu-id="89472-243">El enrutamiento de atributos proporciona más control y mayor flexibilidad a la hora de diseñar los URI para la API.</span><span class="sxs-lookup"><span data-stu-id="89472-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
