---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: Examinando los métodos de edición y la vista de edición | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/06/2019
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 1894971430c4febee19f095373411ed319edc015
ms.sourcegitcommit: 4e6d586faadbe4d9ef27122f86335ec9385134af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89045213"
---
# <a name="examining-the-edit-methods-and-edit-view"></a>Examinar los métodos y la vista Edit

por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

En esta sección, examinará las `Edit` vistas y los métodos de acción generados para el controlador de película. Pero primero vamos a tomar una breve versión para que la fecha de lanzamiento tenga un aspecto mejor. Abra el archivo *Models\Movie.CS* y agregue las líneas resaltadas que se muestran a continuación:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

También puede hacer que la referencia cultural de fecha sea específica de la siguiente manera:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

En el próximo tutorial hablaremos de [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx). El atributo [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) especifica qué se muestra como nombre de un campo (en este caso, "Release Date" en lugar de "ReleaseDate"). El atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) especifica el tipo de los datos, en este caso es una fecha, por lo que no se muestra la información de hora almacenada en el campo. El atributo [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) es necesario para un error en el explorador Chrome que presenta los formatos de fecha incorrectamente.

Ejecute la aplicación y vaya al `Movies` controlador. Mantenga el puntero del mouse sobre un vínculo de **edición** para ver la dirección URL a la que está vinculado.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

