---
uid: mvc/overview/getting-started/introduction/adding-search
title: Buscar | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/17/2019
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: be4e4d13e574b0fcb77d2d0fb8c6f58041b1ece2
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89044926"
---
# <a name="search"></a><span data-ttu-id="04f4c-102">Search</span><span class="sxs-lookup"><span data-stu-id="04f4c-102">Search</span></span>

[!INCLUDE [consider RP](~/includes/razor.md)]

## <a name="adding-a-search-method-and-search-view"></a><span data-ttu-id="04f4c-103">Agregar un método de búsqueda y una vista de búsqueda</span><span class="sxs-lookup"><span data-stu-id="04f4c-103">Adding a Search Method and Search View</span></span>

<span data-ttu-id="04f4c-104">En esta sección, agregará la funcionalidad de búsqueda al `Index` método de acción que le permite buscar películas por género o nombre.</span><span class="sxs-lookup"><span data-stu-id="04f4c-104">In this section you'll add search capability to the `Index` action method that lets you search movies by genre or name.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="04f4c-105">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="04f4c-105">Prerequisites</span></span>

<span data-ttu-id="04f4c-106">Para que coincida con las capturas de pantallas de esta sección, debe ejecutar la aplicación (F5) y agregar las siguientes películas a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="04f4c-106">To match this section's screenshots, you need to run the application (F5) and add the following movies to the database.</span></span>

| <span data-ttu-id="04f4c-107">Título</span><span class="sxs-lookup"><span data-stu-id="04f4c-107">Title</span></span> | <span data-ttu-id="04f4c-108">Fecha de la versión</span><span class="sxs-lookup"><span data-stu-id="04f4c-108">Release Date</span></span> | <span data-ttu-id="04f4c-109">Género</span><span class="sxs-lookup"><span data-stu-id="04f4c-109">Genre</span></span> | <span data-ttu-id="04f4c-110">Price</span><span class="sxs-lookup"><span data-stu-id="04f4c-110">Price</span></span> |
| ----- | ------------ | ----- | ----- |
| <span data-ttu-id="04f4c-111">Ghostbusters</span><span class="sxs-lookup"><span data-stu-id="04f4c-111">Ghostbusters</span></span> | <span data-ttu-id="04f4c-112">6/8/1984</span><span class="sxs-lookup"><span data-stu-id="04f4c-112">6/8/1984</span></span> | <span data-ttu-id="04f4c-113">Comedia</span><span class="sxs-lookup"><span data-stu-id="04f4c-113">Comedy</span></span> | <span data-ttu-id="04f4c-114">6,99</span><span class="sxs-lookup"><span data-stu-id="04f4c-114">6.99</span></span> |
| <span data-ttu-id="04f4c-115">Ghostbusters II</span><span class="sxs-lookup"><span data-stu-id="04f4c-115">Ghostbusters II</span></span> | <span data-ttu-id="04f4c-116">6/16/1989</span><span class="sxs-lookup"><span data-stu-id="04f4c-116">6/16/1989</span></span> | <span data-ttu-id="04f4c-117">Comedia</span><span class="sxs-lookup"><span data-stu-id="04f4c-117">Comedy</span></span> | <span data-ttu-id="04f4c-118">6,99</span><span class="sxs-lookup"><span data-stu-id="04f4c-118">6.99</span></span> |
| <span data-ttu-id="04f4c-119">Planeta del Apes</span><span class="sxs-lookup"><span data-stu-id="04f4c-119">Planet of the Apes</span></span> | <span data-ttu-id="04f4c-120">3/27/1986</span><span class="sxs-lookup"><span data-stu-id="04f4c-120">3/27/1986</span></span> | <span data-ttu-id="04f4c-121">Acción</span><span class="sxs-lookup"><span data-stu-id="04f4c-121">Action</span></span> | <span data-ttu-id="04f4c-122">5,99</span><span class="sxs-lookup"><span data-stu-id="04f4c-122">5.99</span></span> |

## <a name="updating-the-index-form"></a><span data-ttu-id="04f4c-123">Actualizar el formulario de índice</span><span class="sxs-lookup"><span data-stu-id="04f4c-123">Updating the Index Form</span></span>

