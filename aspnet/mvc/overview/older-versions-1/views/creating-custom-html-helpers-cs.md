---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Creación de aplicaciones auxiliares HTML personalizadas (C-) Microsoft Docs
author: microsoft
description: El objetivo de este tutorial es demostrar cómo puede crear aplicaciones auxiliares HTML personalizadas que puede usar dentro de las vistas MVC. Aprovechando HTML Helper...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 7a2e5a5b42aa5bf267a42fef2fcad7022001ce6f
ms.sourcegitcommit: ce28244209db8615bc9bdd576a2e2c88174d318d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "80675339"
---
# <a name="creating-custom-html-helpers-c"></a><span data-ttu-id="e507d-104">Crear asistentes de HTML personalizados (C#)</span><span class="sxs-lookup"><span data-stu-id="e507d-104">Creating Custom HTML Helpers (C#)</span></span>

<span data-ttu-id="e507d-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e507d-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="e507d-106">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="e507d-106">Download PDF</span></span>](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> <span data-ttu-id="e507d-107">El objetivo de este tutorial es demostrar cómo puede crear aplicaciones auxiliares HTML personalizadas que puede usar dentro de las vistas MVC.</span><span class="sxs-lookup"><span data-stu-id="e507d-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="e507d-108">Aprovechando las aplicaciones auxiliares HTML, puede reducir la cantidad de tediosa escritura de etiquetas HTML que debe realizar para crear una página HTML estándar.</span><span class="sxs-lookup"><span data-stu-id="e507d-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="e507d-109">El objetivo de este tutorial es demostrar cómo puede crear aplicaciones auxiliares HTML personalizadas que puede usar dentro de las vistas MVC.</span><span class="sxs-lookup"><span data-stu-id="e507d-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="e507d-110">Aprovechando las aplicaciones auxiliares HTML, puede reducir la cantidad de tediosa escritura de etiquetas HTML que debe realizar para crear una página HTML estándar.</span><span class="sxs-lookup"><span data-stu-id="e507d-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="e507d-111">En la primera parte de este tutorial, describo algunas de las aplicaciones auxiliares HTML existentes incluidas con el marco de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e507d-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="e507d-112">A continuación, describo dos métodos de creación de aplicaciones auxiliares HTML personalizadas: explico cómo crear aplicaciones auxiliares HTML personalizadas mediante la creación de un método estático y mediante la creación de un método de extensión.</span><span class="sxs-lookup"><span data-stu-id="e507d-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a static method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="e507d-113">Comprender los ayudantes HTML</span><span class="sxs-lookup"><span data-stu-id="e507d-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="e507d-114">Una aplicación auxiliar HTML es solo un método que devuelve una cadena.</span><span class="sxs-lookup"><span data-stu-id="e507d-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="e507d-115">La cadena puede representar cualquier tipo de contenido que desee.</span><span class="sxs-lookup"><span data-stu-id="e507d-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="e507d-116">Por ejemplo, puede utilizar aplicaciones auxiliares HTML `<input>` para `<img>` representar etiquetas HTML estándar como HTML y etiquetas.</span><span class="sxs-lookup"><span data-stu-id="e507d-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="e507d-117">También puede usar aplicaciones auxiliares HTML para representar contenido más complejo, como una franja de pestañas o una tabla HTML de datos de base de datos.</span><span class="sxs-lookup"><span data-stu-id="e507d-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="e507d-118">El marco de ASP.NET MVC incluye el siguiente conjunto de aplicaciones auxiliares HTML estándar (esto no es una lista completa):</span><span class="sxs-lookup"><span data-stu-id="e507d-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="e507d-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="e507d-119">Html.ActionLink()</span></span>
- <span data-ttu-id="e507d-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="e507d-120">Html.BeginForm()</span></span>
- <span data-ttu-id="e507d-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="e507d-121">Html.CheckBox()</span></span>
- <span data-ttu-id="e507d-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="e507d-122">Html.DropDownList()</span></span>
- <span data-ttu-id="e507d-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="e507d-123">Html.EndForm()</span></span>
- <span data-ttu-id="e507d-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="e507d-124">Html.Hidden()</span></span>
- <span data-ttu-id="e507d-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="e507d-125">Html.ListBox()</span></span>
- <span data-ttu-id="e507d-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="e507d-126">Html.Password()</span></span>
- <span data-ttu-id="e507d-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="e507d-127">Html.RadioButton()</span></span>
- <span data-ttu-id="e507d-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="e507d-128">Html.TextArea()</span></span>
- <span data-ttu-id="e507d-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="e507d-129">Html.TextBox()</span></span>

<span data-ttu-id="e507d-130">Por ejemplo, considere el formulario en el listado 1.</span><span class="sxs-lookup"><span data-stu-id="e507d-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="e507d-131">Este formulario se representa con la ayuda de dos de las aplicaciones auxiliares HTML estándar (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="e507d-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="e507d-132">Este formulario `Html.BeginForm()` utiliza `Html.TextBox()` los métodos y Helper para representar un formulario HTML simple.</span><span class="sxs-lookup"><span data-stu-id="e507d-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods to render a simple HTML form.</span></span>

