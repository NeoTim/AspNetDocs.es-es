---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Creación de una API de REST con enrutamiento mediante atributos en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: a58daa96410de734619bf65f84346137c7d3cf44
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59393306"
---
# <a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="1ec36-102">Creación de una API de REST con enrutamiento de atributos en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="1ec36-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>

<span data-ttu-id="1ec36-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1ec36-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1ec36-104">Web API 2 es compatible con un nuevo tipo de enrutamiento, llamado *enrutamiento mediante atributos*.</span><span class="sxs-lookup"><span data-stu-id="1ec36-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="1ec36-105">Para obtener una descripción general de enrutamiento mediante atributos, vea [enrutamiento mediante atributos en Web API 2](attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="1ec36-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="1ec36-106">En este tutorial, utilizará el enrutamiento mediante atributos para crear una API de REST para una colección de los libros en pantalla.</span><span class="sxs-lookup"><span data-stu-id="1ec36-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="1ec36-107">La API será compatible con las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="1ec36-107">The API will support the following actions:</span></span>

| <span data-ttu-id="1ec36-108">Acción</span><span class="sxs-lookup"><span data-stu-id="1ec36-108">Action</span></span> | <span data-ttu-id="1ec36-109">URI de ejemplo</span><span class="sxs-lookup"><span data-stu-id="1ec36-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="1ec36-110">Obtener una lista de todos los libros.</span><span class="sxs-lookup"><span data-stu-id="1ec36-110">Get a list of all books.</span></span> | <span data-ttu-id="1ec36-111">libros/api /</span><span class="sxs-lookup"><span data-stu-id="1ec36-111">/api/books</span></span> |
| <span data-ttu-id="1ec36-112">Obtenga un libro por identificador.</span><span class="sxs-lookup"><span data-stu-id="1ec36-112">Get a book by ID.</span></span> | <span data-ttu-id="1ec36-113">/API/Books/1</span><span class="sxs-lookup"><span data-stu-id="1ec36-113">/api/books/1</span></span> |
| <span data-ttu-id="1ec36-114">Obtener los detalles de un libro.</span><span class="sxs-lookup"><span data-stu-id="1ec36-114">Get the details of a book.</span></span> | <span data-ttu-id="1ec36-115">/api/books/1/details</span><span class="sxs-lookup"><span data-stu-id="1ec36-115">/api/books/1/details</span></span> |
| <span data-ttu-id="1ec36-116">Obtener una lista de libros por género.</span><span class="sxs-lookup"><span data-stu-id="1ec36-116">Get a list of books by genre.</span></span> | <span data-ttu-id="1ec36-117">/API/Books/fantasy</span><span class="sxs-lookup"><span data-stu-id="1ec36-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="1ec36-118">Obtener una lista de libros por fecha de publicación.</span><span class="sxs-lookup"><span data-stu-id="1ec36-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="1ec36-119">/API/Books/Date/2013-02-16 /api/books/date/2013/02/16 (de forma alternativa)</span><span class="sxs-lookup"><span data-stu-id="1ec36-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="1ec36-120">Obtener una lista de libros por un autor concreto.</span><span class="sxs-lookup"><span data-stu-id="1ec36-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="1ec36-121">/api/authors/1/books</span><span class="sxs-lookup"><span data-stu-id="1ec36-121">/api/authors/1/books</span></span> |

<span data-ttu-id="1ec36-122">Todos los métodos son de solo lectura (las solicitudes HTTP GET).</span><span class="sxs-lookup"><span data-stu-id="1ec36-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="1ec36-123">La capa de datos, vamos a usar Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1ec36-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="1ec36-124">Registros de libro tendrán los siguientes campos:</span><span class="sxs-lookup"><span data-stu-id="1ec36-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="1ec36-125">ID</span><span class="sxs-lookup"><span data-stu-id="1ec36-125">ID</span></span>
- <span data-ttu-id="1ec36-126">Título</span><span class="sxs-lookup"><span data-stu-id="1ec36-126">Title</span></span>
- <span data-ttu-id="1ec36-127">Genre</span><span class="sxs-lookup"><span data-stu-id="1ec36-127">Genre</span></span>
- <span data-ttu-id="1ec36-128">Fecha de publicación</span><span class="sxs-lookup"><span data-stu-id="1ec36-128">Publication date</span></span>
- <span data-ttu-id="1ec36-129">Precio</span><span class="sxs-lookup"><span data-stu-id="1ec36-129">Price</span></span>
- <span data-ttu-id="1ec36-130">Descripción</span><span class="sxs-lookup"><span data-stu-id="1ec36-130">Description</span></span>
- <span data-ttu-id="1ec36-131">AuthorID (clave externa a una tabla Authors)</span><span class="sxs-lookup"><span data-stu-id="1ec36-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="1ec36-132">Sin embargo, para la mayoría de las solicitudes, la API devolverá un subconjunto de estos datos (título, autor y género).</span><span class="sxs-lookup"><span data-stu-id="1ec36-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="1ec36-133">Para obtener el registro completo, el cliente solicitudes `/api/books/{id}/details`.</span><span class="sxs-lookup"><span data-stu-id="1ec36-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1ec36-134">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="1ec36-134">Prerequisites</span></span>