El **Edit** método genera el vínculo de edición `Html.ActionLink` en la vista *Views\Movies\Index.cshtml* :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![HTML. ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

El `Html` objeto es una aplicación auxiliar que se expone mediante una propiedad en la clase base [System. Web. Mvc. WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) . El `ActionLink` método del ayudante facilita la generación dinámica de hipervínculos HTML que se vinculan a los métodos de acción en los controladores. El primer argumento del `ActionLink` método es el texto del vínculo que se va a representar (por ejemplo, `<a>Edit Me</a>` ). El segundo argumento es el nombre del método de acción que se va a invocar (en este caso, la `Edit` acción). El último argumento es un [objeto anónimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) que genera los datos de la ruta (en este caso, el identificador 4).

El vínculo generado que se muestra en la imagen anterior es `http://localhost:1234/Movies/Edit/4` . La ruta predeterminada (establecida en *App \_ Start\RouteConfig.CS*) toma el patrón de la dirección URL `{controller}/{action}/{id}` . Por lo tanto, ASP.NET `http://localhost:1234/Movies/Edit/4` se traduce en una solicitud al `Edit` método de acción del `Movies` controlador con el parámetro `ID` igual a 4. Examine el siguiente código del archivo * \_ Start\RouteConfig.CS* de la aplicación. El método [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) se usa para enrutar las solicitudes HTTP al controlador y el método de acción correctos y proporcionar el parámetro de identificador opcional. El método [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) también lo usa el [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) como `ActionLink` para generar direcciones URL según el controlador, el método de acción y los datos de ruta.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

También puede pasar parámetros de método de acción mediante una cadena de consulta. Por ejemplo, la dirección URL `http://localhost:1234/Movies/Edit?ID=3` también pasa el parámetro `ID` de 3 al `Edit` método de acción del `Movies` controlador.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Abra el `Movies` controlador. `Edit`A continuación se muestran los dos métodos de acción.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

Observe que el segundo método de acción `Edit` va precedido del atributo `HttpPost`. Este atributo especifica que la sobrecarga del `Edit` método solo se puede invocar para las solicitudes post. Podría aplicar el `HttpGet` atributo al primer método de edición, pero no es necesario porque es el valor predeterminado. (Haremos referencia a los métodos de acción a los que se asignan implícitamente el `HttpGet` atributo como `HttpGet` métodos). El atributo de [enlace](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) es otro mecanismo de seguridad importante que evita que los hackers envíen datos por encima del modelo. Solo debe incluir propiedades en el atributo de enlace que desee cambiar. Puede leer acerca de la publicación en la publicación y el atributo de enlace en la nota de seguridad que se está [sobreexpuesto](https://go.microsoft.com/fwlink/?LinkId=317598). En el modelo simple que se usa en este tutorial, se enlazarán todos los datos del modelo. El atributo [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) se usa para impedir la falsificación de una solicitud y se empareja con `@Html.AntiForgeryToken()` en el archivo de vista de edición (*Views\Movies\Edit.cshtml*); a continuación, se muestra una parte:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` genera un token antifalsificación de formulario oculto que debe coincidir en el `Edit` método del `Movies` controlador. Puede leer más sobre la falsificación de solicitudes entre sitios (también conocido como XSRF o CSRF) en mi tutorial [prevención de XSRF/CSRF en MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

El `HttpGet` `Edit` método toma el parámetro de ID. de película, busca la película con el `Find` método Entity Framework y devuelve la película seleccionada a la vista de edición. Si no se encuentra una película, se devuelve [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) . Cuando el sistema de scaffolding creó la vista de edición, examinó la clase `Movie` y creó código para representar los elementos `<label>` y `<input>` para cada propiedad de la clase. En el ejemplo siguiente se muestra la vista de edición generada por el sistema de scaffolding de Visual Studio:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Observe cómo la plantilla de vista tiene una `@model MvcMovie.Models.Movie` instrucción en la parte superior del archivo; esto especifica que la vista espera que el modelo de la plantilla de vista sea de tipo `Movie` .

El código con scaffolding usa varios *métodos auxiliares* para optimizar el marcado HTML. El [`Html.LabelFor`](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) ayudante muestra el nombre del campo ( &quot; title &quot; , &quot; ReleaseDate &quot; , &quot; Genre &quot; o &quot; Price &quot; ). El [`Html.EditorFor`](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) ayudante representa un `<input>` elemento HTML. El [`Html.ValidationMessageFor`](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) ayudante muestra cualquier mensaje de validación asociado a esa propiedad.

Ejecute la aplicación y vaya a la dirección URL de */Movies* . Haga clic en un vínculo **Edit** (Editar). En el explorador, vea el código fuente de la página. A continuación se muestra el código HTML del elemento Form.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

Los `<input>` elementos están en un `<form>` elemento HTML cuyo `action` atributo está establecido en post en la dirección URL de */Movies/Edit* . Los datos del formulario se publicarán en el servidor cuando se haga clic en el botón **Guardar** . La segunda línea muestra el token [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) oculto generado por la `@Html.AntiForgeryToken()` llamada.

## <a name="processing-the-post-request"></a>Procesamiento de la solicitud POST

En la siguiente lista se muestra la versión `HttpPost` del método de acción `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

El atributo [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) valida el token [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) generado por la `@Html.AntiForgeryToken()` llamada en la vista.

El [enlazador de modelos MVC de ASP.net](https://msdn.microsoft.com/library/dd410405.aspx) toma los valores del formulario expuesto y crea un `Movie` objeto que se pasa como `movie` parámetro. `ModelState.IsValid`Comprueba que los datos enviados en el formulario se pueden usar para modificar (editar o actualizar) un `Movie` objeto. Si los datos son válidos, los datos de la película se guardan en la `Movies` colección de `db` ( `MovieDBContext` instancia). Los nuevos datos de la película se guardan en la base de datos llamando al `SaveChanges` método de `MovieDBContext` . Después de guardar los datos, el código redirige al usuario al método de acción `Index` de la clase `MoviesController`, que muestra la colección de películas, incluidos los cambios que se acaban de hacer.

En cuanto la validación del lado cliente determina que el valor de un campo no es válido, se muestra un mensaje de error. Si JavaScript está deshabilitado, la validación del lado cliente está deshabilitada. Sin embargo, el servidor detecta que los valores expuestos no son válidos y los valores del formulario se vuelven a mostrar con mensajes de error.

La validación se examina con más detalle más adelante en el tutorial.

Las `Html.ValidationMessageFor` aplicaciones auxiliares de la plantilla de vista *Edit. cshtml* se encargan de mostrar los mensajes de error correspondientes.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Todos los `HttpGet` métodos siguen un patrón similar. Obtienen un objeto de película (o una lista de objetos, en el caso de `Index` ) y pasan el modelo a la vista. El `Create` método pasa un objeto de película vacío a la vista de creación. Todos los métodos que crean, editan, eliminan o modifican los datos lo hacen en la sobrecarga `HttpPost` del método. La modificación de los datos en un método HTTP GET es un riesgo para la seguridad, como se describe en la entrada de blog de [ASP.NET MVC Tip #46 – no usar los vínculos de eliminación porque crean carencias de seguridad](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). La modificación de los datos en un método GET también infringe los procedimientos recomendados de HTTP y el patrón [Rest](http://en.wikipedia.org/wiki/Representational_State_Transfer) de la arquitectura, que especifica que las solicitudes GET no deben cambiar el estado de la aplicación. En otras palabras, realizar una operación GET debería ser una operación segura sin efectos secundarios, que no modifica los datos persistentes.

## <a name="jquery-validation-for-non-english-locales"></a>validación de jQuery para configuraciones regionales que no estén en inglés

Si usa un equipo Inglés de EE. UU., puede omitir esta sección y pasar al siguiente tutorial. Puede descargar la versión de globalización de este tutorial [aquí](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475). Para obtener un tutorial de dos partes excelente sobre internacionalización, consulte [internacionalización de MVC 5 de Nadeem de ASP.net](http://afana.me/post/aspnet-mvc-internationalization.aspx).

> [!NOTE]
> para admitir la validación de jQuery para configuraciones regionales distintas del inglés que usan una coma ( &quot; , &quot; ) para un separador decimal y formatos de fecha que no son de Inglés de EE. UU., debe incluir *globalize.js* y el archivo de *globalize.cultures.jso referencia cultural * específico (de [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) y JavaScript que se va a usar `Globalize.parseFloat` . Puede obtener la validación no en Inglés de jQuery desde NuGet. (No instale el globalizado si usa una configuración regional en inglés).

1. En el menú **herramientas** , haga clic en **Administrador de paquetes Nuget**y, a continuación, haga clic en **administrar paquetes NuGet para la solución**.

    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. En el panel izquierdo, seleccione <strong>examinar *.</strong> * (Consulte la imagen siguiente).
3. En el cuadro de entrada, escriba * Globalization * *.

    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) Elija `jQuery.Validation.Globalize` , elija `MvcMovie` y haga clic en **instalar**. El archivo *Scripts\jquery.globalize\globalize.js* se agregará al proyecto. La carpeta * Scripts\jquery.globalize\cultures contendrá \* muchos archivos JavaScript de referencia cultural. Tenga en cuenta que este paquete puede tardar cinco minutos en instalarse.

   En el código siguiente se muestran las modificaciones en el archivo Views\Movies\Edit.cshtml:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Para evitar repetir este código en cada vista de edición, puede moverlo al archivo de diseño. Para optimizar la descarga del script, vea mi tutorial [de Unión y minificación](../../performance/bundling-and-minification.md).

Para obtener más información, consulte [ASP.NET MVC 3 internacionalización](http://afana.me/post/aspnet-mvc-internationalization.aspx) y [ASP.NET MVC 3 Internacionalization, parte 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

Como solución temporal, si no consigue que la validación funcione en su configuración regional, puede forzar que el equipo use el Inglés de EE. UU. o puede deshabilitar JavaScript en el explorador. Para obligar al equipo a utilizar el Inglés de EE. UU., puede Agregar el elemento de globalización al archivo de *web.config* raíz del proyecto. En el código siguiente se muestra el elemento Globalization con la referencia cultural establecida en Estados Unidos English.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a> En el siguiente tutorial, se implementará la funcionalidad de búsqueda.

> [!div class="step-by-step"]
> [Anterior](accessing-your-models-data-from-a-controller.md)
> [Siguiente](adding-search.md)
