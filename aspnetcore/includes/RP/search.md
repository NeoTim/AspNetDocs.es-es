---
ms.openlocfilehash: 68902f2839e3f8687509dd625535f210ab725f04
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57038312"
---
# <a name="add-search-to-an-aspnet-core-razor-pages-app"></a>Agregar búsqueda a una aplicación de Razor Pages de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este documento se agrega a la página de índice una capacidad de búsqueda que permite buscar películas por *género* o *nombre*.

Actualice el método `OnGetAsync` de la página de índice con el código siguiente:

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

La primera línea del método `OnGetAsync` crea una consulta [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) para seleccionar las películas:

```csharp
var movies = from m in _context.Movie
             select m;
```

En este momento *solo* se define la consulta, **no** se ejecuta en la base de datos.

Si el parámetro `searchString` contiene una cadena, la consulta de películas se modifica para filtrar según la cadena de búsqueda:

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

El código `s => s.Title.Contains()` es una [expresión lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Las lambdas se usan en consultas [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) basadas en métodos como argumentos para métodos de operador de consulta estándar, como el método [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) o `Contains` (usado en el código anterior). Las consultas LINQ no se ejecutan cuando se definen ni cuando se modifican mediante una llamada a un método (como `Where`, `Contains` u `OrderBy`). En su lugar, se aplaza la ejecución de la consulta. Esto significa que la evaluación de una expresión se aplaza hasta que su valor realizado se repite o se llama al método `ToListAsync`. Para más información, vea [Query Execution (Ejecución de consultas)](/dotnet/framework/data/adonet/ef/language-reference/query-execution).

**Nota:** El método [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) se ejecuta en la base de datos, no en el código de C#. La distinción entre mayúsculas y minúsculas en la consulta depende de la base de datos y la intercalación. En SQL Server, `Contains` se asigna a [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), que distingue entre mayúsculas y minúsculas. En SQLite, con la intercalación predeterminada, se distingue entre mayúsculas y minúsculas.

Vaya a la página de películas y anexe una cadena de consulta como `?searchString=Ghost` a la dirección URL (por ejemplo, `http://localhost:5000/Movies?searchString=Ghost`). Se muestran las películas filtradas.

![Vista de índice](../../tutorials/razor-pages/search/_static/ghost.png)

Si se agrega la siguiente plantilla de ruta a la página de índice, la cadena de búsqueda se puede pasar como un segmento de dirección URL (por ejemplo, `http://localhost:5000/Movies/ghost`).

```cshtml
@page "{searchString?}"
```

La restricción de ruta anterior permite buscar el título como datos de ruta (un segmento de dirección URL) en lugar de como un valor de cadena de consulta.  El elemento `?` de `"{searchString?}"` significa que se trata de un parámetro de ruta opcional.

![Vista de índice con la palabra Ghost (Fantasma) agregada a la dirección URL y una lista de películas devuelta con dos películas, Ghostbusters y Ghostbusters 2](../../tutorials/razor-pages/search/_static/g2.png)

Sin embargo, no se puede esperar que los usuarios modifiquen la dirección URL para buscar una película. En este paso, se agrega la interfaz de usuario para filtrar las películas. Si ha agregado la restricción de ruta `"{searchString?}"`, quítela.

Abra el archivo *Pages/Movies/Index.cshtml* y agregue el marcado `<form>` resaltado en el siguiente código:

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

La etiqueta HTML `<form>` usa el [asistente de etiquetas de formulario](xref:mvc/views/working-with-forms#the-form-tag-helper). Cuando se envía el formulario, la cadena de filtro se envía a la página *Pages/Movies/Index*. Guarde los cambios y pruebe el filtro.

![Vista de índice con la palabra Ghost (Fantasma) escrita en el cuadro de texto del filtro de título](../../tutorials/razor-pages/search/_static/filter.png)

## <a name="search-by-genre"></a>Búsqueda por género

Agregue las siguientes propiedades resaltadas a *Pages/Movies/Index.cshtml.cs*:

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

El elemento `SelectList Genres` contiene la lista de géneros. Esto permite al usuario seleccionar un género de la lista.

La propiedad `MovieGenre` contiene el género concreto que selecciona el usuario (por ejemplo, "Western").

Actualice el método `OnGetAsync` con el código siguiente:

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

El código siguiente es una consulta LINQ que recupera todos los géneros de la base de datos.

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

La `SelectList` de géneros se crea mediante la proyección de los distintos géneros.

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a>Adición de búsqueda por género

Actualice *Index.cshtml* como se indica a continuación:

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

Pruebe la aplicación al buscar por género, título de la película y ambos.