<span data-ttu-id="1ec36-135">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise edition.</span><span class="sxs-lookup"><span data-stu-id="1ec36-135">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="1ec36-136">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ec36-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="1ec36-137">Comience ejecutando Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ec36-137">Start by running Visual Studio.</span></span> <span data-ttu-id="1ec36-138">Desde el **archivo** menú, seleccione **New** y, a continuación, seleccione **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="1ec36-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="1ec36-139">Expanda el **instalado** > **Visual C#** categoría.</span><span class="sxs-lookup"><span data-stu-id="1ec36-139">Expand the **Installed** > **Visual C#** category.</span></span> <span data-ttu-id="1ec36-140">En **Visual C#**, seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="1ec36-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="1ec36-141">En la lista de plantillas de proyecto, seleccione **aplicación Web ASP.NET (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="1ec36-141">In the list of project templates, select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="1ec36-142">Denomine el proyecto &quot;BooksAPI&quot;.</span><span class="sxs-lookup"><span data-stu-id="1ec36-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="1ec36-143">En el **nueva aplicación Web ASP.NET** cuadro de diálogo, seleccione el **vacía** plantilla.</span><span class="sxs-lookup"><span data-stu-id="1ec36-143">In the **New ASP.NET Web Application** dialog, select the **Empty** template.</span></span> <span data-ttu-id="1ec36-144">En "Agregar carpetas y referencias centrales para", seleccione el **API Web** casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="1ec36-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="1ec36-145">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="1ec36-145">Click **OK**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="1ec36-146">Esto crea un proyecto de esqueleto configurada para la funcionalidad de API Web.</span><span class="sxs-lookup"><span data-stu-id="1ec36-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="1ec36-147">Modelos de dominio</span><span class="sxs-lookup"><span data-stu-id="1ec36-147">Domain Models</span></span>

<span data-ttu-id="1ec36-148">A continuación, agregue las clases para los modelos de dominio.</span><span class="sxs-lookup"><span data-stu-id="1ec36-148">Next, add classes for domain models.</span></span> <span data-ttu-id="1ec36-149">En el Explorador de soluciones, haga clic en la carpeta Models.</span><span class="sxs-lookup"><span data-stu-id="1ec36-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="1ec36-150">Seleccione **agregar**, a continuación, seleccione **clase**.</span><span class="sxs-lookup"><span data-stu-id="1ec36-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="1ec36-151">Asigne a la clase el nombre `Author`.</span><span class="sxs-lookup"><span data-stu-id="1ec36-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="1ec36-152">Reemplace el código en Author.cs con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1ec36-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="1ec36-153">Ahora agregue otra clase llamada `Book`.</span><span class="sxs-lookup"><span data-stu-id="1ec36-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="1ec36-154">Agregar un controlador de API Web</span><span class="sxs-lookup"><span data-stu-id="1ec36-154">Add a Web API Controller</span></span>