<span data-ttu-id="04f4c-124">Empiece por actualizar el `Index` método de acción a la `MoviesController` clase existente.</span><span class="sxs-lookup"><span data-stu-id="04f4c-124">Start by updating the `Index` action method to the existing `MoviesController` class.</span></span> <span data-ttu-id="04f4c-125">Este es el código:</span><span class="sxs-lookup"><span data-stu-id="04f4c-125">Here's the code:</span></span>

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

<span data-ttu-id="04f4c-126">La primera línea del `Index` método crea la siguiente consulta [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) para seleccionar las películas:</span><span class="sxs-lookup"><span data-stu-id="04f4c-126">The first line of the `Index` method creates the following [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query to select the movies:</span></span>

[!code-csharp[Main](adding-search/samples/sample2.cs)]

<span data-ttu-id="04f4c-127">La consulta se define en este punto, pero aún no se ha ejecutado en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="04f4c-127">The query is defined at this point, but hasn't yet been run against the database.</span></span>

<span data-ttu-id="04f4c-128">Si el `searchString` parámetro contiene una cadena, la consulta de películas se modifica para filtrar según el valor de la cadena de búsqueda, mediante el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="04f4c-128">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string, using the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample3.cs)]

<span data-ttu-id="04f4c-129">El código `s => s.Title` anterior es una [expresión Lambda](https://msdn.microsoft.com/library/bb397687.aspx).</span><span class="sxs-lookup"><span data-stu-id="04f4c-129">The `s => s.Title` code above is a [Lambda Expression](https://msdn.microsoft.com/library/bb397687.aspx).</span></span> <span data-ttu-id="04f4c-130">Las lambdas se usan en consultas [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) basadas en métodos como argumentos para métodos de operador de consulta estándar, como el método [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) usado en el código anterior.</span><span class="sxs-lookup"><span data-stu-id="04f4c-130">Lambdas are used in method-based [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) queries as arguments to standard query operator methods such as the [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) method used in the above code.</span></span> <span data-ttu-id="04f4c-131">Las consultas LINQ no se ejecutan cuando se definen o cuando se modifican llamando a un método como `Where` o `OrderBy` .</span><span class="sxs-lookup"><span data-stu-id="04f4c-131">LINQ queries are not executed when they are defined or when they are modified by calling a method such as `Where` or `OrderBy`.</span></span> <span data-ttu-id="04f4c-132">En su lugar, se aplaza la ejecución de la consulta, lo que significa que la evaluación de una expresión se retrasa hasta que su valor realizado se recorra en iteración o [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) se llame al método.</span><span class="sxs-lookup"><span data-stu-id="04f4c-132">Instead, query execution is deferred, which means that the evaluation of an expression is delayed until its realized value is actually iterated over or the [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) method is called.</span></span> <span data-ttu-id="04f4c-133">En el `Search` ejemplo, la consulta se ejecuta en la vista *index. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="04f4c-133">In the `Search` sample, the query is executed in the *Index.cshtml* view.</span></span> <span data-ttu-id="04f4c-134">Para más información sobre la ejecución de consultas en diferido, vea [Ejecución de la consulta](https://msdn.microsoft.com/library/bb738633.aspx).</span><span class="sxs-lookup"><span data-stu-id="04f4c-134">For more information about deferred query execution, see [Query Execution](https://msdn.microsoft.com/library/bb738633.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="04f4c-135">El método [Contains](https://msdn.microsoft.com/library/bb155125.aspx) se ejecuta en la base de datos, no en el código de c# anterior.</span><span class="sxs-lookup"><span data-stu-id="04f4c-135">The [Contains](https://msdn.microsoft.com/library/bb155125.aspx) method is run on the database, not the c# code above.</span></span> <span data-ttu-id="04f4c-136">En la base de datos, [contiene](https://msdn.microsoft.com/library/bb155125.aspx) asignaciones a [SQL like](https://msdn.microsoft.com/library/ms179859.aspx), que distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="04f4c-136">On the database, [Contains](https://msdn.microsoft.com/library/bb155125.aspx) maps to [SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx), which is case insensitive.</span></span>

<span data-ttu-id="04f4c-137">Ahora puede actualizar la `Index` vista que mostrará el formulario al usuario.</span><span class="sxs-lookup"><span data-stu-id="04f4c-137">Now you can update the `Index` view that will display the form to the user.</span></span>

<span data-ttu-id="04f4c-138">Ejecute la aplicación y vaya a */Movies/index*.</span><span class="sxs-lookup"><span data-stu-id="04f4c-138">Run the application and navigate to */Movies/Index*.</span></span> <span data-ttu-id="04f4c-139">Anexe una cadena de consulta como `?searchString=ghost` a la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="04f4c-139">Append a query string such as `?searchString=ghost` to the URL.</span></span> <span data-ttu-id="04f4c-140">Se muestran las películas filtradas.</span><span class="sxs-lookup"><span data-stu-id="04f4c-140">The filtered movies are displayed.</span></span>

![SearchQryStr](adding-search/_static/image1.png)

<span data-ttu-id="04f4c-142">Si cambia la firma del `Index` método para que tenga un parámetro denominado `id` , el `id` parámetro coincidirá con el `{id}` marcador de posición de las rutas predeterminadas establecidas en el archivo \* \_ Start\RouteConfig.CS\* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="04f4c-142">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the `{id}` placeholder for the default routes set in the *App\_Start\RouteConfig.cs* file.</span></span>

[!code-json[Main](adding-search/samples/sample4.txt)]

<span data-ttu-id="04f4c-143">El método original tiene el siguiente `Index` aspecto::</span><span class="sxs-lookup"><span data-stu-id="04f4c-143">The original `Index` method looks like this::</span></span>

[!code-csharp[Main](adding-search/samples/sample5.cs)]

<span data-ttu-id="04f4c-144">El `Index` método modificado tendría el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="04f4c-144">The modified `Index` method would look as follows:</span></span>

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

<span data-ttu-id="04f4c-145">Ahora puede pasar el título de la búsqueda como datos de ruta (un segmento de dirección URL) en lugar de como un valor de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="04f4c-145">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![](adding-search/_static/image2.png)

<span data-ttu-id="04f4c-146">Sin embargo, no se puede esperar que los usuarios modifiquen la dirección URL cada vez que quieran buscar una película.</span><span class="sxs-lookup"><span data-stu-id="04f4c-146">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="04f4c-147">Por lo tanto, ahora deberá agregar la interfaz de usuario con la que podrán filtrar las películas.</span><span class="sxs-lookup"><span data-stu-id="04f4c-147">So now you'll add UI to help them filter movies.</span></span> <span data-ttu-id="04f4c-148">Si ha cambiado la firma del `Index` método para probar cómo pasar el parámetro de identificador enlazado a ruta, cámbielo de nuevo para que el `Index` método tome un parámetro de cadena denominado `searchString` :</span><span class="sxs-lookup"><span data-stu-id="04f4c-148">If you changed the signature of the `Index` method to test how to pass the route-bound ID parameter, change it back so that your `Index` method takes a string parameter named `searchString`:</span></span>

[!code-csharp[Main](adding-search/samples/sample7.cs)]

<span data-ttu-id="04f4c-149">Abra el archivo *Views\Movies\Index.cshtml* y, justo después `@Html.ActionLink("Create New", "Create")` , agregue el marcado de formulario resaltado a continuación:</span><span class="sxs-lookup"><span data-stu-id="04f4c-149">Open the *Views\Movies\Index.cshtml* file, and just after `@Html.ActionLink("Create New", "Create")`, add the form markup highlighted below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

<span data-ttu-id="04f4c-150">El `Html.BeginForm` ayudante crea una etiqueta de apertura `<form>` .</span><span class="sxs-lookup"><span data-stu-id="04f4c-150">The `Html.BeginForm` helper creates an opening `<form>` tag.</span></span> <span data-ttu-id="04f4c-151">El `Html.BeginForm` ayudante hace que el formulario se publique en sí mismo cuando el usuario envía el formulario haciendo clic en el botón **filtro** .</span><span class="sxs-lookup"><span data-stu-id="04f4c-151">The `Html.BeginForm` helper causes the form to post to itself when the user submits the form by clicking the **Filter** button.</span></span>

<span data-ttu-id="04f4c-152">Visual Studio 2013 tiene una mejora agradable al mostrar y editar archivos de vista.</span><span class="sxs-lookup"><span data-stu-id="04f4c-152">Visual Studio 2013 has a nice improvement when displaying and editing View files.</span></span> <span data-ttu-id="04f4c-153">Al ejecutar la aplicación con un archivo de vista abierto, Visual Studio 2013 invoca el método de acción de controlador correcto para mostrar la vista.</span><span class="sxs-lookup"><span data-stu-id="04f4c-153">When you run the application with a view file open, Visual Studio 2013 invokes the correct controller action method to display the view.</span></span>

![](adding-search/_static/image3.png)

<span data-ttu-id="04f4c-154">Con la vista de índice abierta en Visual Studio (como se muestra en la imagen anterior), pulse Ctr F5 o F5 para ejecutar la aplicación y, a continuación, intente buscar una película.</span><span class="sxs-lookup"><span data-stu-id="04f4c-154">With the Index view open in Visual Studio (as shown in the image above), tap Ctr F5 or F5 to run the application and then try searching for a movie.</span></span>

![](adding-search/_static/image4.png)

<span data-ttu-id="04f4c-155">No hay ninguna `HttpPost` sobrecarga del `Index` método.</span><span class="sxs-lookup"><span data-stu-id="04f4c-155">There's no `HttpPost` overload of the `Index` method.</span></span> <span data-ttu-id="04f4c-156">No es necesario porque el método no cambia el estado de la aplicación, simplemente filtrando los datos.</span><span class="sxs-lookup"><span data-stu-id="04f4c-156">You don't need it, because the method isn't changing the state of the application, just filtering data.</span></span>

<span data-ttu-id="04f4c-157">Después, puede agregar el método `HttpPost Index` siguiente.</span><span class="sxs-lookup"><span data-stu-id="04f4c-157">You could add the following `HttpPost Index` method.</span></span> <span data-ttu-id="04f4c-158">En ese caso, el invocador de acción coincidiría con el `HttpPost Index` método y el `HttpPost Index` método se ejecutaría tal y como se muestra en la imagen siguiente.</span><span class="sxs-lookup"><span data-stu-id="04f4c-158">In that case, the action invoker would match the `HttpPost Index` method, and the `HttpPost Index` method would run as shown in the image below.</span></span>

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

<span data-ttu-id="04f4c-160">Sin embargo, aunque agregue esta versión de `HttpPost` al método `Index`, hay una limitación en cómo se ha implementado todo esto.</span><span class="sxs-lookup"><span data-stu-id="04f4c-160">However, even if you add this `HttpPost` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="04f4c-161">Supongamos que quiere marcar una búsqueda en particular o que quiere enviar un vínculo a sus amigos donde puedan hacer clic para ver la misma lista filtrada de películas.</span><span class="sxs-lookup"><span data-stu-id="04f4c-161">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="04f4c-162">Tenga en cuenta que la dirección URL de la solicitud HTTP POST es la misma que la dirección URL de la solicitud GET (localhost: xxxxx/Movies/index) (no hay información de búsqueda en la propia dirección URL).</span><span class="sxs-lookup"><span data-stu-id="04f4c-162">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL itself.</span></span> <span data-ttu-id="04f4c-163">En este momento, la información de la cadena de búsqueda se envía al servidor como un valor de campo de formulario.</span><span class="sxs-lookup"><span data-stu-id="04f4c-163">Right now, the search string information is sent to the server as a form field value.</span></span> <span data-ttu-id="04f4c-164">Esto significa que no se puede capturar esa información de búsqueda para marcar o enviar a amigos en una dirección URL.</span><span class="sxs-lookup"><span data-stu-id="04f4c-164">This means you can't capture that search information to bookmark or send to friends in a URL.</span></span>

<span data-ttu-id="04f4c-165">La solución consiste en usar una sobrecarga de `BeginForm` que especifique que la solicitud post debe agregar la información de búsqueda a la dirección URL y que se debe enrutar a la `HttpGet` versión del `Index` método.</span><span class="sxs-lookup"><span data-stu-id="04f4c-165">The solution is to use an overload of `BeginForm` that specifies that the POST request should add the search information to the URL and that it should be routed to the `HttpGet` version of the `Index` method.</span></span> <span data-ttu-id="04f4c-166">Reemplace el método sin parámetros existente `BeginForm` por el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="04f4c-166">Replace the existing parameterless `BeginForm` method with the following markup:</span></span>

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

<span data-ttu-id="04f4c-168">Ahora, cuando se envía una búsqueda, la dirección URL contiene una cadena de consulta de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="04f4c-168">Now when you submit a search, the URL contains a search query string.</span></span> <span data-ttu-id="04f4c-169">La búsqueda también será dirigida al método de acción `HttpGet Index`, aunque tenga un método `HttpPost Index`.</span><span class="sxs-lookup"><span data-stu-id="04f4c-169">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a><span data-ttu-id="04f4c-171">Agregar búsqueda por género</span><span class="sxs-lookup"><span data-stu-id="04f4c-171">Adding Search by Genre</span></span>

<span data-ttu-id="04f4c-172">Si agregó la `HttpPost` versión del `Index` método, elimínela ahora.</span><span class="sxs-lookup"><span data-stu-id="04f4c-172">If you added the `HttpPost` version of the `Index` method, delete it now.</span></span>

<span data-ttu-id="04f4c-173">A continuación, agregará una característica para permitir que los usuarios busquen películas por género.</span><span class="sxs-lookup"><span data-stu-id="04f4c-173">Next, you'll add a feature to let users search for movies by genre.</span></span> <span data-ttu-id="04f4c-174">Reemplace el método `Index` por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="04f4c-174">Replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](adding-search/samples/sample11.cs)]

<span data-ttu-id="04f4c-175">Esta versión del `Index` método toma un parámetro adicional, es decir, `movieGenre` .</span><span class="sxs-lookup"><span data-stu-id="04f4c-175">This version of the `Index` method takes an additional parameter, namely `movieGenre`.</span></span> <span data-ttu-id="04f4c-176">Las primeras líneas de código crean un `List` objeto para contener géneros de película de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="04f4c-176">The first few lines of code create a `List` object to hold movie genres from the database.</span></span>

<span data-ttu-id="04f4c-177">El código siguiente es una consulta LINQ que recupera todos los géneros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="04f4c-177">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](adding-search/samples/sample12.cs)]

