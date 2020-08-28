---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
title: Examinar los métodos de edición y la vista de edición (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 5cb3c59b-1e96-464b-b3a8-c55607201872
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: fc4d59323ab3cc1e4a454676a0ec498f3c5246fd
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89044602"
---
# <a name="examining-the-edit-methods-and-edit-view-vb"></a>Examinar los métodos y la vista Edit (VB)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos que se enumeran a continuación. Para instalar todos ellos, haga clic en el vínculo siguiente: [instalador de plataforma web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos con los siguientes vínculos:
> 
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(compatibilidad con herramientas y tiempo de ejecución)
> 
> Si utiliza Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos haciendo clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con código fuente de VB.NET está disponible para acompañar este tema. [Descargue la versión de VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si prefiere C#, cambie a la [versión de c#](../cs/examining-the-edit-methods-and-edit-view.md) de este tutorial.

En esta sección, examinará las vistas y los métodos de acción generados para el controlador de película. A continuación, agregará una página de búsqueda personalizada.

Ejecute la aplicación y vaya al `Movies` controlador anexando */Movies* a la dirección URL en la barra de direcciones del explorador. Mantenga el puntero del mouse sobre un vínculo de **edición** para ver la dirección URL a la que está vinculado.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

El **Edit** método genera el vínculo de edición `Html.ActionLink` en la vista *Views\Movies\Index.vbhtml* :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image3.png)](examining-the-edit-methods-and-edit-view/_static/image2.png)

El `Html` objeto es una aplicación auxiliar que se expone mediante una propiedad en la `WebViewPage` clase base. El `ActionLink` método del ayudante facilita la generación dinámica de hipervínculos HTML que se vinculan a los métodos de acción en los controladores. El primer argumento del `ActionLink` método es el texto del vínculo que se va a representar (por ejemplo, `<a>Edit Me</a>` ). El segundo argumento es el nombre del método de acción que se va a invocar. El último argumento es un [objeto anónimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) que genera los datos de la ruta (en este caso, el identificador 4).

El vínculo generado que se muestra en la imagen anterior es `http://localhost:xxxxx/Movies/Edit/4` . La ruta predeterminada toma el modelo de dirección URL `{controller}/{action}/{id}` . Por lo tanto, ASP.NET `http://localhost:xxxxx/Movies/Edit/4` se traduce en una solicitud al `Edit` método de acción del `Movies` controlador con el parámetro `ID` igual a 4.

También puede pasar parámetros de método de acción mediante una cadena de consulta. Por ejemplo, la dirección URL `http://localhost:xxxxx/Movies/Edit?ID=4` también pasa el parámetro `ID` de 4 al `Edit` método de acción del `Movies` controlador.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image5.png)](examining-the-edit-methods-and-edit-view/_static/image4.png)

Abra el `Movies` controlador. `Edit`A continuación se muestran los dos métodos de acción.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample3.vb)]

Observe que el segundo método de acción `Edit` va precedido del atributo `HttpPost`. Este atributo especifica que `Edit` solo se puede invocar la sobrecarga del método para las solicitudes post. Podría aplicar el `HttpGet` atributo al primer método de edición, pero no es necesario porque es el valor predeterminado. (Haremos referencia a los métodos de acción a los que se asignan implícitamente el `HttpGet` atributo como `HttpGet` métodos).

El `HttpGet` `Edit` método toma el parámetro de ID. de película, busca la película con el `Find` método Entity Framework y devuelve la película seleccionada a la vista de edición. Cuando el sistema de scaffolding creó la vista de edición, examinó la clase `Movie` y creó código para representar los elementos `<label>` y `<input>` para cada propiedad de la clase. En el ejemplo siguiente se muestra la vista de edición que se generó:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample4.vbhtml)]

Observe cómo la plantilla de vista tiene una `@ModelType MvcMovie.Models.Movie` instrucción en la parte superior del archivo; esto especifica que la vista espera que el modelo de la plantilla de vista sea de tipo `Movie` .

