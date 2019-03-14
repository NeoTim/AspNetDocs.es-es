---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: Agregar una vista | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versión actualizada de este tutorial está disponible aquí que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y demostraciones...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 7b55a55db6207b8ff18b2dd207e919cee45f6973
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57030382"
---
<a name="adding-a-view"></a><span data-ttu-id="3f232-104">Agregar una vista</span><span class="sxs-lookup"><span data-stu-id="3f232-104">Adding a View</span></span>
====================
<span data-ttu-id="3f232-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="3f232-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> > [!NOTE]
> > <span data-ttu-id="3f232-106">Hay disponible una versión actualizada de este tutorial [aquí](../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="3f232-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="3f232-107">Es más seguro y mucho más fácil de seguir y se muestran las características más.</span><span class="sxs-lookup"><span data-stu-id="3f232-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="3f232-108">En esta sección va a modificar el `HelloWorldController` clase para usar la vista archivos de plantilla correctamente encapsulan el proceso de generar respuestas HTML a un cliente.</span><span class="sxs-lookup"><span data-stu-id="3f232-108">In this section you're going to modify the `HelloWorldController` class to use view template files to cleanly encapsulate the process of generating HTML responses to a client.</span></span>

<span data-ttu-id="3f232-109">Creará un archivo de plantilla de vista mediante el [motor de vistas Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introducidos con ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="3f232-109">You'll create a view template file using the [Razor view engine](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introduced with ASP.NET MVC 3.</span></span> <span data-ttu-id="3f232-110">Las plantillas de vista Razor tienen una *.cshtml* la extensión de archivo y proporcionan una manera elegante para crear el código HTML de salida con C#.</span><span class="sxs-lookup"><span data-stu-id="3f232-110">Razor-based view templates have a *.cshtml* file extension, and provide an elegant way to create HTML output using C#.</span></span> <span data-ttu-id="3f232-111">Minimiza el número de caracteres y pulsaciones de teclas necesarias cuando se escribe una plantilla de vista Razor y permite un rápido, fluido flujo de trabajo de codificación.</span><span class="sxs-lookup"><span data-stu-id="3f232-111">Razor minimizes the number of characters and keystrokes required when writing a view template, and enables a fast, fluid coding workflow.</span></span>

<span data-ttu-id="3f232-112">Actualmente, el método `Index` devuelve una cadena con un mensaje que está codificado de forma rígida en la clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="3f232-112">Currently the `Index` method returns a string with a message that is hard-coded in the controller class.</span></span> <span data-ttu-id="3f232-113">Cambiar el `Index` método devuelva un `View` de objeto, como se muestra en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3f232-113">Change the `Index` method to return a `View` object, as shown in the following code:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

<span data-ttu-id="3f232-114">El `Index` método anterior usa una plantilla de vista para generar una respuesta HTML al explorador.</span><span class="sxs-lookup"><span data-stu-id="3f232-114">The `Index` method above uses a view template to generate an HTML response to the browser.</span></span> <span data-ttu-id="3f232-115">Los métodos de controlador (también conocido como [métodos de acción](http://rachelappel.com/asp.net-mvc-actionresults-explained)), como el `Index` método anterior, suelen devolver un [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (o una clase derivada de [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), los tipos no primitivos como cadena.</span><span class="sxs-lookup"><span data-stu-id="3f232-115">Controller methods (also known as [action methods](http://rachelappel.com/asp.net-mvc-actionresults-explained)), such as the `Index` method above, generally return an [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (or a class derived from [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), not primitive types like string.</span></span>

<span data-ttu-id="3f232-116">En el proyecto, agregue una plantilla de vista que puede usar con el `Index` método.</span><span class="sxs-lookup"><span data-stu-id="3f232-116">In the project, add a view template that you can use with the `Index` method.</span></span> <span data-ttu-id="3f232-117">Para ello, haga clic en el `Index` método y haga clic en **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="3f232-117">To do this, right-click inside the `Index` method and click **Add View**.</span></span>

![](adding-a-view/_static/image1.png)

<span data-ttu-id="3f232-118">El **agregar vista** aparece el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="3f232-118">The **Add View** dialog box appears.</span></span> <span data-ttu-id="3f232-119">Deje los valores predeterminados de la manera son y haga clic en el **agregar** botón:</span><span class="sxs-lookup"><span data-stu-id="3f232-119">Leave the defaults the way they are and click the **Add** button:</span></span>

![](adding-a-view/_static/image2.png)

<span data-ttu-id="3f232-120">El *MvcMovie\Views\HelloWorld* carpeta y el *MvcMovie\Views\HelloWorld\Index.cshtml* se crean archivos.</span><span class="sxs-lookup"><span data-stu-id="3f232-120">The *MvcMovie\Views\HelloWorld* folder and the *MvcMovie\Views\HelloWorld\Index.cshtml* file are created.</span></span> <span data-ttu-id="3f232-121">Puede verlos en **el Explorador de soluciones**:</span><span class="sxs-lookup"><span data-stu-id="3f232-121">You can see them in **Solution Explorer**:</span></span>

![](adding-a-view/_static/image3.png)

<span data-ttu-id="3f232-122">La siguiente muestra la *Index.cshtml* archivo creado:</span><span class="sxs-lookup"><span data-stu-id="3f232-122">The following shows the *Index.cshtml* file that was created:</span></span>

![HelloWorldIndex](adding-a-view/_static/image4.png)

<span data-ttu-id="3f232-124">Agregue el siguiente código HTML bajo el `<h2>` etiqueta.</span><span class="sxs-lookup"><span data-stu-id="3f232-124">Add the following HTML under the `<h2>` tag.</span></span>

[!code-html[Main](adding-a-view/samples/sample2.html)]

<span data-ttu-id="3f232-125">La completa *MvcMovie\Views\HelloWorld\Index.cshtml* archivo se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="3f232-125">The complete *MvcMovie\Views\HelloWorld\Index.cshtml* file is shown below.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

<span data-ttu-id="3f232-126">Si utiliza Visual Studio 2012, en el Explorador de soluciones, haga clic la *Index.cshtml* de archivo y seleccione **ver en Inspector de página**.</span><span class="sxs-lookup"><span data-stu-id="3f232-126">If you are using Visual Studio 2012, in solution explorer, right click the *Index.cshtml* file and select **View in Page Inspector**.</span></span>

![PI](adding-a-view/_static/image5.png)

<span data-ttu-id="3f232-128">El [tutorial del Inspector de página](../../views/using-page-inspector-in-aspnet-mvc.md) tiene más información sobre esta nueva herramienta.</span><span class="sxs-lookup"><span data-stu-id="3f232-128">The [Page Inspector tutorial](../../views/using-page-inspector-in-aspnet-mvc.md) has more information about this new tool.</span></span>

<span data-ttu-id="3f232-129">Como alternativa, ejecute la aplicación y vaya a la `HelloWorld` controlador (`http://localhost:xxxx/HelloWorld`).</span><span class="sxs-lookup"><span data-stu-id="3f232-129">Alternatively, run the application and browse to the `HelloWorld` controller (`http://localhost:xxxx/HelloWorld`).</span></span> <span data-ttu-id="3f232-130">El `Index` método en el controlador no hacen mucho trabajo; simplemente ejecutó la instrucción `return View()`, que especifica que el método debe utilizar un archivo de plantilla de vista para presentar una respuesta al explorador.</span><span class="sxs-lookup"><span data-stu-id="3f232-130">The `Index` method in your controller didn't do much work; it simply ran the statement `return View()`, which specified that the method should use a view template file to render a response to the browser.</span></span> <span data-ttu-id="3f232-131">Dado que no especifica explícitamente el nombre del archivo de plantilla de vista para usar, ASP.NET MVC usa de forma predeterminada el *Index.cshtml* los archivos de vista el *\Views\HelloWorld* carpeta.</span><span class="sxs-lookup"><span data-stu-id="3f232-131">Because you didn't explicitly specify the name of the view template file to use, ASP.NET MVC defaulted to using the *Index.cshtml* view file in the *\Views\HelloWorld* folder.</span></span> <span data-ttu-id="3f232-132">La siguiente imagen muestra la cadena &quot;Hola desde nuestra plantilla de vista!&quot; codificado de forma rígida en la vista.</span><span class="sxs-lookup"><span data-stu-id="3f232-132">The image below shows the string &quot;Hello from our View Template!&quot; hard-coded in the view.</span></span>

![](adding-a-view/_static/image6.png)

<span data-ttu-id="3f232-133">Parece bastante bueno.</span><span class="sxs-lookup"><span data-stu-id="3f232-133">Looks pretty good.</span></span> <span data-ttu-id="3f232-134">Sin embargo, tenga en cuenta que se muestra la barra de título del explorador &quot;indizar mi A ASP.NET&quot; y el vínculo grande en la parte superior de la página se lee &quot;su logotipo aquí.&quot; A continuación el &quot;su logotipo aquí.&quot; vínculo son aproximadamente de registro y registro en vínculos y, a continuación que se vincula a la página principal y póngase en contacto con las páginas.</span><span class="sxs-lookup"><span data-stu-id="3f232-134">However, notice that the browser's title bar shows &quot;Index My ASP.NET A&quot; and the big link on the top of the page says &quot;your logo here.&quot; Below the &quot;your logo here.&quot; link are registration and log in links, and below that links to Home, About and Contact pages.</span></span> <span data-ttu-id="3f232-135">Vamos a cambiar algunas de ellas.</span><span class="sxs-lookup"><span data-stu-id="3f232-135">Let's change some of these.</span></span>

## <a name="changing-views-and-layout-pages"></a><span data-ttu-id="3f232-136">Cambiar vistas y páginas de diseño</span><span class="sxs-lookup"><span data-stu-id="3f232-136">Changing Views and Layout Pages</span></span>

<span data-ttu-id="3f232-137">En primer lugar, desea cambiar la &quot;su logotipo aquí.&quot; título en la parte superior de la página.</span><span class="sxs-lookup"><span data-stu-id="3f232-137">First, you want to change the &quot;your logo here.&quot; title at the top of the page.</span></span> <span data-ttu-id="3f232-138">Es común a todas las páginas que el texto.</span><span class="sxs-lookup"><span data-stu-id="3f232-138">That text is common to every page.</span></span> <span data-ttu-id="3f232-139">En realidad se implementa en un único lugar en el proyecto, aunque aparezca en todas las páginas en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3f232-139">It's actually implemented in only one place in the project, even though it appears on every page in the application.</span></span> <span data-ttu-id="3f232-140">Vaya a la */Views/Shared* carpeta **el Explorador de soluciones** y abra el  *\_Layout.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="3f232-140">Go to the */Views/Shared* folder in **Solution Explorer** and open the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="3f232-141">Este archivo se denomina un *página de diseño* y es el recurso compartido &quot;shell&quot; que usan todas las demás páginas.</span><span class="sxs-lookup"><span data-stu-id="3f232-141">This file is called a *layout page* and it's the shared &quot;shell&quot; that all other pages use.</span></span>

![_LayoutCshtml](adding-a-view/_static/image7.png)

<span data-ttu-id="3f232-143">Las plantillas de diseño permiten especificar el diseño del contenedor HTML del sitio en un solo lugar y, a continuación, aplicarla en varias páginas del sitio.</span><span class="sxs-lookup"><span data-stu-id="3f232-143">Layout templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="3f232-144">Busque la línea `@RenderBody()`.</span><span class="sxs-lookup"><span data-stu-id="3f232-144">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="3f232-145">`RenderBody` es un marcador de posición donde se mostrarán todas las páginas específicas de vista que cree, &quot;encapsuladas&quot; en la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="3f232-145">`RenderBody` is a placeholder where all the view-specific pages you create show up, &quot;wrapped&quot; in the layout page.</span></span> <span data-ttu-id="3f232-146">Por ejemplo, si selecciona el vínculo acerca la *Views\Home\About.cshtml* vista se representa dentro de la `RenderBody` método.</span><span class="sxs-lookup"><span data-stu-id="3f232-146">For example, if you select the About link, the *Views\Home\About.cshtml* view is rendered inside the `RenderBody` method.</span></span>

<span data-ttu-id="3f232-147">Cambie el encabezado de título del sitio en la plantilla de diseño de &quot;su logotipo aquí&quot; a &quot;MVC Movie&quot;.</span><span class="sxs-lookup"><span data-stu-id="3f232-147">Change the site-title heading in the layout template from &quot;your logo here&quot; to &quot;MVC Movie&quot;.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

<span data-ttu-id="3f232-148">Reemplace el contenido del elemento de título con el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="3f232-148">Replace the contents of the title element with the following markup:</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

<span data-ttu-id="3f232-149">Ejecute la aplicación y observe que ahora dice &quot;MVC Movie &quot;.</span><span class="sxs-lookup"><span data-stu-id="3f232-149">Run the application and notice that it now says &quot;MVC Movie &quot;.</span></span> <span data-ttu-id="3f232-150">Haga clic en el **sobre** vínculo y ver cómo se muestra esa página &quot;MVC Movie&quot;, demasiado.</span><span class="sxs-lookup"><span data-stu-id="3f232-150">Click the **About** link, and you see how that page shows &quot;MVC Movie&quot;, too.</span></span> <span data-ttu-id="3f232-151">Fuimos capaces de realizar el cambio una vez en la plantilla de diseño y dispone de todas las páginas del sitio refleja el nuevo título.</span><span class="sxs-lookup"><span data-stu-id="3f232-151">We were able to make the change once in the layout template and have all pages on the site reflect the new title.</span></span>

![](adding-a-view/_static/image8.png)

<span data-ttu-id="3f232-152">Ahora, vamos a cambiar el título de la vista de índice.</span><span class="sxs-lookup"><span data-stu-id="3f232-152">Now, let's change the title of the Index view.</span></span>

<span data-ttu-id="3f232-153">Abra *MvcMovie\Views\HelloWorld\Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3f232-153">Open *MvcMovie\Views\HelloWorld\Index.cshtml*.</span></span> <span data-ttu-id="3f232-154">Hay dos lugares para realizar un cambio: en primer lugar, el texto que aparece en el título del explorador y, a continuación, en el encabezado secundario (el `<h2>` elemento).</span><span class="sxs-lookup"><span data-stu-id="3f232-154">There are two places to make a change: first, the text that appears in the title of the browser, and then in the secondary header (the `<h2>` element).</span></span> <span data-ttu-id="3f232-155">Haremos que sean algo diferentes para que pueda ver qué parte del código cambia cada área de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3f232-155">You'll make them slightly different so you can see which bit of code changes which part of the app.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

<span data-ttu-id="3f232-156">Para indicar el título HTML para mostrar el código anterior establece un `Title` propiedad de la `ViewBag` objeto (que se encuentra en la *Index.cshtml* plantilla de vista).</span><span class="sxs-lookup"><span data-stu-id="3f232-156">To indicate the HTML title to display, the code above sets a `Title` property of the `ViewBag` object (which is in the *Index.cshtml* view template).</span></span> <span data-ttu-id="3f232-157">Si repasa el código fuente de la plantilla de diseño, observará que la plantilla utiliza este valor en el `<title>` elemento como parte de la `<head>` sección del archivo HTML que se modificó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="3f232-157">If you look back at the source code of the layout template, you'll notice that the template uses this value in the `<title>` element as part of the `<head>` section of the HTML that we modified previously.</span></span> <span data-ttu-id="3f232-158">Mediante este `ViewBag` enfoque, puede pasar fácilmente otros parámetros entre la plantilla de vista y el archivo de diseño.</span><span class="sxs-lookup"><span data-stu-id="3f232-158">Using this `ViewBag` approach, you can easily pass other parameters between your view template and your layout file.</span></span>

<span data-ttu-id="3f232-159">Ejecute la aplicación y vaya a `http://localhost:xx/HelloWorld`.</span><span class="sxs-lookup"><span data-stu-id="3f232-159">Run the application and browse to `http://localhost:xx/HelloWorld`.</span></span> <span data-ttu-id="3f232-160">Tenga en cuenta que el título del explorador, el encabezado principal y los encabezados secundarios han cambiado.</span><span class="sxs-lookup"><span data-stu-id="3f232-160">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="3f232-161">(Si no ve los cambios en el explorador, es posible que esté viendo contenido almacenado en caché.</span><span class="sxs-lookup"><span data-stu-id="3f232-161">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="3f232-162">Presione Ctrl+F5 en el explorador para forzar que se cargue la respuesta del servidor). El título del explorador se crea con el `ViewBag.Title` se establece el *Index.cshtml* ver la plantilla y el adicionales &quot;-Movie App&quot; agregado en el archivo de diseño.</span><span class="sxs-lookup"><span data-stu-id="3f232-162">Press Ctrl+F5 in your browser to force the response from the server to be loaded.) The browser title is created with the `ViewBag.Title` we set in the *Index.cshtml* view template and the additional &quot;- Movie App&quot; added in the layout file.</span></span>

<span data-ttu-id="3f232-163">Observe también cómo el contenido en el *Index.cshtml* plantilla de vista se combinó con el  *\_Layout.cshtml* se envió la plantilla de vista y una única respuesta HTML al explorador.</span><span class="sxs-lookup"><span data-stu-id="3f232-163">Also notice how the content in the *Index.cshtml* view template was merged with the *\_Layout.cshtml* view template and a single HTML response was sent to the browser.</span></span> <span data-ttu-id="3f232-164">Con las plantillas de diseño es realmente fácil hacer cambios para que se apliquen en todas las páginas de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3f232-164">Layout templates make it really easy to make changes that apply across all of the pages in your application.</span></span>

![](adding-a-view/_static/image9.png)

<span data-ttu-id="3f232-165">Nuestra pequeña cantidad de &quot;datos&quot; (en este caso el &quot;Hola desde nuestra plantilla de vista!&quot; mensaje) está codificado de forma rígida, sin embargo.</span><span class="sxs-lookup"><span data-stu-id="3f232-165">Our little bit of &quot;data&quot; (in this case the &quot;Hello from our View Template!&quot; message) is hard-coded, though.</span></span> <span data-ttu-id="3f232-166">La aplicación de MVC tiene un &quot;V&quot; (vista) y tiene un &quot;C&quot; (controlador), pero no &quot;M&quot; (model) todavía.</span><span class="sxs-lookup"><span data-stu-id="3f232-166">The MVC application has a &quot;V&quot; (view) and you've got a &quot;C&quot; (controller), but no &quot;M&quot; (model) yet.</span></span> <span data-ttu-id="3f232-167">En breve, le guiaremos a través de cómo crear una base de datos y recuperar los datos del modelo del mismo.</span><span class="sxs-lookup"><span data-stu-id="3f232-167">Shortly, we'll walk through how create a database and retrieve model data from it.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="3f232-168">Pasar datos del controlador a la vista</span><span class="sxs-lookup"><span data-stu-id="3f232-168">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="3f232-169">Antes de hablar acerca de los modelos y vaya a una base de datos, sin embargo, en primer lugar hablemos acerca de cómo pasar información desde el controlador a una vista.</span><span class="sxs-lookup"><span data-stu-id="3f232-169">Before we go to a database and talk about models, though, let's first talk about passing information from the controller to a view.</span></span> <span data-ttu-id="3f232-170">Las clases de controlador se invocan en respuesta a una solicitud de dirección URL entrante.</span><span class="sxs-lookup"><span data-stu-id="3f232-170">Controller classes are invoked in response to an incoming URL request.</span></span> <span data-ttu-id="3f232-171">Una clase de controlador es donde se escribe el código que controla el explorador entrante solicita, recupera datos de una base de datos y, en última instancia decida qué tipo de respuesta para enviar al explorador.</span><span class="sxs-lookup"><span data-stu-id="3f232-171">A controller class is where you write the code that handles the incoming browser requests, retrieves data from a database, and ultimately decides what type of response to send back to the browser.</span></span> <span data-ttu-id="3f232-172">Plantillas de vista, a continuación, pueden utilizarse desde un controlador para generar y dar formato a una respuesta HTML al explorador.</span><span class="sxs-lookup"><span data-stu-id="3f232-172">View templates can then be used from a controller to generate and format an HTML response to the browser.</span></span>

<span data-ttu-id="3f232-173">Los controladores son responsables de proporcionar los datos u objetos son necesarios para una plantilla de vista presentar una respuesta al explorador.</span><span class="sxs-lookup"><span data-stu-id="3f232-173">Controllers are responsible for providing whatever data or objects are required in order for a view template to render a response to the browser.</span></span> <span data-ttu-id="3f232-174">Procedimiento recomendado: **Una plantilla de vista nunca debe realizar la lógica de negocios ni interactuar directamente con una base de datos**.</span><span class="sxs-lookup"><span data-stu-id="3f232-174">A best practice: **A view template should never perform business logic or interact with a database directly**.</span></span> <span data-ttu-id="3f232-175">En su lugar, una plantilla de vista deban trabajar solo con los datos que se proporcionan el controlador.</span><span class="sxs-lookup"><span data-stu-id="3f232-175">Instead, a view template should work only with the data that's provided to it by the controller.</span></span> <span data-ttu-id="3f232-176">Esto mantiene &quot;separación de preocupaciones&quot; ayuda a mantener el código limpio, fácil de probar y más fácil de mantener.</span><span class="sxs-lookup"><span data-stu-id="3f232-176">Maintaining this &quot;separation of concerns&quot; helps keep your code clean, testable and more maintainable.</span></span>

<span data-ttu-id="3f232-177">Actualmente, el `Welcome` método de acción en el `HelloWorldController` clase toma un `name` y un `numTimes` parámetro y, a continuación, obtiene los valores directamente en el explorador.</span><span class="sxs-lookup"><span data-stu-id="3f232-177">Currently, the `Welcome` action method in the `HelloWorldController` class takes a `name` and a `numTimes` parameter and then outputs the values directly to the browser.</span></span> <span data-ttu-id="3f232-178">En lugar de que el controlador represente esta respuesta como una cadena, vamos a cambiar el controlador para que use una plantilla de vista en su lugar.</span><span class="sxs-lookup"><span data-stu-id="3f232-178">Rather than have the controller render this response as a string, let's change the controller to use a view template instead.</span></span> <span data-ttu-id="3f232-179">La plantilla de vista generará una respuesta dinámica, lo que significa que debe pasar las partes de datos adecuadas desde el controlador a la vista para que se genere la respuesta.</span><span class="sxs-lookup"><span data-stu-id="3f232-179">The view template will generate a dynamic response, which means that you need to pass appropriate bits of data from the controller to the view in order to generate the response.</span></span> <span data-ttu-id="3f232-180">Para ello, indique al controlador que coloque los datos dinámicos (parámetros) que necesita la plantilla de vista en un `ViewBag` objeto que puede tener acceso la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="3f232-180">You can do this by having the controller put the dynamic data (parameters) that the view template needs in a `ViewBag` object that the view template can then access.</span></span>

<span data-ttu-id="3f232-181">Vuelva a la *HelloWorldController.cs* y cambie el `Welcome` método para agregar un `Message` y `NumTimes` valor para el `ViewBag` objeto.</span><span class="sxs-lookup"><span data-stu-id="3f232-181">Return to the *HelloWorldController.cs* file and change the `Welcome` method to add a `Message` and `NumTimes` value to the `ViewBag` object.</span></span> <span data-ttu-id="3f232-182">`ViewBag` es un objeto dinámico, lo que significa que puede colocar todo lo desee en él; la `ViewBag` objeto no tiene ninguna propiedad definida hasta que coloca algo dentro de él.</span><span class="sxs-lookup"><span data-stu-id="3f232-182">`ViewBag` is a dynamic object, which means you can put whatever you want in to it; the `ViewBag` object has no defined properties until you put something inside it.</span></span> <span data-ttu-id="3f232-183">El [sistema de enlace de modelos ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) asigna automáticamente los parámetros con nombre (`name` y `numTimes`) de la cadena de consulta en la barra de direcciones para los parámetros del método.</span><span class="sxs-lookup"><span data-stu-id="3f232-183">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters (`name` and `numTimes`) from the query string in the address bar to parameters in your method.</span></span> <span data-ttu-id="3f232-184">El archivo *HelloWorldController.cs* completo tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="3f232-184">The complete *HelloWorldController.cs* file looks like this:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

<span data-ttu-id="3f232-185">Ahora el `ViewBag` objeto contiene los datos que se pasará automáticamente a la vista.</span><span class="sxs-lookup"><span data-stu-id="3f232-185">Now the `ViewBag` object contains data that will be passed to the view automatically.</span></span>

<span data-ttu-id="3f232-186">A continuación, necesita una plantilla de vista principal.</span><span class="sxs-lookup"><span data-stu-id="3f232-186">Next, you need a Welcome view template!</span></span> <span data-ttu-id="3f232-187">En el **compilar** menú, seleccione **MvcMovie compilación** para asegurarse de que se compila el proyecto.</span><span class="sxs-lookup"><span data-stu-id="3f232-187">In the **Build** menu, select **Build MvcMovie** to make sure the project is compiled.</span></span>

<span data-ttu-id="3f232-188">A continuación, haga clic en el `Welcome` método y haga clic en **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="3f232-188">Then right-click inside the `Welcome` method and click **Add View**.</span></span>

![](adding-a-view/_static/image10.png)

<span data-ttu-id="3f232-189">Esto es lo que el **agregar vista** cuadro de diálogo es similar a:</span><span class="sxs-lookup"><span data-stu-id="3f232-189">Here's what the **Add View** dialog box looks like:</span></span>

![](adding-a-view/_static/image11.png)

<span data-ttu-id="3f232-190">Haga clic en **agregar**y, a continuación, agregue el siguiente código bajo el `<h2>` elemento en el nuevo *Welcome.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="3f232-190">Click **Add**, and then add the following code under the `<h2>` element in the new *Welcome.cshtml* file.</span></span> <span data-ttu-id="3f232-191">Se creará un bucle que dice &quot;Hello&quot; tantas veces como el usuario indica que debería.</span><span class="sxs-lookup"><span data-stu-id="3f232-191">You'll create a loop that says &quot;Hello&quot; as many times as the user says it should.</span></span> <span data-ttu-id="3f232-192">La completa *Welcome.cshtml* archivo se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="3f232-192">The complete *Welcome.cshtml* file is shown below.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

<span data-ttu-id="3f232-193">Ejecute la aplicación y vaya a la dirección URL siguiente:</span><span class="sxs-lookup"><span data-stu-id="3f232-193">Run the application and browse to the following URL:</span></span>

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

<span data-ttu-id="3f232-194">Ahora los datos se toman de la dirección URL y pasa al controlador mediante la [enlazador de modelos](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx).</span><span class="sxs-lookup"><span data-stu-id="3f232-194">Now data is taken from the URL and passed to the controller using the [model binder](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx).</span></span> <span data-ttu-id="3f232-195">El controlador empaqueta los datos en un `ViewBag` objeto y pasa ese objeto a la vista.</span><span class="sxs-lookup"><span data-stu-id="3f232-195">The controller packages the data into a `ViewBag` object and passes that object to the view.</span></span> <span data-ttu-id="3f232-196">La vista, a continuación, muestra los datos como HTML al usuario.</span><span class="sxs-lookup"><span data-stu-id="3f232-196">The view then displays the data as HTML to the user.</span></span>

![](adding-a-view/_static/image12.png)

<span data-ttu-id="3f232-197">En el ejemplo anterior, hemos usado un `ViewBag` objeto que se va a pasar datos a una vista del controlador.</span><span class="sxs-lookup"><span data-stu-id="3f232-197">In the sample above, we used a `ViewBag` object to pass data from the controller to a view.</span></span> <span data-ttu-id="3f232-198">Esta última en el tutorial, usaremos un modelo de vista para pasar datos de un controlador a una vista.</span><span class="sxs-lookup"><span data-stu-id="3f232-198">Latter in the tutorial, we will use a view model to pass data from a controller to a view.</span></span> <span data-ttu-id="3f232-199">El enfoque del modelo de vista para pasar los datos es generalmente mucho más conveniente el enfoque de contenedor de vista.</span><span class="sxs-lookup"><span data-stu-id="3f232-199">The view model approach to passing data is generally much preferred over the view bag approach.</span></span> <span data-ttu-id="3f232-200">Consulte la entrada de blog [vistas dinámicas de V fuertemente tipados](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="3f232-200">See the blog entry [Dynamic V Strongly Typed Views](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) for more information.</span></span>

<span data-ttu-id="3f232-201">Bueno, eso era un tipo de un &quot;M&quot; para el modelo, pero no el tipo de base de datos.</span><span class="sxs-lookup"><span data-stu-id="3f232-201">Well, that was a kind of an &quot;M&quot; for model, but not the database kind.</span></span> <span data-ttu-id="3f232-202">Vamos a aprovechar lo que hemos aprendido para crear una base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="3f232-202">Let's take what we've learned and create a database of movies.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3f232-203">[Anterior](adding-a-controller.md)
> [Siguiente](adding-a-model.md)</span><span class="sxs-lookup"><span data-stu-id="3f232-203">[Previous](adding-a-controller.md)
[Next](adding-a-model.md)</span></span>