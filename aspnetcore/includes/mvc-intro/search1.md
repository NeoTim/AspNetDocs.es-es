---
ms.openlocfilehash: 24726fba7f431f701b264a988a8de1b67d41d8a2
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048722"
---
# <a name="add-search-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="7479b-101">Agregar búsqueda a una aplicación de ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="7479b-101">Add search to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="7479b-102">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7479b-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7479b-103">En esta sección agregará capacidad de búsqueda para el método de acción `Index` que permite buscar películas por *género* o *nombre*.</span><span class="sxs-lookup"><span data-stu-id="7479b-103">In this section you add search capability to the `Index` action method that lets you search movies by *genre* or *name*.</span></span>

<span data-ttu-id="7479b-104">Actualice el método `Index` con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7479b-104">Update the `Index` method with the following code:</span></span>
<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

<span data-ttu-id="7479b-105">La primera línea del método de acción `Index` crea una consulta [LINQ](/dotnet/standard/using-linq) para seleccionar las películas:</span><span class="sxs-lookup"><span data-stu-id="7479b-105">The first line of the `Index` action method creates a [LINQ](/dotnet/standard/using-linq) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="7479b-106">En este momento *solo* se define la consulta, **no** se ejecuta en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7479b-106">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="7479b-107">Si el parámetro `searchString` contiene una cadena, la consulta de películas se modifica para filtrar según el valor de la cadena de búsqueda:</span><span class="sxs-lookup"><span data-stu-id="7479b-107">If the `searchString` parameter contains a string, the movies query is modified to filter on the value of the search string:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

<span data-ttu-id="7479b-108">El código `s => s.Title.Contains()` anterior es una [expresión Lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="7479b-108">The `s => s.Title.Contains()` code above is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="7479b-109">Las lambdas se usan en consultas [LINQ](/dotnet/standard/using-linq) basadas en métodos como argumentos para métodos de operador de consulta estándar, tales como el método [Where](/dotnet/api/system.linq.enumerable.where) o `Contains` (usado en el código anterior).</span><span class="sxs-lookup"><span data-stu-id="7479b-109">Lambdas are used in method-based [LINQ](/dotnet/standard/using-linq) queries as arguments to standard query operator methods such as the [Where](/dotnet/api/system.linq.enumerable.where) method or `Contains` (used in the code above).</span></span> <span data-ttu-id="7479b-110">Las consultas LINQ no se ejecutan cuando se definen ni cuando se modifican mediante una llamada a un método, como `Where`, `Contains` u `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="7479b-110">LINQ queries are not executed when they're defined or when they're modified by calling a method such as `Where`, `Contains`  or `OrderBy`.</span></span> <span data-ttu-id="7479b-111">En su lugar, se aplaza la ejecución de la consulta.</span><span class="sxs-lookup"><span data-stu-id="7479b-111">Rather, query execution is deferred.</span></span>  <span data-ttu-id="7479b-112">Esto significa que la evaluación de una expresión se aplaza hasta que su valor realizado se repita realmente o se llame al método `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="7479b-112">That means that the evaluation of an expression is delayed until its realized value is actually iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="7479b-113">Para más información sobre la ejecución de consultas en diferido, vea [Ejecución de la consulta](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="7479b-113">For more information about deferred query execution, see [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span></span>

<span data-ttu-id="7479b-114">Nota: El método [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) se ejecuta en la base de datos, no en el código de c# que se muestra arriba.</span><span class="sxs-lookup"><span data-stu-id="7479b-114">Note: The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the c# code shown above.</span></span> <span data-ttu-id="7479b-115">La distinción entre mayúsculas y minúsculas en la consulta depende de la base de datos y la intercalación.</span><span class="sxs-lookup"><span data-stu-id="7479b-115">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="7479b-116">En SQL Server, [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) se asigna a [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), que distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="7479b-116">On SQL Server, [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="7479b-117">En SQLite, con la intercalación predeterminada, se distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="7479b-117">In SQLlite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="7479b-118">Navegue a `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="7479b-118">Navigate to `/Movies/Index`.</span></span> <span data-ttu-id="7479b-119">Anexe una cadena de consulta como `?searchString=Ghost` a la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="7479b-119">Append a query string such as `?searchString=Ghost` to the URL.</span></span> <span data-ttu-id="7479b-120">Se muestran las películas filtradas.</span><span class="sxs-lookup"><span data-stu-id="7479b-120">The filtered movies are displayed.</span></span>

![Vista de índice](~/tutorials/first-mvc-app/search/_static/ghost.png)

<span data-ttu-id="7479b-122">Si se cambia la firma del método `Index` para que tenga un parámetro con el nombre `id`, el parámetro `id` coincidirá con el marcador `{id}` opcional para el conjunto de rutas predeterminado en *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7479b-122">If you change the signature of the `Index` method to have a parameter named `id`, the `id` parameter will match the optional `{id}` placeholder for the default routes set in *Startup.cs*.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]
