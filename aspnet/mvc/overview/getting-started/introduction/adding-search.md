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
ms.openlocfilehash: f6d6d32a648fed453be924790a1b55698c9cf209
ms.sourcegitcommit: 0d583ed9253103f3e50b6d729276e667591cdd41
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2020
ms.locfileid: "86211480"
---
# <a name="search"></a>Search

[!INCLUDE [Tutorial Note](index.md)]

## <a name="adding-a-search-method-and-search-view"></a>Agregar un método de búsqueda y una vista de búsqueda

En esta sección, agregará la funcionalidad de búsqueda al `Index` método de acción que le permite buscar películas por género o nombre.

## <a name="prerequisites"></a>Requisitos previos

Para que coincida con las capturas de pantallas de esta sección, debe ejecutar la aplicación (F5) y agregar las siguientes películas a la base de datos.

| Título | Fecha de la versión | Género | Precio |
| ----- | ------------ | ----- | ----- |
| Ghostbusters | 6/8/1984 | Comedia | 6,99 |
| Ghostbusters II | 6/16/1989 | Comedia | 6,99 |
| Planeta del Apes | 3/27/1986 | Acción | 5,99 |

## <a name="updating-the-index-form"></a>Actualizar el formulario de índice

Empiece por actualizar el `Index` método de acción a la `MoviesController` clase existente. Este es el código:

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

La primera línea del `Index` método crea la siguiente consulta [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) para seleccionar las películas:

[!code-csharp[Main](adding-search/samples/sample2.cs)]

La consulta se define en este punto, pero aún no se ha ejecutado en la base de datos.

Si el `searchString` parámetro contiene una cadena, la consulta de películas se modifica para filtrar según el valor de la cadena de búsqueda, mediante el código siguiente:

[!code-csharp[Main](adding-search/samples/sample3.cs)]