<span data-ttu-id="1ec36-155">En este paso, vamos a agregar un controlador de API Web que utiliza Entity Framework como la capa de datos.</span><span class="sxs-lookup"><span data-stu-id="1ec36-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="1ec36-156">Presione Ctrl+Mayús+B para compilar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="1ec36-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="1ec36-157">Entity Framework usa la reflexión para detectar las propiedades de los modelos, por lo que requiere un ensamblado compilado crear el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ec36-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="1ec36-158">En el Explorador de soluciones, haga clic en la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="1ec36-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="1ec36-159">Seleccione **agregar**, a continuación, seleccione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="1ec36-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="1ec36-160">En el **agregar Scaffold** cuadro de diálogo, seleccione **controlador Web API 2 con acciones mediante Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="1ec36-160">In the **Add Scaffold** dialog, select **Web API 2 Controller with actions, using Entity Framework**.</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="1ec36-161">En el **Agregar controlador** cuadro de diálogo para **nombre del controlador**, escriba &quot;BooksController&quot;.</span><span class="sxs-lookup"><span data-stu-id="1ec36-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="1ec36-162">Seleccione el &quot;usar acciones de controlador asincrónicas&quot; casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="1ec36-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="1ec36-163">Para **clase modelo**, seleccione &quot;libro&quot;.</span><span class="sxs-lookup"><span data-stu-id="1ec36-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="1ec36-164">(Si no ve el `Book` clase aparece en la lista desplegable, asegúrese de que ha generado el proyecto.) A continuación, haga clic en el botón "+".</span><span class="sxs-lookup"><span data-stu-id="1ec36-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="1ec36-165">Haga clic en **agregar** en el **nuevo contexto de datos** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="1ec36-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="1ec36-166">Haga clic en **agregar** en el **Agregar controlador** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="1ec36-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="1ec36-167">El scaffolding agrega una clase denominada `BooksController` que define el controlador de API.</span><span class="sxs-lookup"><span data-stu-id="1ec36-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="1ec36-168">También agrega una clase denominada `BooksAPIContext` en la carpeta Models, que define el contexto de datos para Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1ec36-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="1ec36-169">Inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="1ec36-169">Seed the Database</span></span>

<span data-ttu-id="1ec36-170">En el menú Herramientas, seleccione **Administrador de paquetes de NuGet**y, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="1ec36-170">From the Tools menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="1ec36-171">En la ventana de consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="1ec36-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="1ec36-172">Este comando crea una carpeta de migraciones y agrega un nuevo archivo de código denominado Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="1ec36-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="1ec36-173">Abra este archivo y agregue el código siguiente a la `Configuration.Seed` método.</span><span class="sxs-lookup"><span data-stu-id="1ec36-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="1ec36-174">En la ventana de consola de administrador de paquetes, escriba los siguientes comandos.</span><span class="sxs-lookup"><span data-stu-id="1ec36-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="1ec36-175">Estos comandos creación una base de datos local e invocan el método de inicialización para rellenar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ec36-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="1ec36-176">Agregar clases DTO</span><span class="sxs-lookup"><span data-stu-id="1ec36-176">Add DTO Classes</span></span>

<span data-ttu-id="1ec36-177">Si ejecuta la aplicación ahora y enviar una solicitud GET a /api/books/1, la respuesta será similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="1ec36-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="1ec36-178">(Agrega la sangría para mejorar la legibilidad).</span><span class="sxs-lookup"><span data-stu-id="1ec36-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="1ec36-179">En su lugar, quiero que esta solicitud para devolver un subconjunto de los campos.</span><span class="sxs-lookup"><span data-stu-id="1ec36-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="1ec36-180">Además, quiero para devolver el nombre del autor, en lugar de con el identificador del autor.</span><span class="sxs-lookup"><span data-stu-id="1ec36-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="1ec36-181">Para lograr esto, se modificará los métodos de controlador para devolver un *objeto de transferencia de datos* (DTO) en lugar del modelo EF.</span><span class="sxs-lookup"><span data-stu-id="1ec36-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="1ec36-182">Un DTO es un objeto que está diseñado solo para transportar datos.</span><span class="sxs-lookup"><span data-stu-id="1ec36-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="1ec36-183">En el Explorador de soluciones, haga clic en el proyecto y seleccione **agregar** | **nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="1ec36-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="1ec36-184">Nombre de la carpeta &quot;dto&quot;.</span><span class="sxs-lookup"><span data-stu-id="1ec36-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="1ec36-185">Agregue una clase denominada `BookDto` hasta la carpeta dto, con la siguiente definición:</span><span class="sxs-lookup"><span data-stu-id="1ec36-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="1ec36-186">Agregue otra clase llamada `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="1ec36-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="1ec36-187">A continuación, actualice el `BooksController` clase para devolver `BookDto` instancias.</span><span class="sxs-lookup"><span data-stu-id="1ec36-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="1ec36-188">Vamos a usar el [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) método al proyecto `Book` instancias `BookDto` instancias.</span><span class="sxs-lookup"><span data-stu-id="1ec36-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="1ec36-189">Este es el código para la clase de controlador actualizado.</span><span class="sxs-lookup"><span data-stu-id="1ec36-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="1ec36-190">Se ha eliminado el `PutBook`, `PostBook`, y `DeleteBook` métodos, ya que no son necesarios para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="1ec36-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>


