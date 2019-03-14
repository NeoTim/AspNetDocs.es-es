---
ms.openlocfilehash: 984edac5f093d59bd5e1d826df8f32fda89f9e46
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57024582"
---
# <a name="work-with-sqlite-in-an-aspnet-core-mvc-app"></a>Trabajar con SQLite en una aplicación de ASP.NET Core MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

El objeto `MvcMovieContext` controla la tarea de conexión a la base de datos y asignación de objetos `Movie` a los registros de la base de datos. El contexto de base de datos se registra con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection) en el método `ConfigureServices` del archivo *Startup.cs*:

[!code-csharp[](~/tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a>SQLite

Según la información del sitio web de [SQLite](https://www.sqlite.org/):

> SQLite es un motor de base de datos SQL independiente, de alta confiabilidad, insertado, con características completas y dominio público. SQLite es el motor de base de datos más usado en el mundo.

Existen muchas herramientas de terceros que se pueden descargar para administrar y ver una base de datos de SQLite. La imagen de abajo es de [DB Browser for SQLite](http://sqlitebrowser.org/). Si tiene una herramienta favorita de SQLite, deje un comentario sobre lo que le gusta de ella.

![DB Browser for SQLite mostrando una base de datos de películas](~/tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a>Inicializar la base de datos

Cree una nueva clase denominada `SeedData` en la carpeta *Models*. Reemplace el código generado con el siguiente:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

Si hay alguna película en la base de datos, se devuelve el inicializador.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Agregar el inicializador

Agregue el inicializador al método `Main` en el archivo *Program.cs*:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]

::: moniker-end

### <a name="test-the-app"></a>Prueba de la aplicación

Elimine todos los registros de la base de datos (para que se ejecute el método de inicialización). Detenga e inicie la aplicación para inicializar la base de datos.
   
La aplicación muestra los datos inicializados.

![Explorador abierto con la aplicación MVC Movie donde se muestran los datos de películas](~/tutorials/first-mvc-app/working-with-sql/_static/m55.png)