<span data-ttu-id="04f4c-178">El código usa el `AddRange` método de la `List` colección genérica para agregar todos los géneros distintos a la lista.</span><span class="sxs-lookup"><span data-stu-id="04f4c-178">The code uses the `AddRange` method of the generic `List` collection to add all the distinct genres to the list.</span></span> <span data-ttu-id="04f4c-179">(Sin el `Distinct` modificador, se agregarán géneros duplicados; por ejemplo, comedia se agregaría dos veces en nuestro ejemplo).</span><span class="sxs-lookup"><span data-stu-id="04f4c-179">(Without the `Distinct` modifier, duplicate genres would be added — for example, comedy would be added twice in our sample).</span></span> <span data-ttu-id="04f4c-180">A continuación, el código almacena la lista de géneros en el `ViewBag.MovieGenre` objeto.</span><span class="sxs-lookup"><span data-stu-id="04f4c-180">The code then stores the list of genres in the `ViewBag.MovieGenre` object.</span></span> <span data-ttu-id="04f4c-181">Almacenar los datos de la categoría (como un género de la película) como un objeto [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) en un `ViewBag` y, después, tener acceso a los datos de la categoría en un cuadro de lista desplegable es un enfoque típico para las aplicaciones MVC.</span><span class="sxs-lookup"><span data-stu-id="04f4c-181">Storing category data (such a movie genres) as a [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) object in a `ViewBag`, then accessing the category data in a dropdown list box is a typical approach for MVC applications.</span></span>