<span data-ttu-id="1ec36-191">Ahora si ejecuta la aplicación y solicitar /api/books/1, el cuerpo de respuesta debe ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="1ec36-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="1ec36-192">Agregar atributos de ruta</span><span class="sxs-lookup"><span data-stu-id="1ec36-192">Add Route Attributes</span></span>

<span data-ttu-id="1ec36-193">A continuación, aprenderá a convertir el controlador para que use el enrutamiento mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="1ec36-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="1ec36-194">En primer lugar, agregue un **RoutePrefix** atributo al controlador.</span><span class="sxs-lookup"><span data-stu-id="1ec36-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="1ec36-195">Este atributo define los segmentos URI iniciales para todos los métodos en este controlador.</span><span class="sxs-lookup"><span data-stu-id="1ec36-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="1ec36-196">A continuación, agregue **[ruta]** atributos a las acciones del controlador, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="1ec36-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="1ec36-197">La plantilla de ruta para cada método de controlador es el prefijo además de la cadena especificada en el **ruta** atributo.</span><span class="sxs-lookup"><span data-stu-id="1ec36-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="1ec36-198">Para el `GetBook` método, la plantilla de ruta incluye la cadena parametrizada &quot;{id: int}&quot;, que detecta si el segmento del URI contiene un valor entero.</span><span class="sxs-lookup"><span data-stu-id="1ec36-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="1ec36-199">Método</span><span class="sxs-lookup"><span data-stu-id="1ec36-199">Method</span></span> | <span data-ttu-id="1ec36-200">Plantilla de ruta</span><span class="sxs-lookup"><span data-stu-id="1ec36-200">Route Template</span></span> | <span data-ttu-id="1ec36-201">URI de ejemplo</span><span class="sxs-lookup"><span data-stu-id="1ec36-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="1ec36-202">"api/libros"</span><span class="sxs-lookup"><span data-stu-id="1ec36-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="1ec36-203">"api/libros / {id: int}"</span><span class="sxs-lookup"><span data-stu-id="1ec36-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="1ec36-204">Obtener detalles del libro</span><span class="sxs-lookup"><span data-stu-id="1ec36-204">Get Book Details</span></span>

<span data-ttu-id="1ec36-205">Para obtener detalles del libro, el cliente enviará una solicitud GET a `/api/books/{id}/details`, donde *{id}* es el identificador del libro.</span><span class="sxs-lookup"><span data-stu-id="1ec36-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="1ec36-206">Agregue el método siguiente a la clase `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="1ec36-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="1ec36-207">Si se solicita `/api/books/1/details`, la respuesta tendrá un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="1ec36-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="1ec36-208">Obtener los libros en pantalla por género</span><span class="sxs-lookup"><span data-stu-id="1ec36-208">Get Books By Genre</span></span>

<span data-ttu-id="1ec36-209">Para obtener una lista de los libros en pantalla de un género específico, el cliente enviará una solicitud GET a `/api/books/genre`, donde *género* es el nombre del género.</span><span class="sxs-lookup"><span data-stu-id="1ec36-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="1ec36-210">(Por ejemplo: `/api/books/fantasy`.)</span><span class="sxs-lookup"><span data-stu-id="1ec36-210">(For example, `/api/books/fantasy`.)</span></span>

<span data-ttu-id="1ec36-211">Agregue el método siguiente a `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="1ec36-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="1ec36-212">Aquí se define una ruta que contiene un parámetro de {género} en la plantilla URI.</span><span class="sxs-lookup"><span data-stu-id="1ec36-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="1ec36-213">Tenga en cuenta que Web API es capaz de distinguir a estos dos identificadores URI y enrutarlas a diferentes métodos:</span><span class="sxs-lookup"><span data-stu-id="1ec36-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="1ec36-214">Eso es porque el `GetBook` método incluye una restricción de que el segmento "id" debe ser un valor entero:</span><span class="sxs-lookup"><span data-stu-id="1ec36-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="1ec36-215">Si se solicitan /api/books/fantasy, la respuesta será similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="1ec36-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="1ec36-216">Obtener los libros en pantalla por autor</span><span class="sxs-lookup"><span data-stu-id="1ec36-216">Get Books By Author</span></span>

<span data-ttu-id="1ec36-217">Para obtener una lista de libros de una para un autor concreto, el cliente enviará una solicitud GET a `/api/authors/id/books`, donde *id* es el identificador del autor.</span><span class="sxs-lookup"><span data-stu-id="1ec36-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="1ec36-218">Agregue el método siguiente a `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="1ec36-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="1ec36-219">En este ejemplo es interesante porque &quot;libros&quot; es trata de un recurso secundario de &quot;autores&quot;.</span><span class="sxs-lookup"><span data-stu-id="1ec36-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="1ec36-220">Este patrón es bastante común en las API de RESTful.</span><span class="sxs-lookup"><span data-stu-id="1ec36-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="1ec36-221">La tilde (~) en la plantilla de ruta invalida el prefijo de ruta en el **RoutePrefix** atributo.</span><span class="sxs-lookup"><span data-stu-id="1ec36-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="1ec36-222">Obtener los libros en pantalla por fecha de publicación</span><span class="sxs-lookup"><span data-stu-id="1ec36-222">Get Books By Publication Date</span></span>

