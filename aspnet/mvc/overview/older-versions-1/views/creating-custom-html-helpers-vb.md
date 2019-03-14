---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Creación de aplicaciones auxiliares HTML personalizadas (VB) | Microsoft Docs
author: microsoft
description: El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares de HTML personalizado que puede usar en las vistas MVC. Aprovechando las ventajas de la aplicación auxiliar HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: e62e47bceddc516af7aa18fc66ed4ca4d704d277
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57034952"
---
<a name="creating-custom-html-helpers-vb"></a><span data-ttu-id="06ba4-104">Crear asistentes de HTML personalizados (VB)</span><span class="sxs-lookup"><span data-stu-id="06ba4-104">Creating Custom HTML Helpers (VB)</span></span>
====================
<span data-ttu-id="06ba4-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="06ba4-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="06ba4-106">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="06ba4-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> <span data-ttu-id="06ba4-107">El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares de HTML personalizado que puede usar en las vistas MVC.</span><span class="sxs-lookup"><span data-stu-id="06ba4-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="06ba4-108">Aprovechando las ventajas de las aplicaciones auxiliares HTML, puede reducir la cantidad de una mecanografía tediosa de etiquetas HTML que debe realizar para crear una página HTML estándar.</span><span class="sxs-lookup"><span data-stu-id="06ba4-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>


<span data-ttu-id="06ba4-109">El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares de HTML personalizado que puede usar en las vistas MVC.</span><span class="sxs-lookup"><span data-stu-id="06ba4-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="06ba4-110">Aprovechando las ventajas de las aplicaciones auxiliares HTML, puede reducir la cantidad de una mecanografía tediosa de etiquetas HTML que debe realizar para crear una página HTML estándar.</span><span class="sxs-lookup"><span data-stu-id="06ba4-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="06ba4-111">En la primera parte de este tutorial, describen algunas de las aplicaciones auxiliares de HTML existentes incluidos con el marco de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="06ba4-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="06ba4-112">A continuación, describen dos métodos para crear aplicaciones auxiliares de HTML personalizado: Explica cómo crear aplicaciones auxiliares de HTML personalizado mediante la creación de un método compartido y mediante la creación de un método de extensión.</span><span class="sxs-lookup"><span data-stu-id="06ba4-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a shared method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="06ba4-113">Descripción de las aplicaciones auxiliares HTML</span><span class="sxs-lookup"><span data-stu-id="06ba4-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="06ba4-114">Una aplicación auxiliar HTML es simplemente un método que devuelve una cadena.</span><span class="sxs-lookup"><span data-stu-id="06ba4-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="06ba4-115">La cadena puede representar cualquier tipo de contenido que desee.</span><span class="sxs-lookup"><span data-stu-id="06ba4-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="06ba4-116">Por ejemplo, puede usar aplicaciones auxiliares HTML para representar las etiquetas HTML estándar como HTML `<input>` y `<img>` etiquetas.</span><span class="sxs-lookup"><span data-stu-id="06ba4-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="06ba4-117">También puede usar aplicaciones auxiliares HTML para representar el contenido más complejo, como una tabla HTML de la base de datos o de una franja de pestañas.</span><span class="sxs-lookup"><span data-stu-id="06ba4-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="06ba4-118">El marco de ASP.NET MVC incluye el siguiente conjunto de aplicaciones auxiliares de HTML estándar (no es una lista completa):</span><span class="sxs-lookup"><span data-stu-id="06ba4-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="06ba4-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="06ba4-119">Html.ActionLink()</span></span>
- <span data-ttu-id="06ba4-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="06ba4-120">Html.BeginForm()</span></span>
- <span data-ttu-id="06ba4-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="06ba4-121">Html.CheckBox()</span></span>
- <span data-ttu-id="06ba4-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="06ba4-122">Html.DropDownList()</span></span>
- <span data-ttu-id="06ba4-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="06ba4-123">Html.EndForm()</span></span>
- <span data-ttu-id="06ba4-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="06ba4-124">Html.Hidden()</span></span>
- <span data-ttu-id="06ba4-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="06ba4-125">Html.ListBox()</span></span>
- <span data-ttu-id="06ba4-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="06ba4-126">Html.Password()</span></span>
- <span data-ttu-id="06ba4-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="06ba4-127">Html.RadioButton()</span></span>
- <span data-ttu-id="06ba4-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="06ba4-128">Html.TextArea()</span></span>
- <span data-ttu-id="06ba4-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="06ba4-129">Html.TextBox()</span></span>