El código con scaffolding usa varios *métodos auxiliares* para optimizar el marcado HTML. El [`Html.LabelFor`](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) ayudante muestra el nombre del campo ( &quot; title &quot; , &quot; ReleaseDate &quot; , &quot; Genre &quot; o &quot; Price &quot; ). El [`Html.EditorFor`](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) ayudante muestra un elemento HTML `<input>` . El [`Html.ValidationMessageFor`](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) ayudante muestra cualquier mensaje de validación asociado a esa propiedad.

Ejecute la aplicación y vaya a la dirección URL de */Movies* . Haga clic en un vínculo **Edit** (Editar). En el explorador, vea el código fuente de la página. El código HTML de la página es similar al del ejemplo siguiente. (El marcado de menú se excluyó para mayor claridad).

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample5.html)]

Los `<input>` elementos están en un `<form>` elemento HTML cuyo `action` atributo está establecido en post en la dirección URL de */Movies/Edit* . Los datos del formulario se publicarán en el servidor cuando se haga clic en el botón **Editar** .

## <a name="processing-the-post-request"></a>Procesamiento de la solicitud POST

En la siguiente lista se muestra la versión `HttpPost` del método de acción `Edit`.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample6.vb)]

El enlazador de modelos de ASP.NET Framework toma los valores del formulario expuesto y crea un `Movie` objeto que se pasa como `movie` parámetro. La `ModelState.IsValid` protección del código comprueba que los datos enviados en el formulario se pueden usar para modificar un `Movie` objeto. Si los datos son válidos, el código guarda los datos de la película en la `Movies` colección de la `MovieDBContext` instancia. Después, el código guarda los nuevos datos de la película en la base de datos llamando al `SaveChanges` método de `MovieDBContext` , que conserva los cambios en la base de datos. Después de guardar los datos, el código redirige al usuario al `Index` método de acción de la `MoviesController` clase, lo que hace que la película actualizada se muestre en la lista de películas.

Si los valores expuestos no son válidos, se vuelven a mostrar en el formulario. Las `Html.ValidationMessageFor` aplicaciones auxiliares de la plantilla de vista *Edit. vbhtml* se encargan de mostrar los mensajes de error correspondientes.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image7.png)](examining-the-edit-methods-and-edit-view/_static/image6.png)

> **Nota acerca de las configuraciones regionales** Si trabaja normalmente con una configuración regional que no sea inglés, consulte [compatibilidad con la validación de ASP.NET MVC 3 con configuraciones regionales distintas del inglés.](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)

## <a name="making-the-edit-method-more-robust"></a>Aumentar la solidez del método de edición

El `HttpGet` `Edit` método generado por el sistema de scaffolding no comprueba que el identificador que se le pasa es válido. Si un usuario quita el segmento de identificador de la dirección URL ( `http://localhost:xxxxx/Movies/Edit` ), se muestra el siguiente error:

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image9.png)](examining-the-edit-methods-and-edit-view/_static/image8.png)

Un usuario también podría pasar un identificador que no existe en la base de datos, como `http://localhost:xxxxx/Movies/Edit/1234` . Puede realizar dos cambios en el `HttpGet` `Edit` método de acción para abordar esta limitación. En primer lugar, cambie el `ID` parámetro para que tenga un valor predeterminado de cero cuando no se pase explícitamente un identificador. También puede comprobar que el `Find` método realmente encontró una película antes de devolver el objeto de película a la plantilla de vista. `Edit`A continuación se muestra el método actualizado.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample7.vb)]

Si no se encuentra ninguna película, `HttpNotFound` se llama al método.

