---
title: Páginas de Razor con scaffolding en ASP.NET Core
author: rick-anderson
description: Explica las páginas de Razor generadas por la técnica scaffolding.
ms.author: riande
ms.date: 12/4/2018
uid: tutorials/razor-pages/page
ms.openlocfilehash: ad87e3da72c3dd6adf8cf55d16da58fa47ed5542
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57055512"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a>Páginas de Razor con scaffolding en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tutorial se examinan las páginas de Razor creadas por la técnica scaffolding en el tutorial anterior.

[Vea o descargue](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) un ejemplo.

## <a name="the-create-delete-details-and-edit-pages"></a>Páginas de creación, eliminación, detalles y edición

Examine el modelo de página *Pages/Movies/Index.cshtml.cs*:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

Las páginas de Razor se derivan de `PageModel`. Por convención, la clase derivada de `PageModel` se denomina `<PageName>Model`. El constructor aplica la [inserción de dependencias](xref:fundamentals/dependency-injection) para agregar el `RazorPagesMovieContext` a la página. Todas las páginas con scaffolding siguen este patrón. Vea [Código asincrónico](xref:data/ef-rp/intro#asynchronous-code) para obtener más información sobre programación asincrónica con Entity Framework.

Cuando se efectúa una solicitud para la página, el método `OnGetAsync` devuelve una lista de películas a la página de Razor. Se llama a `OnGetAsync` o a `OnGet` en una página de Razor para inicializar el estado de la página. En este caso, `OnGetAsync` obtiene una lista de películas y las muestra.

Cuando `OnGet` devuelve `void` o `OnGetAsync` devuelve `Task`, no se utiliza ningún método de devolución. Cuando el tipo de valor devuelto es `IActionResult` o `Task<IActionResult>`, se debe proporcionar una instrucción return. Por ejemplo, el método *Pages/Movies/Create.cshtml.cs*`OnPostAsync`:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> Examine la página de Razor *Pages/Movies/Index.cshtml*:

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

Razor puede realizar la transición de HTML a C# o a un marcado específico de Razor. Cuando el símbolo `@` va seguido de una [palabra clave reservada de Razor](xref:mvc/views/razor#razor-reserved-keywords), realiza una transición a un marcado específico de Razor; en caso contrario, realiza la transición a C#.

La directiva de Razor `@page` convierte el archivo en una acción de MVC, lo que significa que puede controlar las solicitudes. `@page` debe ser la primera directiva de Razor de una página. `@page` es un ejemplo de la transición a un marcado específico de Razor. Vea [Razor syntax](xref:mvc/views/razor#razor-syntax) (Sintaxis de Razor) para más información.

Examine la expresión lambda usada en el siguiente asistente de HTML:

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

El asistente de HTML `DisplayNameFor` inspecciona la propiedad `Title` a la que se hace referencia en la expresión lambda para determinar el nombre para mostrar. La expresión lambda se inspecciona, no se evalúa. Esto significa que no hay ninguna infracción de acceso si `model`, `model.Movie` o `model.Movie[0]` son `null` o están vacíos. Al evaluar la expresión lambda (por ejemplo, con `@Html.DisplayFor(modelItem => item.Title)`), se evalúan los valores de propiedad del modelo.

<a name="md"></a>
### <a name="the-model-directive"></a>La directiva @model 

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

La directiva `@model` especifica el tipo del modelo que se pasa a la página de Razor. En el ejemplo anterior, la línea `@model` permite que la clase derivada de `PageModel` esté disponible en la página de Razor. El modelo se usa en los [asistentes de HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)`@Html.DisplayNameFor` y `@Html.DisplayFor` de la página.

### <a name="the-layout-page"></a>Página de diseño

Seleccione los vínculos de menú (**RazorPagesMovie** [Película de Razor Pages], **Home** [Inicio] y **Privacy** [Privacidad]). Cada página muestra el mismo diseño de menú. El diseño de menú se implementa en el archivo *Pages/Shared/_Layout.cshtml*. Abra el archivo *Pages/Shared/_Layout.cshtml*.

Las plantillas de [diseño](xref:mvc/views/layout) permiten especificar el diseño del contenedor HTML del sitio en un solo lugar y, después, aplicarlo en varias páginas del sitio. Busque la línea `@RenderBody()`. `RenderBody` es un marcador de posición donde se mostrarán todas las vistas específicas de página que cree, *encapsuladas* en la página de diseño. Por ejemplo, si selecciona el vínculo **Privacy** (Privacidad), la vista **Pages/Privacy.cshtml** se representará dentro del método `RenderBody`.

<a name="vd"></a>
### <a name="viewdata-and-layout"></a>Propiedades ViewData y Layout

Tenga en cuenta el siguiente código del archivo *Pages/Movies/Index.cshtml*:

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

El código resaltado anterior es un ejemplo de Razor con una transición a C#. Los caracteres `{` y `}` delimitan un bloque de código de C#.

La clase base `PageModel` tiene una propiedad de diccionario `ViewData` que se puede usar para agregar datos que quiera pasar a una vista. Puede agregar objetos al diccionario `ViewData` con un patrón de clave/valor. En el ejemplo anterior, se agrega la propiedad "Title" al diccionario `ViewData`. 

La propiedad "Title" se usa en el archivo *Pages/Shared/_Layout.cshtml*. En el siguiente marcado se muestran las primeras líneas del archivo *_Layout.cshtml*.

<!-- we need a snapshot copy of layout because we are
changing in in the next step. 
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

La línea `@*Markup removed for brevity.*@` es un comentario de Razor que no aparece en el archivo de diseño. A diferencia de los comentarios HTML (`<!-- -->`), los comentarios de Razor no se envían al cliente.

### <a name="update-the-layout"></a>Actualizar el diseño

Al cambiar el elemento `<title>` del archivo *Pages/Shared/_Layout.cshtml*, se muestra **Movie** en lugar de **RazorPagesMovie**.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]


Busque el siguiente elemento delimitador en el archivo *Pages/Shared/_Layout.cshtml*.

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

Reemplace el elemento anterior por el marcado siguiente.

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

El elemento delimitador anterior es un [asistente de etiquetas](xref:mvc/views/tag-helpers/intro). En este caso, se trata de el [asistente de etiquetas Anchor](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper). El atributo y valor del asistente de etiquetas `asp-page="/Movies/Index"` crea un vínculo a la página de Razor `/Movies/Index`. El valor de atributo `asp-area` está vacío, por lo que no se usa el área del vínculo. Consulte [Áreas](xref:mvc/controllers/areas) para obtener más información.

Guarde los cambios y pruebe la aplicación haciendo clic en el vínculo **RpMovie**. Si tiene cualquier problema, consulte el archivo [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) en GitHub.

Pruebe los otros vínculos (**Inicio**, **RpMovie**, **Crear**, **Editar** y **Eliminar**). Cada página establece el título, que puede ver en la pestaña del explorador. Al marcar una página, se usa el título para el marcador.

> [!NOTE]
> Es posible que no pueda escribir comas decimales en el campo `Price`. Para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos, debe seguir unos pasos para globalizar la aplicación. Consulte el [problema 4076 de GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) para obtener instrucciones sobre cómo agregar la coma decimal.

La propiedad `Layout` se establece en el archivo *Pages/_ViewStart.cshtml*:

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

El marcado anterior establece el archivo de diseño en *Pages/Shared/_Layout.cshtml* para todos los archivos de Razor en la carpeta *Pages*. Vea [Layout](xref:razor-pages/index#layout) (Diseño) para más información.

### <a name="the-create-page-model"></a>Modelo de página Crear

Examine el modelo de página *Pages/Movies/Create.cshtml.cs*:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

El método `OnGet` inicializa cualquier estado necesario para la página. La página Crear no tiene ningún estado que inicializar, de modo que se devuelve `Page`. Más adelante en el tutorial veremos el estado de inicialización del método `OnGet`. El método `Page` crea un objeto `PageResult` que representa la página *Create.cshtml*.

La propiedad `Movie` usa el atributo `[BindProperty]` para participar en el [enlace de modelos](xref:mvc/models/model-binding). Cuando el formulario de creación publica los valores del formulario, el tiempo de ejecución de ASP.NET Core enlaza los valores publicados con el modelo `Movie`.

El método `OnPostAsync` se ejecuta cuando la página publica los datos del formulario:

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

Si hay algún error de modelo, se vuelve a mostrar el formulario, junto con los datos del formulario publicados. La mayoría de los errores de modelo se pueden capturar en el cliente antes de que se publique el formulario. Un ejemplo de un error de modelo consiste en publicar un valor para el campo de fecha que no se puede convertir en una fecha. Más adelante en el tutorial, hablaremos de la validación del lado cliente y de la validación de modelos.

Si no hay ningún error de modelo, los datos se guardan y el explorador se redirige a la página Índice.

### <a name="the-create-razor-page"></a>Página de Razor Crear

Examine el archivo de la página de Razor *Pages/Movies/Create.cshtml*:

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio muestra la etiqueta `<form method="post">` con una fuente negrita diferenciada que se aplica a los asistentes de etiquetas:

![Vista de VS17 de la página Create.cshtml](page/_static/th.png)
<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Para obtener más información sobre los asistentes de etiquetas como `<form method="post">`, consulte [Asistentes de etiquetas en ASP.NET Core](xref:mvc/views/tag-helpers/intro).

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

Visual Studio para Mac muestra la etiqueta `<form method="post">` con una fuente negrita diferenciada que se aplica a los asistentes de etiquetas.

---  
<!-- End of VS tabs -->

El elemento `<form method="post">` es un [asistente de etiquetas de formulario](xref:mvc/views/working-with-forms#the-form-tag-helper). El asistente de etiquetas de formulario incluye automáticamente un [token antifalsificación](xref:security/anti-request-forgery).

El motor de scaffolding crea un marcado de Razor para cada campo del modelo (excepto el identificador) similar al siguiente:

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

Los [asistentes de etiquetas de validación](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` y ` <span asp-validation-for`) muestran errores de validación. La validación se trata con más detalle en un punto posterior de esta serie.

El [asistente de etiquetas](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) genera el título de la etiqueta y el atributo `for` para la propiedad `Title`.

El [asistente de etiquetas de entrada](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) usa los atributos [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) y genera los atributos HTML necesarios para la validación de jQuery en el lado del cliente.

> [!div class="step-by-step"]
> [Anterior: Adición de un modelo](xref:tutorials/razor-pages/model)
> [Siguiente: Base de datos](xref:tutorials/razor-pages/sql)