<span data-ttu-id="06ba4-130">Por ejemplo, considere el formulario en el listado 1.</span><span class="sxs-lookup"><span data-stu-id="06ba4-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="06ba4-131">Este formulario se representa con la Ayuda de dos de las aplicaciones auxiliares de HTML estándar (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="06ba4-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="06ba4-132">Este formulario utiliza la `Html.BeginForm()` y `Html.TextBox()` métodos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="06ba4-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods.</span></span>


<span data-ttu-id="06ba4-133">[![Representa la página con las aplicaciones auxiliares HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="06ba4-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="06ba4-134">**Figura 01**: Representa la página con las aplicaciones auxiliares HTML ([haga clic aquí para ver imagen en tamaño completo](creating-custom-html-helpers-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="06ba4-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image3.png))</span></span>


<span data-ttu-id="06ba4-135">**Listado 1: `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="06ba4-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

<span data-ttu-id="06ba4-136">El `Html.BeginForm()` método auxiliar se utiliza para crear el código HTML de apertura y cierre `<form>` etiquetas.</span><span class="sxs-lookup"><span data-stu-id="06ba4-136">The `Html.BeginForm()` Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="06ba4-137">Tenga en cuenta que el `Html.BeginForm()` se llama al método dentro de un uso de la instrucción.</span><span class="sxs-lookup"><span data-stu-id="06ba4-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="06ba4-138">La instrucción using garantiza que el `<form>` etiqueta a la que se cierra al final del uso de los bloques.</span><span class="sxs-lookup"><span data-stu-id="06ba4-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="06ba4-139">Si lo prefiere, en lugar de crear el uso de un bloque, se puede llamar al método auxiliar Html.EndForm() para cerrar el `<form>` etiqueta.</span><span class="sxs-lookup"><span data-stu-id="06ba4-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="06ba4-140">Utilice el método de creación de apertura y cierre `<form>` etiqueta que se parece más intuitiva.</span><span class="sxs-lookup"><span data-stu-id="06ba4-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="06ba4-141">El `Html.TextBox()` métodos auxiliares se utilizan en el listado 1 para representar HTML `<input>` etiquetas.</span><span class="sxs-lookup"><span data-stu-id="06ba4-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="06ba4-142">Si selecciona Ver código fuente en el explorador, a continuación, verá el código fuente HTML en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="06ba4-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="06ba4-143">Tenga en cuenta que el origen contiene etiquetas HTML estándar.</span><span class="sxs-lookup"><span data-stu-id="06ba4-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="06ba4-144">Tenga en cuenta que el `Html.TextBox()`-HTML auxiliar se presenta con `<%= %>` etiquetas en lugar de `<% %>` etiquetas.</span><span class="sxs-lookup"><span data-stu-id="06ba4-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="06ba4-145">Si no incluye el signo igual, nada obtiene representa en el explorador.</span><span class="sxs-lookup"><span data-stu-id="06ba4-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="06ba4-146">El marco ASP.NET MVC contiene un pequeño conjunto de aplicaciones auxiliares.</span><span class="sxs-lookup"><span data-stu-id="06ba4-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="06ba4-147">Probablemente, necesitará ampliar el marco de MVC con las aplicaciones auxiliares de HTML personalizado.</span><span class="sxs-lookup"><span data-stu-id="06ba4-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="06ba4-148">En el resto de este tutorial, aprenderá dos métodos para crear aplicaciones auxiliares de HTML personalizado.</span><span class="sxs-lookup"><span data-stu-id="06ba4-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="06ba4-149">**Listado 2: `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="06ba4-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a><span data-ttu-id="06ba4-150">Crear aplicaciones auxiliares HTML con métodos compartidos</span><span class="sxs-lookup"><span data-stu-id="06ba4-150">Creating HTML Helpers with Shared Methods</span></span>