<span data-ttu-id="1ec36-223">Para obtener una lista de libros por fecha de publicación, el cliente enviará una solicitud GET a `/api/books/date/yyyy-mm-dd`, donde *aaaa-mm-dd* es la fecha.</span><span class="sxs-lookup"><span data-stu-id="1ec36-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="1ec36-224">Aquí es una manera de hacerlo:</span><span class="sxs-lookup"><span data-stu-id="1ec36-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="1ec36-225">El `{pubdate:datetime}` parámetro está restringido para que coincida con un **DateTime** valor.</span><span class="sxs-lookup"><span data-stu-id="1ec36-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="1ec36-226">Esto funciona, pero es realmente más permisivo que nos gustaría.</span><span class="sxs-lookup"><span data-stu-id="1ec36-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="1ec36-227">Por ejemplo, estos URI también coincidirá con la ruta:</span><span class="sxs-lookup"><span data-stu-id="1ec36-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="1ec36-228">No hay nada malo en lo que permite a estos URI.</span><span class="sxs-lookup"><span data-stu-id="1ec36-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="1ec36-229">Sin embargo, puede restringir la ruta a un formato determinado mediante la adición de una restricción de expresiones regulares para la plantilla de ruta:</span><span class="sxs-lookup"><span data-stu-id="1ec36-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="1ec36-230">Ahora solo las fechas en el formulario &quot;aaaa-mm-dd&quot; coincidirá.</span><span class="sxs-lookup"><span data-stu-id="1ec36-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="1ec36-231">Tenga en cuenta que no usamos la expresión regular para validar que se obtuvo una fecha real.</span><span class="sxs-lookup"><span data-stu-id="1ec36-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="1ec36-232">Que se administra al API Web intenta convertir el segmento URI en un **DateTime** instancia.</span><span class="sxs-lookup"><span data-stu-id="1ec36-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="1ec36-233">Una fecha no válida, como "2012-47-99' no se puede convertir, y el cliente obtendrá un error 404.</span><span class="sxs-lookup"><span data-stu-id="1ec36-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="1ec36-234">También puede admitir un separador de barra diagonal (`/api/books/date/yyyy/mm/dd`) agregando otro **[ruta]** atributo con un regex diferentes.</span><span class="sxs-lookup"><span data-stu-id="1ec36-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="1ec36-235">Hay un detalle importante pero sutil.</span><span class="sxs-lookup"><span data-stu-id="1ec36-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="1ec36-236">La segunda plantilla de ruta tiene un carácter comodín (\*) al principio del parámetro {pubdate}:</span><span class="sxs-lookup"><span data-stu-id="1ec36-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

<span data-ttu-id="1ec36-237">Esto indica que el motor de enrutamiento que {pubdate} debe coincidir con el resto de la dirección URI.</span><span class="sxs-lookup"><span data-stu-id="1ec36-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="1ec36-238">De forma predeterminada, un parámetro de plantilla coincide con un único segmento URI.</span><span class="sxs-lookup"><span data-stu-id="1ec36-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="1ec36-239">En este caso, queremos {pubdate} abarcan varios segmentos URI:</span><span class="sxs-lookup"><span data-stu-id="1ec36-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="1ec36-240">Código de controlador</span><span class="sxs-lookup"><span data-stu-id="1ec36-240">Controller Code</span></span>

<span data-ttu-id="1ec36-241">Este es el código completo de la clase BooksController.</span><span class="sxs-lookup"><span data-stu-id="1ec36-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="1ec36-242">Resumen</span><span class="sxs-lookup"><span data-stu-id="1ec36-242">Summary</span></span>

<span data-ttu-id="1ec36-243">Enrutamiento mediante atributos le ofrece más control y mayor flexibilidad al diseñar a los URI para la API.</span><span class="sxs-lookup"><span data-stu-id="1ec36-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