<span data-ttu-id="04f4c-182">En el código siguiente se muestra cómo comprobar el `movieGenre` parámetro.</span><span class="sxs-lookup"><span data-stu-id="04f4c-182">The following code shows how to check the `movieGenre` parameter.</span></span> <span data-ttu-id="04f4c-183">Si no está vacío, el código restringe aún más la consulta de películas para limitar las películas seleccionadas al género especificado.</span><span class="sxs-lookup"><span data-stu-id="04f4c-183">If it's not empty, the code further constrains the movies query to limit the selected movies to the specified genre.</span></span>

[!code-csharp[Main](adding-search/samples/sample13.cs)]

<span data-ttu-id="04f4c-184">Como se indicó anteriormente, la consulta no se ejecuta en la base de datos hasta que la lista de películas se recorre en iteración (lo que sucede en la vista, después de que el `Index` método de acción vuelva).</span><span class="sxs-lookup"><span data-stu-id="04f4c-184">As stated previously, the query is not run on the database until the movie list is iterated over (which happens in the View, after the `Index` action method returns).</span></span>

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a><span data-ttu-id="04f4c-185">Agregar marcado a la vista de índice para admitir la búsqueda por género</span><span class="sxs-lookup"><span data-stu-id="04f4c-185">Adding Markup to the Index View to Support Search by Genre</span></span>

