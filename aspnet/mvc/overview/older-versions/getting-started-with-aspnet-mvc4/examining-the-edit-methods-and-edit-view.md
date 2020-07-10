---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
title: Examinando los métodos de edición y la vista de edición | Microsoft Docs
author: Rick-Anderson
description: 'Nota: hay disponible una versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 41eb99ca-e88f-4720-ae6d-49a958da8116
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 85ad9a5758d70b5fe4ed792efb80217d7b3e2132
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "86163058"
---
# <a name="examining-the-edit-methods-and-edit-view"></a>Examinar los métodos y la vista Edit

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > [Hay disponible una](../../getting-started/introduction/getting-started.md) versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demuestra más características.

En esta sección, examinará las vistas y los métodos de acción generados para el controlador de película. A continuación, agregará una página de búsqueda personalizada.

Ejecute la aplicación y vaya al `Movies` controlador anexando */Movies* a la dirección URL en la barra de direcciones del explorador. Mantenga el puntero del mouse sobre un vínculo de **edición** para ver la dirección URL a la que está vinculado.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

El **Edit** método genera el vínculo de edición `Html.ActionLink` en la vista *Views\Movies\Index.cshtml* :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

![HTML. ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

El `Html` objeto es una aplicación auxiliar que se expone mediante una propiedad en la clase base [System. Web. Mvc. WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) . El `ActionLink` método del ayudante facilita la generación dinámica de hipervínculos HTML que se vinculan a los métodos de acción en los controladores. El primer argumento del `ActionLink` método es el texto del vínculo que se va a representar (por ejemplo, `<a>Edit Me</a>` ). El segundo argumento es el nombre del método de acción que se va a invocar. El último argumento es un [objeto anónimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) que genera los datos de la ruta (en este caso, el identificador 4).

El vínculo generado que se muestra en la imagen anterior es `http://localhost:xxxxx/Movies/Edit/4` . La ruta predeterminada (establecida en *App \_ Start\RouteConfig.CS*) toma el patrón de la dirección URL `{controller}/{action}/{id}` . Por lo tanto, ASP.NET `http://localhost:xxxxx/Movies/Edit/4` se traduce en una solicitud al `Edit` método de acción del `Movies` controlador con el parámetro `ID` igual a 4. Examine el siguiente código del archivo * \_ Start\RouteConfig.CS* de la aplicación.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

También puede pasar parámetros de método de acción mediante una cadena de consulta. Por ejemplo, la dirección URL `http://localhost:xxxxx/Movies/Edit?ID=4` también pasa el parámetro `ID` de 4 al `Edit` método de acción del `Movies` controlador.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Abra el `Movies` controlador. `Edit`A continuación se muestran los dos métodos de acción.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cs)]

Observe que el segundo método de acción `Edit` va precedido del atributo `HttpPost`. Este atributo especifica que `Edit` solo se puede invocar la sobrecarga del método para las solicitudes post. Podría aplicar el `HttpGet` atributo al primer método de edición, pero no es necesario porque es el valor predeterminado. (Haremos referencia a los métodos de acción a los que se asignan implícitamente el `HttpGet` atributo como `HttpGet` métodos).

El `HttpGet` `Edit` método toma el parámetro de ID. de película, busca la película con el `Find` método Entity Framework y devuelve la película seleccionada a la vista de edición. El parámetro ID especifica un [valor predeterminado](https://msdn.microsoft.com/library/dd264739.aspx) de cero si `Edit` se llama al método sin un parámetro. Si no se encuentra una película, se devuelve [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) . Cuando el sistema de scaffolding creó la vista de edición, examinó la clase `Movie` y creó código para representar los elementos `<label>` y `<input>` para cada propiedad de la clase. En el ejemplo siguiente se muestra la vista de edición que se generó:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cshtml)]

Observe cómo la plantilla de vista tiene una `@model MvcMovie.Models.Movie` instrucción en la parte superior del archivo; esto especifica que la vista espera que el modelo de la plantilla de vista sea de tipo `Movie` .

El código con scaffolding usa varios *métodos auxiliares* para optimizar el marcado HTML. El [`Html.LabelFor`](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) ayudante muestra el nombre del campo ( &quot; title &quot; , &quot; ReleaseDate &quot; , &quot; Genre &quot; o &quot; Price &quot; ). El [`Html.EditorFor`](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) ayudante representa un `<input>` elemento HTML. El [`Html.ValidationMessageFor`](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) ayudante muestra cualquier mensaje de validación asociado a esa propiedad.

Ejecute la aplicación y vaya a la dirección URL de */Movies* . Haga clic en un vínculo **Edit** (Editar). En el explorador, vea el código fuente de la página. A continuación se muestra el código HTML del elemento Form.

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html?highlight=7,10-11)]