<span data-ttu-id="e507d-133">[![Página renderizada con aplicaciones auxiliares HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e507d-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="e507d-134">**Figura 01**: Página representada con aplicaciones auxiliares HTML[(Haga clic para ver la imagen de tamaño completo)](creating-custom-html-helpers-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="e507d-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image3.png))</span></span>

<span data-ttu-id="e507d-135">**Listado 1 –`Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="e507d-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

<span data-ttu-id="e507d-136">El método auxiliar Html.BeginForm() se utiliza para `<form>` crear las etiquetas HTML de apertura y cierre.</span><span class="sxs-lookup"><span data-stu-id="e507d-136">The Html.BeginForm() Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="e507d-137">Observe que `Html.BeginForm()` se llama al método dentro de una instrucción using.</span><span class="sxs-lookup"><span data-stu-id="e507d-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="e507d-138">La instrucción using `<form>` garantiza que la etiqueta se cierra al final del bloque using.</span><span class="sxs-lookup"><span data-stu-id="e507d-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="e507d-139">Si lo prefiere, en lugar de crear un bloque using, puede llamar `<form>` al método auxiliar Html.EndForm() para cerrar la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="e507d-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="e507d-140">Utilice cualquier enfoque para crear `<form>` una etiqueta de apertura y cierre que le parezca más intuitiva.</span><span class="sxs-lookup"><span data-stu-id="e507d-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="e507d-141">Los `Html.TextBox()` métodos auxiliares se utilizan `<input>` en el listado 1 para representar etiquetas HTML.</span><span class="sxs-lookup"><span data-stu-id="e507d-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="e507d-142">Si selecciona ver origen en su navegador, verá el origen HTML en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="e507d-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="e507d-143">Observe que el origen contiene etiquetas HTML estándar.</span><span class="sxs-lookup"><span data-stu-id="e507d-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e507d-144">observe que `Html.TextBox()`la aplicación auxiliar `<%= %>` -HTML `<% %>` se representa con etiquetas en lugar de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="e507d-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="e507d-145">Si no incluye el signo igual, no se representa nada en el explorador.</span><span class="sxs-lookup"><span data-stu-id="e507d-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="e507d-146">El marco de ASP.NET MVC contiene un pequeño conjunto de aplicaciones auxiliares.</span><span class="sxs-lookup"><span data-stu-id="e507d-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="e507d-147">Lo más probable es que necesite ampliar el marco MVC con aplicaciones auxiliares HTML personalizadas.</span><span class="sxs-lookup"><span data-stu-id="e507d-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="e507d-148">En el resto de este tutorial, aprenderá dos métodos para crear aplicaciones auxiliares HTML personalizadas.</span><span class="sxs-lookup"><span data-stu-id="e507d-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="e507d-149">**Listado 2 –`Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="e507d-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a><span data-ttu-id="e507d-150">Creación de aplicaciones auxiliares HTML con métodos estáticos</span><span class="sxs-lookup"><span data-stu-id="e507d-150">Creating HTML Helpers with Static Methods</span></span>

<span data-ttu-id="e507d-151">La forma más fácil de crear una nueva aplicación auxiliar HTML es crear un método estático que devuelva una cadena.</span><span class="sxs-lookup"><span data-stu-id="e507d-151">The easiest way to create a new HTML Helper is to create a static method that returns a string.</span></span> <span data-ttu-id="e507d-152">Imagine, por ejemplo, que decide crear una nueva aplicación `<label>` auxiliar HTML que represente una etiqueta HTML.</span><span class="sxs-lookup"><span data-stu-id="e507d-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="e507d-153">Puede utilizar la clase del listado `<label>` 2 para representar un archivo .</span><span class="sxs-lookup"><span data-stu-id="e507d-153">You can use the class in Listing 2 to render a `<label>` .</span></span>

<span data-ttu-id="e507d-154">**Listado 2 –`Helpers\LabelHelper.cs`**</span><span class="sxs-lookup"><span data-stu-id="e507d-154">**Listing 2 – `Helpers\LabelHelper.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

<span data-ttu-id="e507d-155">No hay nada especial en la clase en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="e507d-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="e507d-156">El `Label()` método simplemente devuelve una cadena.</span><span class="sxs-lookup"><span data-stu-id="e507d-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="e507d-157">La vista de índice modificada `LabelHelper` en `<label>` el listado 3 utiliza la para representar etiquetas HTML.</span><span class="sxs-lookup"><span data-stu-id="e507d-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="e507d-158">Observe que la `<%@ imports %>` vista incluye `Application1.Helpers` una directiva que importa el espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="e507d-158">Notice that the view includes an `<%@ imports %>` directive that imports the `Application1.Helpers` namespace.</span></span>

<span data-ttu-id="e507d-159">**Listado 2 –`Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="e507d-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="e507d-160">Creación de aplicaciones auxiliares HTML con métodos de extensión</span><span class="sxs-lookup"><span data-stu-id="e507d-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="e507d-161">Si desea crear aplicaciones auxiliares HTML que funcionan igual que las aplicaciones auxiliares HTML estándar incluidas en el marco de ASP.NET MVC, debe crear métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="e507d-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="e507d-162">Los métodos de extensión permiten agregar nuevos métodos a una clase existente.</span><span class="sxs-lookup"><span data-stu-id="e507d-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="e507d-163">Al crear un método auxiliar HTML, agregue nuevos métodos a la HtmlHelper clase representada por una vista Html propiedad.</span><span class="sxs-lookup"><span data-stu-id="e507d-163">When creating an HTML Helper method, you add new methods to the HtmlHelper class represented by a view's Html property.</span></span>

<span data-ttu-id="e507d-164">La clase del listado 3 agrega `HtmlHelper` un `Label()`método de extensión a la clase denominada .</span><span class="sxs-lookup"><span data-stu-id="e507d-164">The class in Listing 3 adds an extension method to the `HtmlHelper` class named `Label()`.</span></span> <span data-ttu-id="e507d-165">Hay un par de cosas que debe sintener acerca de esta clase.</span><span class="sxs-lookup"><span data-stu-id="e507d-165">There are a couple of things that you should notice about this class.</span></span> <span data-ttu-id="e507d-166">En primer lugar, observe que la clase es una clase estática.</span><span class="sxs-lookup"><span data-stu-id="e507d-166">First, notice that the class is a static class.</span></span> <span data-ttu-id="e507d-167">Debe definir un método de extensión con una clase estática.</span><span class="sxs-lookup"><span data-stu-id="e507d-167">You must define an extension method with a static class.</span></span>

<span data-ttu-id="e507d-168">En segundo lugar, observe `Label()` que el primer parámetro `this`del método va precedido de la palabra clave .</span><span class="sxs-lookup"><span data-stu-id="e507d-168">Second, notice that the first parameter of the `Label()` method is preceded by the keyword `this`.</span></span> <span data-ttu-id="e507d-169">El primer parámetro de un método de extensión indica la clase que extiende el método de extensión.</span><span class="sxs-lookup"><span data-stu-id="e507d-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="e507d-170">**Listado 3 –`Helpers\LabelExtensions.cs`**</span><span class="sxs-lookup"><span data-stu-id="e507d-170">**Listing 3 – `Helpers\LabelExtensions.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

<span data-ttu-id="e507d-171">Después de crear un método de extensión y compilar la aplicación correctamente, el método de extensión aparece en Visual Studio Intellisense como todos los demás métodos de una clase (consulte la figura 2).</span><span class="sxs-lookup"><span data-stu-id="e507d-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="e507d-172">La única diferencia es que los métodos de extensión aparecen con un símbolo especial junto a ellos (un icono de una flecha hacia abajo).</span><span class="sxs-lookup"><span data-stu-id="e507d-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>

<span data-ttu-id="e507d-173">[![Uso del método de extensión Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="e507d-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span></span>

<span data-ttu-id="e507d-174">**Figura 02**: Uso del método de extensión Html.Label() ( Haga clic para ver la[imagen de tamaño completo](creating-custom-html-helpers-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="e507d-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image6.png))</span></span>

<span data-ttu-id="e507d-175">La vista de índice modificada en el listado 4 utiliza el `<label>` método de extensión Html.Label() para representar todas sus etiquetas.</span><span class="sxs-lookup"><span data-stu-id="e507d-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its `<label>` tags.</span></span>

<span data-ttu-id="e507d-176">**Listado 4 –`Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="e507d-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="e507d-177">Resumen</span><span class="sxs-lookup"><span data-stu-id="e507d-177">Summary</span></span>

<span data-ttu-id="e507d-178">En este tutorial, aprendió dos métodos para crear aplicaciones auxiliares HTML personalizadas.</span><span class="sxs-lookup"><span data-stu-id="e507d-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="e507d-179">En primer lugar, aprendió `Label()` a crear una aplicación auxiliar HTML personalizada mediante la creación de un método estático que devuelve una cadena.</span><span class="sxs-lookup"><span data-stu-id="e507d-179">First, you learned how to create a custom `Label()` HTML Helper by creating a static method that returns a string.</span></span> <span data-ttu-id="e507d-180">A continuación, aprendió a `Label()` crear un método auxiliar HTML `HtmlHelper` personalizado mediante la creación de un método de extensión en la clase.</span><span class="sxs-lookup"><span data-stu-id="e507d-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="e507d-181">En este tutorial, me centré en la creación de un método de aplicación auxiliar HTML extremadamente simple.</span><span class="sxs-lookup"><span data-stu-id="e507d-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="e507d-182">Tenga en cuenta que una aplicación auxiliar HTML puede ser tan complicada como desee.</span><span class="sxs-lookup"><span data-stu-id="e507d-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="e507d-183">Puede crear aplicaciones auxiliares HTML que representen contenido enriquecido, como vistas de árbol, menús o tablas de datos de base de datos.</span><span class="sxs-lookup"><span data-stu-id="e507d-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e507d-184">[Anterior](asp-net-mvc-views-overview-cs.md)
> [Siguiente](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span><span class="sxs-lookup"><span data-stu-id="e507d-184">[Previous](asp-net-mvc-views-overview-cs.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span></span>
