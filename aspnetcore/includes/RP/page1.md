---
ms.openlocfilehash: 8e11e5a8858e6cbc80cdbbeb3e69650487d720ee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57038752"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="a5185-101">Páginas de Razor con scaffolding en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5185-101">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="a5185-102">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a5185-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a5185-103">En este tutorial se examinan las páginas de Razor creadas por la técnica scaffolding en el tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="a5185-103">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span> 

<span data-ttu-id="a5185-104">[Vea o descargue](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21) un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a5185-104">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="a5185-105">Páginas Crear, Eliminar, Detalles y Editar.</span><span class="sxs-lookup"><span data-stu-id="a5185-105">The Create, Delete, Details, and Edit pages.</span></span>

<span data-ttu-id="a5185-106">Examine el modelo de página *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="a5185-106">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index21.cshtml.cs)]

::: moniker-end

<span data-ttu-id="a5185-107">Las páginas de Razor se derivan de `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="a5185-107">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="a5185-108">Por convención, la clase derivada de `PageModel` se denomina `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="a5185-108">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="a5185-109">El constructor aplica la [inserción de dependencias](xref:fundamentals/dependency-injection) para agregar el `MovieContext` a la página.</span><span class="sxs-lookup"><span data-stu-id="a5185-109">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `MovieContext` to the page.</span></span> <span data-ttu-id="a5185-110">Todas las páginas con scaffolding siguen este patrón.</span><span class="sxs-lookup"><span data-stu-id="a5185-110">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="a5185-111">Vea [Código asincrónico](xref:data/ef-rp/intro#asynchronous-code) para obtener más información sobre programación asincrónica con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a5185-111">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="a5185-112">Cuando se efectúa una solicitud para la página, el método `OnGetAsync` devuelve una lista de películas a la página de Razor.</span><span class="sxs-lookup"><span data-stu-id="a5185-112">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="a5185-113">Se llama a `OnGetAsync` o a `OnGet` en una página de Razor para inicializar el estado de la página.</span><span class="sxs-lookup"><span data-stu-id="a5185-113">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="a5185-114">En este caso, `OnGetAsync` obtiene una lista de películas y las muestra.</span><span class="sxs-lookup"><span data-stu-id="a5185-114">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="a5185-115">Cuando `OnGet` devuelve `void` o `OnGetAsync` devuelve `Task`, no se utiliza ningún método de devolución.</span><span class="sxs-lookup"><span data-stu-id="a5185-115">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="a5185-116">Cuando el tipo de valor devuelto es `IActionResult` o `Task<IActionResult>`, se debe proporcionar una instrucción return.</span><span class="sxs-lookup"><span data-stu-id="a5185-116">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="a5185-117">Por ejemplo, el método *Pages/Movies/Create.cshtml.cs*`OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="a5185-117">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="a5185-118">Examine la página de Razor *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a5185-118">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="a5185-119">Razor puede realizar la transición de HTML a C# o a un marcado específico de Razor.</span><span class="sxs-lookup"><span data-stu-id="a5185-119">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="a5185-120">Cuando el símbolo `@` va seguido de una [palabra clave reservada de Razor](xref:mvc/views/razor#razor-reserved-keywords), realiza una transición a un marcado específico de Razor; en caso contrario, realiza la transición a C#.</span><span class="sxs-lookup"><span data-stu-id="a5185-120">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="a5185-121">La directiva de Razor `@page` convierte el archivo en una acción &mdash; de MVC, lo que significa que puede controlar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="a5185-121">The `@page` Razor directive makes the file into an MVC action &mdash; which means that it can handle requests.</span></span> <span data-ttu-id="a5185-122">`@page` debe ser la primera directiva de Razor de una página.</span><span class="sxs-lookup"><span data-stu-id="a5185-122">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="a5185-123">`@page` es un ejemplo de la transición a un marcado específico de Razor.</span><span class="sxs-lookup"><span data-stu-id="a5185-123">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="a5185-124">Vea [Razor syntax](xref:mvc/views/razor#razor-syntax) (Sintaxis de Razor) para más información.</span><span class="sxs-lookup"><span data-stu-id="a5185-124">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="a5185-125">Examine la expresión lambda usada en el siguiente asistente de HTML:</span><span class="sxs-lookup"><span data-stu-id="a5185-125">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="a5185-126">El asistente de HTML `DisplayNameFor` inspecciona la propiedad `Title` a la que se hace referencia en la expresión lambda para determinar el nombre para mostrar.</span><span class="sxs-lookup"><span data-stu-id="a5185-126">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="a5185-127">La expresión lambda se inspecciona, no se evalúa.</span><span class="sxs-lookup"><span data-stu-id="a5185-127">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="a5185-128">Esto significa que no hay ninguna infracción de acceso si `model`, `model.Movie` o `model.Movie[0]` son `null` o están vacíos.</span><span class="sxs-lookup"><span data-stu-id="a5185-128">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="a5185-129">Al evaluar la expresión lambda (por ejemplo, con `@Html.DisplayFor(modelItem => item.Title)`), se evalúan los valores de propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="a5185-129">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="a5185-130">La directiva @model </span><span class="sxs-lookup"><span data-stu-id="a5185-130">The @model directive</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="a5185-131">La directiva `@model` especifica el tipo del modelo que se pasa a la página de Razor.</span><span class="sxs-lookup"><span data-stu-id="a5185-131">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="a5185-132">En el ejemplo anterior, la línea `@model` permite que la clase derivada de `PageModel` esté disponible en la página de Razor.</span><span class="sxs-lookup"><span data-stu-id="a5185-132">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="a5185-133">El modelo se usa en los [asistentes de HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)`@Html.DisplayNameFor` y `@Html.DisplayFor` de la página.</span><span class="sxs-lookup"><span data-stu-id="a5185-133">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

<span data-ttu-id="a5185-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### Propiedades ViewData y Layout</span><span class="sxs-lookup"><span data-stu-id="a5185-134"><!-- why don't xref links work?
[HTML Helpers 2](xref:aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs)
-->

<a name="vd"></a>
### ViewData and layout</span></span>

<span data-ttu-id="a5185-135">Observe el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a5185-135">Consider the following code:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="a5185-136">El código resaltado anterior es un ejemplo de Razor con una transición a C#.</span><span class="sxs-lookup"><span data-stu-id="a5185-136">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="a5185-137">Los caracteres `{` y `}` delimitan un bloque de código de C#.</span><span class="sxs-lookup"><span data-stu-id="a5185-137">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="a5185-138">La clase base `PageModel` tiene una propiedad de diccionario `ViewData` que se puede usar para agregar datos que quiera pasar a una vista.</span><span class="sxs-lookup"><span data-stu-id="a5185-138">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="a5185-139">Puede agregar objetos al diccionario `ViewData` con un patrón de clave/valor.</span><span class="sxs-lookup"><span data-stu-id="a5185-139">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="a5185-140">En el ejemplo anterior, se agrega la propiedad "Title" al diccionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="a5185-140">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> 

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a5185-141">La propiedad "Title" se usa en el archivo *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a5185-141">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="a5185-142">En el siguiente marcado se muestran las primeras líneas del archivo *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a5185-142">The following markup shows the first few lines of the *Pages/Shared/_Layout.cshtml* file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a5185-143">La propiedad "Title" se usa en el archivo *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a5185-143">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="a5185-144">En el siguiente marcado se muestran las primeras líneas del archivo *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a5185-144">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

::: moniker-end

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout1.cshtml?highlight=6-999)]