Todos los `HttpGet` métodos siguen un patrón similar. Obtienen un objeto de película (o una lista de objetos, en el caso de `Index` ) y pasan el modelo a la vista. El `Create` método pasa un objeto de película vacío a la vista de creación. Todos los métodos que crean, editan, eliminan o modifican los datos lo hacen en la sobrecarga `HttpPost` del método. La modificación de los datos en un método HTTP GET es un riesgo para la seguridad, como se describe en la entrada de blog de [ASP.NET MVC Tip #46 – no usar los vínculos de eliminación porque crean carencias de seguridad](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). La modificación de los datos en un método GET también infringe los procedimientos recomendados de HTTP y el patrón REST de la arquitectura, que especifica que las solicitudes GET no deben cambiar el estado de la aplicación. En otras palabras, realizar una operación GET debe ser una operación segura sin efectos secundarios.

## <a name="adding-a-search-method-and-search-view"></a>Agregar un método de búsqueda y una vista de búsqueda

En esta sección, agregará un `SearchIndex` método de acción que le permitirá buscar películas por género o nombre. Estará disponible mediante la dirección URL de */Movies/SearchIndex* . La solicitud mostrará un formulario HTML que contiene los elementos de entrada que un usuario puede rellenar para buscar una película. Cuando un usuario envía el formulario, el método de acción obtendrá los valores de búsqueda publicados por el usuario y usará los valores para buscar en la base de datos.

![SearchIndx_SM](examining-the-edit-methods-and-edit-view/_static/image10.png)

## <a name="displaying-the-searchindex-form"></a>Mostrar el formulario SearchIndex

Comience agregando un `SearchIndex` método de acción a la `MoviesController` clase existente. El método devolverá una vista que contenga un formulario HTML. Este es el código:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample8.vb)]

La primera línea del `SearchIndex` método crea la siguiente consulta [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) para seleccionar las películas:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample9.vb)]

La consulta se define en este punto, pero aún no se ha ejecutado en el almacén de datos.

Si el `searchString` parámetro contiene una cadena, la consulta de películas se modifica para filtrar según el valor de la cadena de búsqueda, mediante el código siguiente:

Si no es String. IsNullOrEmpty (searchString)   
 películas = películas. Where (Functions s. title. Contains (searchString))   
 Finalizar si

Las consultas LINQ no se ejecutan cuando se definen o cuando se modifican llamando a un método como `Where` o `OrderBy` . En su lugar, se aplaza la ejecución de la consulta, lo que significa que la evaluación de una expresión se retrasa hasta que su valor realizado se recorra en iteración o [`ToList`](https://msdn.microsoft.com/library/bb342261.aspx) se llame al método. En el `SearchIndex` ejemplo, la consulta se ejecuta en la vista SearchIndex. Para más información sobre la ejecución de consultas en diferido, vea [Ejecución de la consulta](https://msdn.microsoft.com/library/bb738633.aspx).

Ahora puede implementar la `SearchIndex` vista que mostrará el formulario al usuario. Haga clic con el botón secundario en el `SearchIndex` método y, a continuación, haga clic en **Agregar vista**. En el cuadro de diálogo **Agregar vista** , especifique que va a pasar un `Movie` objeto a la plantilla de vista como su clase de modelo. En la lista de **plantillas scaffolding** , elija **lista**y, a continuación, haga clic en **Agregar**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

Al hacer clic en el botón **Agregar** , se crea la plantilla de vista *Views\Movies\SearchIndex.vbhtml* . Dado que seleccionó **lista** en la lista de **plantillas scaffolding** , Visual Web Developer generará automáticamente (scaffolding) algún contenido predeterminado en la vista. El scaffolding creó un formulario HTML. Examinó la `Movie` clase y creó código para representar `<label>` elementos para cada propiedad de la clase. En la siguiente lista se muestra la vista de creación que se generó:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.vbhtml)]

Ejecute la aplicación y vaya a */Movies/SearchIndex*. Anexe una cadena de consulta como `?searchString=ghost` a la dirección URL. Se muestran las películas filtradas.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

Si cambia la firma del `SearchIndex` método para que tenga un parámetro denominado `id` , el `id` parámetro coincidirá con el `{id}` marcador de posición de las rutas predeterminadas establecidas en el archivo *global. asax* .

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.txt)]