<span data-ttu-id="06ba4-151">Es la manera más fácil de crear una nueva aplicación auxiliar de HTML crear un método compartido que devuelve una cadena.</span><span class="sxs-lookup"><span data-stu-id="06ba4-151">The easiest way to create a new HTML Helper is to create a shared method that returns a string.</span></span> <span data-ttu-id="06ba4-152">Por ejemplo, imagine que se decide crear una nueva aplicación auxiliar HTML que representa un elemento HTML `<label>` etiqueta.</span><span class="sxs-lookup"><span data-stu-id="06ba4-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="06ba4-153">Puede usar la clase en el listado 2 para representar un `<label>`.</span><span class="sxs-lookup"><span data-stu-id="06ba4-153">You can use the class in Listing 2 to render a `<label>`.</span></span>

<span data-ttu-id="06ba4-154">**Listado 2: `Helpers\LabelHelper.vb`**</span><span class="sxs-lookup"><span data-stu-id="06ba4-154">**Listing 2 – `Helpers\LabelHelper.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

<span data-ttu-id="06ba4-155">No hay nada especial acerca de la clase en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="06ba4-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="06ba4-156">El `Label()` método simplemente devuelve una cadena.</span><span class="sxs-lookup"><span data-stu-id="06ba4-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="06ba4-157">La vista de índice modificada en el listado 3 utiliza el `LabelHelper` para representar HTML `<label>` etiquetas.</span><span class="sxs-lookup"><span data-stu-id="06ba4-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="06ba4-158">Tenga en cuenta que la vista incluye un `<%@ imports %>` directiva que importa el espacio de nombres Application1.Helpers.</span><span class="sxs-lookup"><span data-stu-id="06ba4-158">Notice that the view includes an `<%@ imports %>` directive that imports the Application1.Helpers namespace.</span></span>

<span data-ttu-id="06ba4-159">**Listado 2: `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="06ba4-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="06ba4-160">Crear aplicaciones auxiliares HTML con los métodos de extensión</span><span class="sxs-lookup"><span data-stu-id="06ba4-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="06ba4-161">Si desea crear aplicaciones auxiliares de HTML que simplemente funcionan como las aplicaciones auxiliares de HTML estándar incluidos en el marco de ASP.NET MVC, deberá crear métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="06ba4-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="06ba4-162">Métodos de extensión permiten agregar nuevos métodos a una clase existente.</span><span class="sxs-lookup"><span data-stu-id="06ba4-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="06ba4-163">Al crear un método de aplicación auxiliar HTML, agregar nuevos métodos para la `HtmlHelper` clase representada por la propiedad de una vista Html.</span><span class="sxs-lookup"><span data-stu-id="06ba4-163">When creating an HTML Helper method, you add new methods to the `HtmlHelper` class represented by a view's Html property.</span></span>

<span data-ttu-id="06ba4-164">El módulo de Visual Basic en el listado 3 agrega un método de extensión denominado `Label()` a la `HtmlHelper` clase.</span><span class="sxs-lookup"><span data-stu-id="06ba4-164">The Visual Basic module in Listing 3 adds an extension method named `Label()` to the `HtmlHelper` class.</span></span> <span data-ttu-id="06ba4-165">Hay un par de cosas que debe tener en cuenta acerca de este módulo.</span><span class="sxs-lookup"><span data-stu-id="06ba4-165">There are a couple of things that you should notice about this module.</span></span> <span data-ttu-id="06ba4-166">En primer lugar, tenga en cuenta que el módulo está decorado con el `<Extension()>` atributo.</span><span class="sxs-lookup"><span data-stu-id="06ba4-166">First, notice that the module is decorated with the `<Extension()>` attribute.</span></span> <span data-ttu-id="06ba4-167">Para poder usar este atributo, debe importar el `System.Runtime.CompilerServices` espacio de nombres</span><span class="sxs-lookup"><span data-stu-id="06ba4-167">In order to use this attribute, you must import the `System.Runtime.CompilerServices` namespace</span></span>