<span data-ttu-id="a5185-145">La línea `@*Markup removed for brevity.*@` es un comentario de Razor.</span><span class="sxs-lookup"><span data-stu-id="a5185-145">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="a5185-146">A diferencia de los comentarios HTML (`<!-- -->`), los comentarios de Razor no se envían al cliente.</span><span class="sxs-lookup"><span data-stu-id="a5185-146">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

<span data-ttu-id="a5185-147">Ejecute la aplicación y pruebe los vínculos del proyecto (**Inicio**, **Acerca de**, **Contacto**, **Crear**, **Editar** y **Eliminar**).</span><span class="sxs-lookup"><span data-stu-id="a5185-147">Run the app and test the links in the project (**Home**, **About**, **Contact**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="a5185-148">Cada página establece el título, que puede ver en la pestaña del explorador. Al marcar una página, se usa el título para el marcador.</span><span class="sxs-lookup"><span data-stu-id="a5185-148">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span> <span data-ttu-id="a5185-149">*Pages/Index.cshtml* y *Pages/Movies/Index.cshtml* actualmente tienen el mismo título, pero puede modificarlos para que tengan valores diferentes.</span><span class="sxs-lookup"><span data-stu-id="a5185-149">*Pages/Index.cshtml* and *Pages/Movies/Index.cshtml* currently have the same title, but you can modify them to have different values.</span></span>

> [!NOTE]
> <span data-ttu-id="a5185-150">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="a5185-150">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="a5185-151">Para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos, debe seguir unos pasos para globalizar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a5185-151">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="a5185-152">Consulte el [problema 4076 de GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) para obtener instrucciones sobre cómo agregar la coma decimal.</span><span class="sxs-lookup"><span data-stu-id="a5185-152">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="a5185-153">La propiedad `Layout` se establece en el archivo *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a5185-153">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

<span data-ttu-id="a5185-154">El marcado anterior establece el archivo de diseño en *Pages/Shared/_Layout.cshtml* para todos los archivos de Razor en la carpeta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="a5185-154">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="a5185-155">Vea [Layout](xref:razor-pages/index#layout) (Diseño) para más información.</span><span class="sxs-lookup"><span data-stu-id="a5185-155">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="a5185-156">Actualizar el diseño</span><span class="sxs-lookup"><span data-stu-id="a5185-156">Update the layout</span></span>

<span data-ttu-id="a5185-157">Cambie el elemento `<title>` del archivo *Pages/Shared/_Layout.cshtml* para usar una cadena más corta.</span><span class="sxs-lookup"><span data-stu-id="a5185-157">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to use a shorter string.</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="a5185-158">Busque el siguiente elemento delimitador en el archivo *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a5185-158">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a asp-page="/Index" class="navbar-brand">RazorPagesMovie</a>
```
<span data-ttu-id="a5185-159">Reemplace el elemento anterior por el marcado siguiente.</span><span class="sxs-lookup"><span data-stu-id="a5185-159">Replace the preceding element with the following markup.</span></span>

```cshtml
<a asp-page="/Movies/Index" class="navbar-brand">RpMovie</a>
```

<span data-ttu-id="a5185-160">El elemento delimitador anterior es un [asistente de etiquetas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="a5185-160">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="a5185-161">En este caso, se trata de el [asistente de etiquetas Anchor](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="a5185-161">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="a5185-162">El atributo y valor del asistente de etiquetas `asp-page="/Movies/Index"` crea un vínculo a la página de Razor `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="a5185-162">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span>

<span data-ttu-id="a5185-163">Guarde los cambios y pruebe la aplicación haciendo clic en el vínculo **RpMovie**.</span><span class="sxs-lookup"><span data-stu-id="a5185-163">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="a5185-164">Consulte el archivo [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Shared/_Layout.cshtml) en GitHub.</span><span class="sxs-lookup"><span data-stu-id="a5185-164">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Shared/_Layout.cshtml) file in GitHub.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="a5185-165">Modelo de página Crear</span><span class="sxs-lookup"><span data-stu-id="a5185-165">The Create page model</span></span>

<span data-ttu-id="a5185-166">Examine el modelo de página *Pages/Movies/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="a5185-166">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create21.cshtml.cs?name=snippetALL)]