<span data-ttu-id="04f4c-186">Agregue una `Html.DropDownList` aplicación auxiliar al archivo *Views\Movies\Index.cshtml* , justo antes de la `TextBox` aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="04f4c-186">Add an `Html.DropDownList` helper to the *Views\Movies\Index.cshtml* file, just before the `TextBox` helper.</span></span> <span data-ttu-id="04f4c-187">A continuación se muestra el marcado completado:</span><span class="sxs-lookup"><span data-stu-id="04f4c-187">The completed markup is shown below:</span></span>

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

<span data-ttu-id="04f4c-188">En el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="04f4c-188">In the following code:</span></span>

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

<span data-ttu-id="04f4c-189">El parámetro "MovieGenre" proporciona la clave para `DropDownList` que el ayudante busque un `IEnumerable<SelectListItem>` en `ViewBag` .</span><span class="sxs-lookup"><span data-stu-id="04f4c-189">The parameter "MovieGenre" provides the key for the `DropDownList` helper to find a `IEnumerable<SelectListItem>` in the `ViewBag`.</span></span> <span data-ttu-id="04f4c-190">`ViewBag`Se rellenó en el método de acción:</span><span class="sxs-lookup"><span data-stu-id="04f4c-190">The `ViewBag` was populated in the action method:</span></span>

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