<span data-ttu-id="06ba4-168">En segundo lugar, tenga en cuenta que el primer parámetro de la `Label()` método representa el `HtmlHelper` clase.</span><span class="sxs-lookup"><span data-stu-id="06ba4-168">Second, notice that the first parameter of the `Label()` method represents the `HtmlHelper` class.</span></span> <span data-ttu-id="06ba4-169">El primer parámetro de un método de extensión indica la clase que extiende el método de extensión.</span><span class="sxs-lookup"><span data-stu-id="06ba4-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="06ba4-170">**Listado 3: `Helpers\LabelExtensions.vb`**</span><span class="sxs-lookup"><span data-stu-id="06ba4-170">**Listing 3 – `Helpers\LabelExtensions.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

<span data-ttu-id="06ba4-171">Después de crear un método de extensión y compilar la aplicación correctamente, el método de extensión aparece en Intellisense en Visual Studio, como todos los otros métodos de una clase (consulte la figura 2).</span><span class="sxs-lookup"><span data-stu-id="06ba4-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="06ba4-172">La única diferencia es que dicha extensión se muestran métodos con un símbolo especial junto a ellos (un icono de una flecha hacia abajo).</span><span class="sxs-lookup"><span data-stu-id="06ba4-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>


<span data-ttu-id="06ba4-173">[![Mediante el método de extensión Html.Label()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="06ba4-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span></span>

<span data-ttu-id="06ba4-174">**Figura 02**: Mediante el método de extensión Html.Label() ([haga clic aquí para ver imagen en tamaño completo](creating-custom-html-helpers-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="06ba4-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image6.png))</span></span>


<span data-ttu-id="06ba4-175">La vista de índice modificada en el listado 4 usa el método de extensión Html.Label() para representar todos sus &lt;etiqueta&gt; etiquetas.</span><span class="sxs-lookup"><span data-stu-id="06ba4-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its &lt;label&gt; tags.</span></span>

<span data-ttu-id="06ba4-176">**Listado 4: `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="06ba4-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="06ba4-177">Resumen</span><span class="sxs-lookup"><span data-stu-id="06ba4-177">Summary</span></span>

<span data-ttu-id="06ba4-178">En este tutorial, ha aprendido a dos métodos para crear aplicaciones auxiliares de HTML personalizado.</span><span class="sxs-lookup"><span data-stu-id="06ba4-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="06ba4-179">En primer lugar, ha aprendido a crear un personalizado `Label()` aplicación auxiliar HTML mediante la creación de un método compartido que devuelve una cadena.</span><span class="sxs-lookup"><span data-stu-id="06ba4-179">First, you learned how to create a custom `Label()` HTML Helper by creating a shared method that returns a string.</span></span> <span data-ttu-id="06ba4-180">A continuación, ha aprendido a crear un personalizado `Label()` método de aplicación auxiliar HTML mediante la creación de un método de extensión en el `HtmlHelper` clase.</span><span class="sxs-lookup"><span data-stu-id="06ba4-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="06ba4-181">En este tutorial, me centré en la creación de un método muy sencillo de aplicación auxiliar HTML.</span><span class="sxs-lookup"><span data-stu-id="06ba4-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="06ba4-182">Tenga en cuenta que una aplicación auxiliar HTML puede ser tan complicada como desee.</span><span class="sxs-lookup"><span data-stu-id="06ba4-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="06ba4-183">Puede compilar aplicaciones auxiliares de HTML que presentan contenido enriquecido, como vistas de árbol, los menús o tablas de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="06ba4-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="06ba4-184">[Anterior](asp-net-mvc-views-overview-vb.md)
> [Siguiente](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="06ba4-184">[Previous](asp-net-mvc-views-overview-vb.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span></span>
