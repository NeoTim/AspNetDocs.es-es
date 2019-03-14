---
ms.openlocfilehash: cf30f952c08d9801eead9574d4fc5073e80ceb71
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57027892"
---
<span data-ttu-id="85982-101">En el código resaltado anterior se muestra cómo se agrega el contexto de la base de datos de películas al contenedor [inserción de dependencias](xref:fundamentals/dependency-injection) (en el archivo *Startup.cs*).</span><span class="sxs-lookup"><span data-stu-id="85982-101">The highlighted code above shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container (In the *Startup.cs* file).</span></span> <span data-ttu-id="85982-102">`services.AddDbContext<MvcMovieContext>(options =>` especifica la base de datos que se usará y la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="85982-102">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span> <span data-ttu-id="85982-103">`=>` es un [operador lambda](/dotnet/articles/csharp/language-reference/operators/lambda-operator).</span><span class="sxs-lookup"><span data-stu-id="85982-103">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator).</span></span>

<span data-ttu-id="85982-104">Abra el archivo *Controllers/MoviesController.cs* y examine el constructor:</span><span class="sxs-lookup"><span data-stu-id="85982-104">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)] 

<span data-ttu-id="85982-105">El constructor usa la [inserción de dependencias](xref:fundamentals/dependency-injection) para insertar el contexto de base de datos (`MvcMovieContext `) en el controlador.</span><span class="sxs-lookup"><span data-stu-id="85982-105">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext `) into the controller.</span></span> <span data-ttu-id="85982-106">El contexto de base de datos se usa en cada uno de los métodos [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) del controlador.</span><span class="sxs-lookup"><span data-stu-id="85982-106">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="85982-107">Modelos fuertemente tipados y la palabra clave @model</span><span class="sxs-lookup"><span data-stu-id="85982-107">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="85982-108">Anteriormente en este tutorial, vimos cómo un controlador puede pasar datos u objetos a una vista mediante el diccionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="85982-108">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="85982-109">El diccionario `ViewData` es un objeto dinámico que proporciona una cómoda manera enlazada en tiempo de ejecución de pasar información a una vista.</span><span class="sxs-lookup"><span data-stu-id="85982-109">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="85982-110">MVC también ofrece la capacidad de pasar objetos de modelo fuertemente tipados a una vista.</span><span class="sxs-lookup"><span data-stu-id="85982-110">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="85982-111">Este enfoque fuertemente tipado permite una mejor comprobación del código en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="85982-111">This strongly typed approach enables better compile-time checking of your code.</span></span> <span data-ttu-id="85982-112">El mecanismo de scaffolding usó este enfoque (que consiste en pasar un modelo fuertemente tipado) con la clase `MoviesController` y las vistas cuando creó los métodos y las vistas.</span><span class="sxs-lookup"><span data-stu-id="85982-112">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="85982-113">Examine el método `Details` generado en el archivo *Controllers/MoviesController.cs*:</span><span class="sxs-lookup"><span data-stu-id="85982-113">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MoviesController.cs?name=snippet_details)]

<span data-ttu-id="85982-114">El parámetro `id` suele pasarse como datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="85982-114">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="85982-115">Por ejemplo, `http://localhost:5000/movies/details/1` establece:</span><span class="sxs-lookup"><span data-stu-id="85982-115">For example `http://localhost:5000/movies/details/1` sets:</span></span>

* <span data-ttu-id="85982-116">El controlador en el controlador `movies` (el primer segmento de dirección URL).</span><span class="sxs-lookup"><span data-stu-id="85982-116">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="85982-117">La acción en `details` (el segundo segmento de dirección URL).</span><span class="sxs-lookup"><span data-stu-id="85982-117">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="85982-118">El identificador en 1 (el último segmento de dirección URL).</span><span class="sxs-lookup"><span data-stu-id="85982-118">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="85982-119">También puede pasar `id` con una cadena de consulta como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="85982-119">You can also pass in the `id` with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="85982-120">El parámetro `id` se define como un [tipo que acepta valores NULL](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) en caso de que no se proporcione un valor de identificador.</span><span class="sxs-lookup"><span data-stu-id="85982-120">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="85982-121">Se pasa una [expresión lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) a `FirstOrDefaultAsync` para seleccionar entidades de película que coincidan con los datos de enrutamiento o el valor de consulta de cadena.</span><span class="sxs-lookup"><span data-stu-id="85982-121">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="85982-122">Si se encuentra una película, se pasa una instancia del modelo `Movie` a la vista `Details`:</span><span class="sxs-lookup"><span data-stu-id="85982-122">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="85982-123">Examine el contenido del archivo *Views/Movies/Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="85982-123">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="85982-124">Mediante la inclusión de una instrucción `@model` en la parte superior del archivo de vista, puede especificar el tipo de objeto que espera la vista.</span><span class="sxs-lookup"><span data-stu-id="85982-124">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="85982-125">Al crear el controlador de película, se incluyó automáticamente la siguiente instrucción `@model` en la parte superior del archivo *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="85982-125">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="85982-126">Esta directiva `@model` permite acceder a la película que el controlador pasó a la vista usando un objeto `Model` fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="85982-126">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="85982-127">Por ejemplo, en la vista *Details.cshtml*, el código pasa cada campo de película a los asistentes de HTML `DisplayNameFor` y `DisplayFor` con el objeto `Model` fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="85982-127">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="85982-128">Los métodos `Create` y `Edit` y las vistas también pasan un objeto de modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="85982-128">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="85982-129">Examine la vista *Index.cshtml* y el método `Index` en el controlador Movies.</span><span class="sxs-lookup"><span data-stu-id="85982-129">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="85982-130">Observe cómo el código crea un objeto `List` cuando llama al método `View`.</span><span class="sxs-lookup"><span data-stu-id="85982-130">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="85982-131">El código pasa esta lista `Movies` desde el método de acción `Index` a la vista:</span><span class="sxs-lookup"><span data-stu-id="85982-131">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="85982-132">Cuando se creó el controlador movies, el scaffolding incluyó automáticamente la siguiente instrucción `@model` en la parte superior del archivo *Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="85982-132">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="85982-133">Esta directiva `@model` permite acceder a la lista de películas que el controlador pasó a la vista usando un objeto `Model` fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="85982-133">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="85982-134">Por ejemplo, en la vista *Index.cshtml*, el código recorre en bucle las películas con una instrucción `foreach` sobre el objeto `Model` fuertemente tipado:</span><span class="sxs-lookup"><span data-stu-id="85982-134">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="85982-135">Como el objeto `Model` es fuertemente tipado (como un objeto `IEnumerable<Movie>`), cada elemento del bucle está tipado como `Movie`.</span><span class="sxs-lookup"><span data-stu-id="85982-135">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="85982-136">Entre otras ventajas, esto significa que se obtiene una comprobación del código en tiempo de compilación:</span><span class="sxs-lookup"><span data-stu-id="85982-136">Among other benefits, this means that you get compile-time checking of the code:</span></span>