::: moniker-end


<span data-ttu-id="a5185-167">El método `OnGet` inicializa cualquier estado necesario para la página.</span><span class="sxs-lookup"><span data-stu-id="a5185-167">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="a5185-168">La página Crear no tiene ningún estado que inicializar, de modo que se devuelve `Page`.</span><span class="sxs-lookup"><span data-stu-id="a5185-168">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="a5185-169">Más adelante en el tutorial veremos el estado de inicialización del método `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="a5185-169">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="a5185-170">El método `Page` crea un objeto `PageResult` que representa la página *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a5185-170">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="a5185-171">La propiedad `Movie` usa el atributo `[BindProperty]` para participar en el [enlace de modelos](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="a5185-171">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="a5185-172">Cuando el formulario de creación publica los valores del formulario, el tiempo de ejecución de ASP.NET Core enlaza los valores publicados con el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="a5185-172">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="a5185-173">El método `OnPostAsync` se ejecuta cuando la página publica los datos del formulario:</span><span class="sxs-lookup"><span data-stu-id="a5185-173">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="a5185-174">Si hay algún error de modelo, se vuelve a mostrar el formulario, junto con los datos del formulario publicados.</span><span class="sxs-lookup"><span data-stu-id="a5185-174">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="a5185-175">La mayoría de los errores de modelo se pueden capturar en el cliente antes de que se publique el formulario.</span><span class="sxs-lookup"><span data-stu-id="a5185-175">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="a5185-176">Un ejemplo de un error de modelo consiste en publicar un valor para el campo de fecha que no se puede convertir en una fecha.</span><span class="sxs-lookup"><span data-stu-id="a5185-176">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="a5185-177">Más adelante en el tutorial volveremos a hablar de la validación del lado cliente y de la validación de modelos.</span><span class="sxs-lookup"><span data-stu-id="a5185-177">We'll talk more about client-side validation and model validation later in the tutorial.</span></span>

<span data-ttu-id="a5185-178">Si no hay ningún error de modelo, los datos se guardan y el explorador se redirige a la página Índice.</span><span class="sxs-lookup"><span data-stu-id="a5185-178">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="a5185-179">Página de Razor Crear</span><span class="sxs-lookup"><span data-stu-id="a5185-179">The Create Razor Page</span></span>

<span data-ttu-id="a5185-180">Examine el archivo de la página de Razor *Pages/Movies/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a5185-180">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!--
Visual Studio displays the `<form method="post">` tag in a distinctive font used for Tag Helpers. The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper). The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).


![VS17 view of Create.cshtml page](page/_static/th.png)
-->