El código `s => s.Title` anterior es una [expresión Lambda](https://msdn.microsoft.com/library/bb397687.aspx). Las lambdas se usan en consultas [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) basadas en métodos como argumentos para métodos de operador de consulta estándar, como el método [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) usado en el código anterior. Las consultas LINQ no se ejecutan cuando se definen o cuando se modifican llamando a un método como `Where` o `OrderBy` . En su lugar, se aplaza la ejecución de la consulta, lo que significa que la evaluación de una expresión se retrasa hasta que su valor realizado se recorra en iteración o [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) se llame al método. En el `Search` ejemplo, la consulta se ejecuta en la vista *index. cshtml* . Para más información sobre la ejecución de consultas en diferido, vea [Ejecución de la consulta](https://msdn.microsoft.com/library/bb738633.aspx).

> [!NOTE]
> El método [Contains](https://msdn.microsoft.com/library/bb155125.aspx) se ejecuta en la base de datos, no en el código de c# anterior. En la base de datos, [contiene](https://msdn.microsoft.com/library/bb155125.aspx) asignaciones a [SQL like](https://msdn.microsoft.com/library/ms179859.aspx), que distingue entre mayúsculas y minúsculas.

Ahora puede actualizar la `Index` vista que mostrará el formulario al usuario.

Ejecute la aplicación y vaya a */Movies/index*. Anexe una cadena de consulta como `?searchString=ghost` a la dirección URL. Se muestran las películas filtradas.

![SearchQryStr](adding-search/_static/image1.png)

Si cambia la firma del `Index` método para que tenga un parámetro denominado `id` , el `id` parámetro coincidirá con el `{id}` marcador de posición de las rutas predeterminadas establecidas en el archivo * \_ Start\RouteConfig.CS* de la aplicación.

[!code-json[Main](adding-search/samples/sample4.json)]

El método original tiene el siguiente `Index` aspecto::

[!code-csharp[Main](adding-search/samples/sample5.cs)]

El `Index` método modificado tendría el siguiente aspecto:

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

Ahora puede pasar el título de la búsqueda como datos de ruta (un segmento de dirección URL) en lugar de como un valor de cadena de consulta.

![](adding-search/_static/image2.png)

Sin embargo, no se puede esperar que los usuarios modifiquen la dirección URL cada vez que quieran buscar una película. Por lo tanto, ahora deberá agregar la interfaz de usuario con la que podrán filtrar las películas. Si ha cambiado la firma del `Index` método para probar cómo pasar el parámetro de identificador enlazado a ruta, cámbielo de nuevo para que el `Index` método tome un parámetro de cadena denominado `searchString` :

[!code-csharp[Main](adding-search/samples/sample7.cs)]

Abra el archivo *Views\Movies\Index.cshtml* y, justo después `@Html.ActionLink("Create New", "Create")` , agregue el marcado de formulario resaltado a continuación:

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

El `Html.BeginForm` ayudante crea una etiqueta de apertura `<form>` . El `Html.BeginForm` ayudante hace que el formulario se publique en sí mismo cuando el usuario envía el formulario haciendo clic en el botón **filtro** .

Visual Studio 2013 tiene una mejora agradable al mostrar y editar archivos de vista. Al ejecutar la aplicación con un archivo de vista abierto, Visual Studio 2013 invoca el método de acción de controlador correcto para mostrar la vista.

![](adding-search/_static/image3.png)

Con la vista de índice abierta en Visual Studio (como se muestra en la imagen anterior), pulse Ctr F5 o F5 para ejecutar la aplicación y, a continuación, intente buscar una película.

![](adding-search/_static/image4.png)

No hay ninguna `HttpPost` sobrecarga del `Index` método. No es necesario porque el método no cambia el estado de la aplicación, simplemente filtrando los datos.

Después, puede agregar el método `HttpPost Index` siguiente. En ese caso, el invocador de acción coincidiría con el `HttpPost Index` método y el `HttpPost Index` método se ejecutaría tal y como se muestra en la imagen siguiente.

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

Sin embargo, aunque agregue esta versión de `HttpPost` al método `Index`, hay una limitación en cómo se ha implementado todo esto. Supongamos que quiere marcar una búsqueda en particular o que quiere enviar un vínculo a sus amigos donde puedan hacer clic para ver la misma lista filtrada de películas. Tenga en cuenta que la dirección URL de la solicitud HTTP POST es la misma que la dirección URL de la solicitud GET (localhost: xxxxx/Movies/index) (no hay información de búsqueda en la propia dirección URL). En este momento, la información de la cadena de búsqueda se envía al servidor como un valor de campo de formulario. Esto significa que no se puede capturar esa información de búsqueda para marcar o enviar a amigos en una dirección URL.

La solución consiste en usar una sobrecarga de `BeginForm` que especifique que la solicitud post debe agregar la información de búsqueda a la dirección URL y que se debe enrutar a la `HttpGet` versión del `Index` método. Reemplace el método sin parámetros existente `BeginForm` por el marcado siguiente:

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

Ahora, cuando se envía una búsqueda, la dirección URL contiene una cadena de consulta de búsqueda. La búsqueda también será dirigida al método de acción `HttpGet Index`, aunque tenga un método `HttpPost Index`.

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>Agregar búsqueda por género

Si agregó la `HttpPost` versión del `Index` método, elimínela ahora.

A continuación, agregará una característica para permitir que los usuarios busquen películas por género. Reemplace el método `Index` por el código siguiente:

[!code-csharp[Main](adding-search/samples/sample11.cs)]

Esta versión del `Index` método toma un parámetro adicional, es decir, `movieGenre` . Las primeras líneas de código crean un `List` objeto para contener géneros de película de la base de datos.

El código siguiente es una consulta LINQ que recupera todos los géneros de la base de datos.

[!code-csharp[Main](adding-search/samples/sample12.cs)]

El código usa el `AddRange` método de la `List` colección genérica para agregar todos los géneros distintos a la lista. (Sin el `Distinct` modificador, se agregarán géneros duplicados; por ejemplo, comedia se agregaría dos veces en nuestro ejemplo). A continuación, el código almacena la lista de géneros en el `ViewBag.MovieGenre` objeto. Almacenar los datos de la categoría (como un género de la película) como un objeto [SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx) en un `ViewBag` y, después, tener acceso a los datos de la categoría en un cuadro de lista desplegable es un enfoque típico para las aplicaciones MVC.

En el código siguiente se muestra cómo comprobar el `movieGenre` parámetro. Si no está vacío, el código restringe aún más la consulta de películas para limitar las películas seleccionadas al género especificado.

[!code-csharp[Main](adding-search/samples/sample13.cs)]

Como se indicó anteriormente, la consulta no se ejecuta en la base de datos hasta que la lista de películas se recorre en iteración (lo que sucede en la vista, después de que el `Index` método de acción vuelva).

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>Agregar marcado a la vista de índice para admitir la búsqueda por género

Agregue una `Html.DropDownList` aplicación auxiliar al archivo *Views\Movies\Index.cshtml* , justo antes de la `TextBox` aplicación auxiliar. A continuación se muestra el marcado completado:

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

En el código siguiente:

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

El parámetro "MovieGenre" proporciona la clave para `DropDownList` que el ayudante busque un `IEnumerable<SelectListItem>` en `ViewBag` . `ViewBag`Se rellenó en el método de acción:

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

El parámetro "All" proporciona una etiqueta de opción. Si inspecciona esa opción en el explorador, verá que el atributo "valor" está vacío. Puesto que el controlador solo filtra `if` la cadena no es `null` o está vacía, el envío de un valor vacío para `movieGenre` muestra todos los géneros.

También puede establecer una opción para que se seleccione de forma predeterminada. Si desease "comedia" como opción predeterminada, cambiaría el código del controlador de la manera siguiente:

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

Ejecute la aplicación y vaya a */Movies/index*. Pruebe una búsqueda por género, por nombre de película y por ambos criterios.

![](adding-search/_static/image8.png)

En esta sección se ha creado un método de acción de búsqueda y una vista que permiten a los usuarios buscar por título y género de la película. En la siguiente sección, veremos cómo agregar una propiedad al `Movie` modelo y cómo agregar un inicializador que creará automáticamente una base de datos de prueba.

> [!div class="step-by-step"]
> [Anterior](examining-the-edit-methods-and-edit-view.md)
> [Siguiente](adding-a-new-field.md)