<span data-ttu-id="04f4c-191">El parámetro "All" proporciona una etiqueta de opción.</span><span class="sxs-lookup"><span data-stu-id="04f4c-191">The parameter "All" provides an option label.</span></span> <span data-ttu-id="04f4c-192">Si inspecciona esa opción en el explorador, verá que el atributo "valor" está vacío.</span><span class="sxs-lookup"><span data-stu-id="04f4c-192">If you inspect that choice in your browser, you'll see that its "value" attribute is empty.</span></span> <span data-ttu-id="04f4c-193">Puesto que el controlador solo filtra `if` la cadena no es `null` o está vacía, el envío de un valor vacío para `movieGenre` muestra todos los géneros.</span><span class="sxs-lookup"><span data-stu-id="04f4c-193">Since our controller only filters `if` the string is not `null` or empty, submitting an empty value for `movieGenre` shows all genres.</span></span>

<span data-ttu-id="04f4c-194">También puede establecer una opción para que se seleccione de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="04f4c-194">You can also set an option to be selected by default.</span></span> <span data-ttu-id="04f4c-195">Si desease "comedia" como opción predeterminada, cambiaría el código del controlador de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="04f4c-195">If you wanted "Comedy" as your default option, you would change the code in the Controller like so:</span></span>

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

<span data-ttu-id="04f4c-196">Ejecute la aplicación y vaya a */Movies/index*.</span><span class="sxs-lookup"><span data-stu-id="04f4c-196">Run the application and browse to */Movies/Index*.</span></span> <span data-ttu-id="04f4c-197">Pruebe una búsqueda por género, por nombre de película y por ambos criterios.</span><span class="sxs-lookup"><span data-stu-id="04f4c-197">Try a search by genre, by movie name, and by both criteria.</span></span>

![](adding-search/_static/image8.png)

<span data-ttu-id="04f4c-198">En esta sección se ha creado un método de acción de búsqueda y una vista que permiten a los usuarios buscar por título y género de la película.</span><span class="sxs-lookup"><span data-stu-id="04f4c-198">In this section you created a search action method and view that let users search by movie title and genre.</span></span> <span data-ttu-id="04f4c-199">En la siguiente sección, veremos cómo agregar una propiedad al `Movie` modelo y cómo agregar un inicializador que creará automáticamente una base de datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="04f4c-199">In the next section, you'll look at how to add a property to the `Movie` model and how to add an initializer that will automatically create a test database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="04f4c-200">[Anterior](examining-the-edit-methods-and-edit-view.md)
> [Siguiente](adding-a-new-field.md)</span><span class="sxs-lookup"><span data-stu-id="04f4c-200">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-a-new-field.md)</span></span>