El `SearchIndex` método modificado tendría el siguiente aspecto:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample12.vb)]

Ahora puede pasar el título de la búsqueda como datos de ruta (un segmento de dirección URL) en lugar de como un valor de cadena de consulta.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

Sin embargo, no se puede esperar que los usuarios modifiquen la dirección URL cada vez que quieran buscar una película. Ahora va a agregar una interfaz de usuario que le ayude a filtrar las películas. Si ha cambiado la firma del `SearchIndex` método para probar cómo pasar el parámetro de identificador enlazado a ruta, cámbielo de nuevo para que el `SearchIndex` método tome un parámetro de cadena denominado `searchString` :

Abra el archivo *Views\Movies\SearchIndex.vbhtml* y, justo después `@Html.ActionLink("Create New", "Create")` , agregue lo siguiente:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample13.vbhtml)]

El `Html.BeginForm` ayudante crea una etiqueta de apertura `<form>` . El `Html.BeginForm` ayudante hace que el formulario se publique en sí mismo cuando el usuario envía el formulario haciendo clic en el botón **filtro** .

Ejecute la aplicación e intente buscar una película.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

No hay ninguna `HttpPost` sobrecarga del `SearchIndex` método. No es necesario porque el método no cambia el estado de la aplicación, simplemente filtrando los datos. Si agregó el método siguiente `HttpPost` `SearchIndex` , el invocador de acción coincidiría con el `HttpPost` `SearchIndex` método y el `HttpPost` `SearchIndex` método se ejecutaría tal y como se muestra en la imagen siguiente.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample14.vb)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

## <a name="adding-search-by-genre"></a>Agregar búsqueda por género

Si agregó la `HttpPost` versión del `SearchIndex` método, elimínela ahora.

A continuación, agregará una característica para permitir que los usuarios busquen películas por género. Reemplace el método `SearchIndex` por el código siguiente:

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample15.vb)]

Esta versión del `SearchIndex` método toma un parámetro adicional, es decir, `movieGenre` . Las primeras líneas de código crean un `List` objeto para contener géneros de película de la base de datos.

El código siguiente es una consulta LINQ que recupera todos los géneros de la base de datos.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample16.vb)]

El código usa el `AddRange` método de la `List` colección genérica para agregar todos los géneros distintos a la lista. (Sin el `Distinct` modificador, se agregarán géneros duplicados; por ejemplo, comedia se agregaría dos veces en nuestro ejemplo). A continuación, el código almacena la lista de géneros en el `ViewBag` objeto.

En el código siguiente se muestra cómo comprobar el `movieGenre` parámetro. Si no está vacío, el código restringe aún más la consulta de películas para limitar las películas seleccionadas al género especificado.

[!code-vb[Main](examining-the-edit-methods-and-edit-view/samples/sample17.vb)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Agregar marcado a la vista SearchIndex para admitir la búsqueda por género

Agregue una `Html.DropDownList` aplicación auxiliar al archivo *Views\Movies\SearchIndex.vbhtml* , justo antes de la `TextBox` aplicación auxiliar. A continuación se muestra el marcado completado:

[!code-vbhtml[Main](examining-the-edit-methods-and-edit-view/samples/sample18.vbhtml)]

Ejecute la aplicación y vaya a */Movies/SearchIndex*. Pruebe una búsqueda por género, por nombre de película y por ambos criterios.

En esta sección se han examinado los métodos de acción CRUD y las vistas generadas por el marco. Ha creado un método de acción de búsqueda y una vista que permiten a los usuarios buscar por título y género de la película. En la siguiente sección, veremos cómo agregar una propiedad al `Movie` modelo y cómo agregar un inicializador que creará automáticamente una base de datos de prueba.

> [!div class="step-by-step"]
> [Anterior](accessing-your-models-data-from-a-controller.md)
> [Siguiente](adding-a-new-field.md)