Los `<input>` elementos están en un `<form>` elemento HTML cuyo `action` atributo está establecido en post en la dirección URL de */Movies/Edit* . Los datos del formulario se publicarán en el servidor cuando se haga clic en el botón **Editar** .

## <a name="processing-the-post-request"></a>Procesamiento de la solicitud POST

En la siguiente lista se muestra la versión `HttpPost` del método de acción `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

El [enlazador de modelos MVC de ASP.net](https://msdn.microsoft.com/magazine/hh781022.aspx) toma los valores del formulario expuesto y crea un `Movie` objeto que se pasa como `movie` parámetro. El método `ModelState.IsValid` comprueba que los datos presentados en el formulario pueden usarse para modificar (editar o actualizar) un objeto `Movie`. Si los datos son válidos, los datos de la película se guardan en la `Movies` colección de la `db(MovieDBContext` instancia). Los nuevos datos de la película se guardan en la base de datos llamando al `SaveChanges` método de `MovieDBContext` . Después de guardar los datos, el código redirige al usuario al `Index` método de acción de la `MoviesController` clase, que muestra la colección de películas, incluidos los cambios que se han realizado.

Si los valores expuestos no son válidos, se vuelven a mostrar en el formulario. Las `Html.ValidationMessageFor` aplicaciones auxiliares de la plantilla de vista *Edit. cshtml* se encargan de mostrar los mensajes de error correspondientes.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

> [!NOTE]
> para admitir la validación de jQuery para configuraciones regionales distintas del inglés que usan una coma ( &quot; , &quot; ) para un separador decimal, debe incluir *globalize.js* y el archivo de *globalize.cultures.jso referencia cultural* específico (de [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) y JavaScript que se va a usar `Globalize.parseFloat` . En el código siguiente se muestran las modificaciones en el archivo Views\Movies\Edit.cshtml para trabajar con la &quot; referencia cultural fr-FR &quot; :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

El campo decimal puede requerir una coma, no un separador decimal. Como solución temporal, puede Agregar el elemento de globalización al archivo web.config raíz del proyecto. En el código siguiente se muestra el elemento Globalization con la referencia cultural establecida en Estados Unidos English.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.xml)]

Todos los `HttpGet` métodos siguen un patrón similar. Obtienen un objeto de película (o una lista de objetos, en el caso de `Index` ) y pasan el modelo a la vista. El `Create` método pasa un objeto de película vacío a la vista de creación. Todos los métodos que crean, editan, eliminan o modifican los datos lo hacen en la sobrecarga `HttpPost` del método. La modificación de los datos en un método HTTP GET es un riesgo para la seguridad, como se describe en la entrada de blog de [ASP.NET MVC Tip #46 – no usar los vínculos de eliminación porque crean carencias de seguridad](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). La modificación de los datos en un método GET también infringe los procedimientos recomendados de HTTP y el patrón [Rest](http://en.wikipedia.org/wiki/Representational_State_Transfer) de la arquitectura, que especifica que las solicitudes GET no deben cambiar el estado de la aplicación. En otras palabras, realizar una operación GET debería ser una operación segura sin efectos secundarios, que no modifica los datos persistentes.

## <a name="adding-a-search-method-and-search-view"></a>Agregar un método de búsqueda y una vista de búsqueda

En esta sección, agregará un `SearchIndex` método de acción que le permitirá buscar películas por género o nombre. Estará disponible mediante la dirección URL de */Movies/SearchIndex* . La solicitud mostrará un formulario HTML que contiene los elementos de entrada que un usuario puede escribir para buscar una película. Cuando un usuario envía el formulario, el método de acción obtendrá los valores de búsqueda publicados por el usuario y usará los valores para buscar en la base de datos.

## <a name="displaying-the-searchindex-form"></a>Mostrar el formulario SearchIndex

Comience agregando un `SearchIndex` método de acción a la `MoviesController` clase existente. El método devolverá una vista que contenga un formulario HTML. Este es el código:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

La primera línea del `SearchIndex` método crea la siguiente consulta [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) para seleccionar las películas:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cs)]

La consulta se define en este punto, pero aún no se ha ejecutado en el almacén de datos.

Si el `searchString` parámetro contiene una cadena, la consulta de películas se modifica para filtrar según el valor de la cadena de búsqueda, mediante el código siguiente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample11.cs)]

El código `s => s.Title` anterior es una [expresión Lambda](https://msdn.microsoft.com/library/bb397687.aspx). Las lambdas se usan en consultas [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) basadas en métodos como argumentos para métodos de operador de consulta estándar, como el método [Where](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) usado en el código anterior. Las consultas LINQ no se ejecutan cuando se definen o cuando se modifican llamando a un método como `Where` o `OrderBy` . En su lugar, se aplaza la ejecución de la consulta, lo que significa que la evaluación de una expresión se retrasa hasta que su valor realizado se recorra en iteración o [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) se llame al método. En el `SearchIndex` ejemplo, la consulta se ejecuta en la vista SearchIndex. Para más información sobre la ejecución de consultas en diferido, vea [Ejecución de la consulta](https://msdn.microsoft.com/library/bb738633.aspx).

Ahora puede implementar la `SearchIndex` vista que mostrará el formulario al usuario. Haga clic con el botón secundario en el `SearchIndex` método y, a continuación, haga clic en **Agregar vista**. En el cuadro de diálogo **Agregar vista** , especifique que va a pasar un `Movie` objeto a la plantilla de vista como su clase de modelo. En la lista de **plantillas scaffolding** , elija **lista**y, a continuación, haga clic en **Agregar**.

![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image5.png)

Al hacer clic en el botón **Agregar** , se crea la plantilla de vista *Views\Movies\SearchIndex.cshtml* . Dado que seleccionó **lista** en la lista de **plantillas scaffolding** , Visual Studio generará automáticamente (scaffolding) algún marcado predeterminado en la vista. El scaffolding creó un formulario HTML. Examinó la `Movie` clase y creó código para representar `<label>` elementos para cada propiedad de la clase. En la siguiente lista se muestra la vista de creación que se generó:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cshtml)]

Ejecute la aplicación y vaya a */Movies/SearchIndex*. Anexe una cadena de consulta como `?searchString=ghost` a la dirección URL. Se muestran las películas filtradas.

![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image6.png)

Si cambia la firma del `SearchIndex` método para que tenga un parámetro denominado `id` , el `id` parámetro coincidirá con el `{id}` marcador de posición de las rutas predeterminadas establecidas en el archivo *global. asax* .

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample13.json)]

El método original tiene el siguiente `SearchIndex` aspecto::

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cs)]

El `SearchIndex` método modificado tendría el siguiente aspecto:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cs?highlight=1,3)]

Ahora puede pasar el título de la búsqueda como datos de ruta (un segmento de dirección URL) en lugar de como un valor de cadena de consulta.

![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image7.png)

Sin embargo, no se puede esperar que los usuarios modifiquen la dirección URL cada vez que quieran buscar una película. Ahora va a agregar una interfaz de usuario que le ayude a filtrar las películas. Si ha cambiado la firma del `SearchIndex` método para probar cómo pasar el parámetro de identificador enlazado a ruta, cámbielo de nuevo para que el `SearchIndex` método tome un parámetro de cadena denominado `searchString` :

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

Abra el archivo *Views\Movies\SearchIndex.cshtml* y, justo después `@Html.ActionLink("Create New", "Create")` , agregue lo siguiente:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml?highlight=2)]

En el ejemplo siguiente se muestra una parte del archivo *Views\Movies\SearchIndex.cshtml* con el marcado de filtrado agregado.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cshtml?highlight=12-15)]

El `Html.BeginForm` ayudante crea una etiqueta de apertura `<form>` . El `Html.BeginForm` ayudante hace que el formulario se publique en sí mismo cuando el usuario envía el formulario haciendo clic en el botón **filtro** .

Ejecute la aplicación e intente buscar una película.

![](examining-the-edit-methods-and-edit-view/_static/image8.png)

No hay ninguna `HttpPost` sobrecarga del `SearchIndex` método. No es necesario porque el método no cambia el estado de la aplicación, simplemente filtrando los datos.

Después, puede agregar el método `HttpPost SearchIndex` siguiente. En ese caso, el invocador de acción coincidiría con el `HttpPost SearchIndex` método y el `HttpPost SearchIndex` método se ejecutaría tal y como se muestra en la imagen siguiente.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image9.png)

Sin embargo, aunque agregue esta versión de `HttpPost` al método `SearchIndex`, hay una limitación en cómo se ha implementado todo esto. Supongamos que quiere marcar una búsqueda en particular o que quiere enviar un vínculo a sus amigos donde puedan hacer clic para ver la misma lista filtrada de películas. Tenga en cuenta que la dirección URL de la solicitud HTTP POST es la misma que la dirección URL de la solicitud GET (localhost: xxxxx/Movies/SearchIndex), no hay información de búsqueda en la propia dirección URL. En este momento, la información de la cadena de búsqueda se envía al servidor como un valor de campo de formulario. Esto significa que no se puede capturar esa información de búsqueda para marcar o enviar a amigos en una dirección URL.

La solución consiste en usar una sobrecarga de `BeginForm` que especifique que la solicitud post debe agregar la información de búsqueda a la dirección URL y que se debe enrutar a la versión HttpGet del `SearchIndex` método. Reemplace el método sin parámetros existente `BeginForm` por lo siguiente:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cshtml)]

![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

Ahora, cuando se envía una búsqueda, la dirección URL contiene una cadena de consulta de búsqueda. La búsqueda también será dirigida al método de acción `HttpGet SearchIndex`, aunque tenga un método `HttpPost SearchIndex`.

![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image11.png)

## <a name="adding-search-by-genre"></a>Agregar búsqueda por género

Si agregó la `HttpPost` versión del `SearchIndex` método, elimínela ahora.

A continuación, agregará una característica para permitir que los usuarios busquen películas por género. Reemplace el método `SearchIndex` por el código siguiente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cs)]

Esta versión del `SearchIndex` método toma un parámetro adicional, es decir, `movieGenre` . Las primeras líneas de código crean un `List` objeto para contener géneros de película de la base de datos.

El código siguiente es una consulta LINQ que recupera todos los géneros de la base de datos.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample22.cs)]

El código usa el `AddRange` método de la `List` colección genérica para agregar todos los géneros distintos a la lista. (Sin el `Distinct` modificador, se agregarán géneros duplicados; por ejemplo, comedia se agregaría dos veces en nuestro ejemplo). A continuación, el código almacena la lista de géneros en el `ViewBag` objeto.

En el código siguiente se muestra cómo comprobar el `movieGenre` parámetro. Si no está vacío, el código restringe aún más la consulta de películas para limitar las películas seleccionadas al género especificado.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample23.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Agregar marcado a la vista SearchIndex para admitir la búsqueda por género

Agregue una `Html.DropDownList` aplicación auxiliar al archivo *Views\Movies\SearchIndex.cshtml* , justo antes de la `TextBox` aplicación auxiliar. A continuación se muestra el marcado completado:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample24.cshtml?highlight=4)]

Ejecute la aplicación y vaya a */Movies/SearchIndex*. Pruebe una búsqueda por género, por nombre de película y por ambos criterios.

![](examining-the-edit-methods-and-edit-view/_static/image12.png)

En esta sección se han examinado los métodos de acción CRUD y las vistas generadas por el marco. Ha creado un método de acción de búsqueda y una vista que permiten a los usuarios buscar por título y género de la película. En la siguiente sección, veremos cómo agregar una propiedad al `Movie` modelo y cómo agregar un inicializador que creará automáticamente una base de datos de prueba.

> [!div class="step-by-step"]
> [Anterior](accessing-your-models-data-from-a-controller.md)
> [Siguiente](adding-a-new-field-to-the-movie-model-and-table.md)
