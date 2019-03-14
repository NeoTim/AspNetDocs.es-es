---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: Información general sobre (VB) de las vistas de MVC de ASP.NET | Microsoft Docs
author: StephenWalther
description: ¿Qué es una vista de MVC de ASP.NET y cómo se diferencia de una página HTML? En este tutorial, Stephen Walther presenta las vistas y demuestra cómo puede t...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: a7f4afd70a17281123a7448a00896c186b9a00f8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57030372"
---
<a name="aspnet-mvc-views-overview-vb"></a><span data-ttu-id="888a4-104">Información general sobre las vistas de ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="888a4-104">ASP.NET MVC Views Overview (VB)</span></span>
====================
<span data-ttu-id="888a4-105">by [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="888a4-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="888a4-106">¿Qué es una vista de MVC de ASP.NET y cómo se diferencia de una página HTML?</span><span class="sxs-lookup"><span data-stu-id="888a4-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="888a4-107">En este tutorial, Stephen Walther presenta las vistas y se muestra cómo puede sacar partido de la vista de datos y aplicaciones auxiliares HTML dentro de una vista.</span><span class="sxs-lookup"><span data-stu-id="888a4-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>


<span data-ttu-id="888a4-108">El propósito de este tutorial es proporcionar una breve introducción a las vistas de ASP.NET MVC, ver los datos y aplicaciones auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="888a4-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="888a4-109">Al final de este tutorial, debe comprender cómo crear nuevas vistas, pasar datos de un controlador a una vista y usar aplicaciones auxiliares HTML para generar contenido en una vista.</span><span class="sxs-lookup"><span data-stu-id="888a4-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="888a4-110">Descripción de vistas</span><span class="sxs-lookup"><span data-stu-id="888a4-110">Understanding Views</span></span>

<span data-ttu-id="888a4-111">A diferencia de ASP.NET o páginas Active Server, ASP.NET MVC no incluye todo lo que se corresponde directamente con una página.</span><span class="sxs-lookup"><span data-stu-id="888a4-111">Unlike ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="888a4-112">En una aplicación de ASP.NET MVC, no hay una página en disco que se corresponde con la ruta de acceso en la dirección URL que se escribe en la barra de direcciones del explorador.</span><span class="sxs-lookup"><span data-stu-id="888a4-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="888a4-113">Lo más parecido a una página en una aplicación ASP.NET MVC es algo llamado un *vista*.</span><span class="sxs-lookup"><span data-stu-id="888a4-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="888a4-114">En una aplicación ASP.NET MVC, las solicitudes entrantes se asignan a acciones del controlador.</span><span class="sxs-lookup"><span data-stu-id="888a4-114">In an ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="888a4-115">Una acción de controlador podría devolver una vista.</span><span class="sxs-lookup"><span data-stu-id="888a4-115">A controller action might return a view.</span></span> <span data-ttu-id="888a4-116">Sin embargo, una acción de controlador podría realizar algún otro tipo de acción como se le redirigirá a otra acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="888a4-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="888a4-117">Listado 1 contiene un controlador simple denominado HomeController.</span><span class="sxs-lookup"><span data-stu-id="888a4-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="888a4-118">HomeController expone dos acciones de controlador denominadas Index() y Details().</span><span class="sxs-lookup"><span data-stu-id="888a4-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="888a4-119">**Listado 1 - HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="888a4-119">**Listing 1 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

<span data-ttu-id="888a4-120">Puede invocar la primera acción, la acción de Index() escribiendo la siguiente dirección URL en la barra de direcciones del explorador:</span><span class="sxs-lookup"><span data-stu-id="888a4-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="888a4-121">/Home/Index</span><span class="sxs-lookup"><span data-stu-id="888a4-121">/Home/Index</span></span>

<span data-ttu-id="888a4-122">Puede invocar la segunda acción, la acción Details(), escribiendo esta dirección en el explorador:</span><span class="sxs-lookup"><span data-stu-id="888a4-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="888a4-123">/ Principal/detalles</span><span class="sxs-lookup"><span data-stu-id="888a4-123">/Home/Details</span></span>

<span data-ttu-id="888a4-124">La acción de Index() devuelve una vista.</span><span class="sxs-lookup"><span data-stu-id="888a4-124">The Index() action returns a view.</span></span> <span data-ttu-id="888a4-125">La mayoría de las acciones que cree devolverá las vistas.</span><span class="sxs-lookup"><span data-stu-id="888a4-125">Most actions that you create will return views.</span></span> <span data-ttu-id="888a4-126">Sin embargo, una acción puede devolver otros tipos de resultados de acción.</span><span class="sxs-lookup"><span data-stu-id="888a4-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="888a4-127">Por ejemplo, la acción Details() devuelve un RedirectToActionResult que redirige la solicitud entrante a la acción de Index().</span><span class="sxs-lookup"><span data-stu-id="888a4-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="888a4-128">La acción de Index() contiene la única línea siguiente de código:</span><span class="sxs-lookup"><span data-stu-id="888a4-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="888a4-129">View()</span><span class="sxs-lookup"><span data-stu-id="888a4-129">View()</span></span>

<span data-ttu-id="888a4-130">Esta línea de código devuelve una vista que debe estar ubicada en la siguiente ruta de acceso en el servidor web:</span><span class="sxs-lookup"><span data-stu-id="888a4-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="888a4-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="888a4-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="888a4-132">La ruta de acceso a la vista se deduce el nombre del controlador y el nombre de la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="888a4-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="888a4-133">Si lo prefiere, puede ser explícito acerca de la vista.</span><span class="sxs-lookup"><span data-stu-id="888a4-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="888a4-134">La siguiente línea de código devuelve una vista denominada a Fred:</span><span class="sxs-lookup"><span data-stu-id="888a4-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="888a4-135">Vista (Fred)</span><span class="sxs-lookup"><span data-stu-id="888a4-135">View( Fred )</span></span>

<span data-ttu-id="888a4-136">Cuando se ejecuta esta línea de código, se devuelve una vista de la ruta de acceso siguiente:</span><span class="sxs-lookup"><span data-stu-id="888a4-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="888a4-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="888a4-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="888a4-138">Si va a crear pruebas unitarias para la aplicación de ASP.NET MVC es una buena idea de ser explícito acerca de los nombres de vista.</span><span class="sxs-lookup"><span data-stu-id="888a4-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="888a4-139">De este modo, puede crear una prueba unitaria para comprobar que la vista esperada se devolvió mediante una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="888a4-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>


## <a name="adding-content-to-a-view"></a><span data-ttu-id="888a4-140">Agregar contenido a una vista</span><span class="sxs-lookup"><span data-stu-id="888a4-140">Adding Content to a View</span></span>

<span data-ttu-id="888a4-141">Una vista es un estándar de (documento HTML que puede contener las secuencias de comandos X).</span><span class="sxs-lookup"><span data-stu-id="888a4-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="888a4-142">Usar secuencias de comandos para agregar contenido dinámico a una vista.</span><span class="sxs-lookup"><span data-stu-id="888a4-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="888a4-143">Por ejemplo, la vista en el listado 2 muestra la fecha y hora actuales.</span><span class="sxs-lookup"><span data-stu-id="888a4-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="888a4-144">**Listing 2 - \Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="888a4-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

<span data-ttu-id="888a4-145">Tenga en cuenta que el cuerpo de la página HTML en el listado 2 contiene el script siguiente:</span><span class="sxs-lookup"><span data-stu-id="888a4-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="888a4-146">&lt;% Response.Write(DateTime.Now) %&gt;</span><span class="sxs-lookup"><span data-stu-id="888a4-146">&lt;% Response.Write(DateTime.Now)%&gt;</span></span>

<span data-ttu-id="888a4-147">Utilice los delimitadores de secuencia de comandos &lt;% y %&gt; para marcar el principio y al final de una secuencia de comandos.</span><span class="sxs-lookup"><span data-stu-id="888a4-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="888a4-148">Este script se escribe en Visual basic.</span><span class="sxs-lookup"><span data-stu-id="888a4-148">This script is written in Visual basic.</span></span> <span data-ttu-id="888a4-149">Muestra la fecha y hora actual llamando al método Response.Write () para representar el contenido al explorador.</span><span class="sxs-lookup"><span data-stu-id="888a4-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="888a4-150">Los delimitadores de secuencia de comandos &lt;% y %&gt; puede utilizarse para ejecutar una o varias instrucciones.</span><span class="sxs-lookup"><span data-stu-id="888a4-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="888a4-151">Dado que a menudo se llama a Response.Write (), Microsoft le proporciona un acceso directo para llamar al método Response.Write ().</span><span class="sxs-lookup"><span data-stu-id="888a4-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="888a4-152">La vista en el listado 3 utiliza los delimitadores &lt;% = y %&gt; como método abreviado para llamar a Response.Write ().</span><span class="sxs-lookup"><span data-stu-id="888a4-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="888a4-153">**Listing 3 - Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="888a4-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

<span data-ttu-id="888a4-154">Puede usar cualquier lenguaje .NET para generar contenido dinámico en una vista.</span><span class="sxs-lookup"><span data-stu-id="888a4-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="888a4-155">Normalmente, se deberá usar Visual Basic .NET o C# para escribir los controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="888a4-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="888a4-156">Usar aplicaciones auxiliares HTML para generar contenido de la vista</span><span class="sxs-lookup"><span data-stu-id="888a4-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="888a4-157">Para que sea más fácil agregar contenido a una vista, puede sacar partido de algo llamado un *aplicación auxiliar HTML*.</span><span class="sxs-lookup"><span data-stu-id="888a4-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="888a4-158">Una aplicación auxiliar HTML, por lo general, es un método que genera una cadena.</span><span class="sxs-lookup"><span data-stu-id="888a4-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="888a4-159">Puede usar aplicaciones auxiliares HTML para generar los elementos HTML estándar, como cuadros de texto, vínculos, listas desplegables y cuadros de lista.</span><span class="sxs-lookup"><span data-stu-id="888a4-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="888a4-160">Por ejemplo, la vista en el listado 4 se aprovecha de tres aplicaciones auxiliares de HTML, los ayudantes BeginForm(), TextBox() y Password()--para generar un inicio de sesión de formulario (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="888a4-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="888a4-161">**Listing 4 -- \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="888a4-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]


<span data-ttu-id="888a4-162">[![El cuadro de diálogo nuevo proyecto](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="888a4-162">[![The New Project dialog box](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="888a4-163">**Figura 01**: Un formulario de inicio de sesión estándar ([haga clic aquí para ver imagen en tamaño completo](asp-net-mvc-views-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="888a4-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-vb/_static/image2.png))</span></span>


<span data-ttu-id="888a4-164">Todos los métodos de las aplicaciones auxiliares HTML se llaman en la propiedad Html de la vista.</span><span class="sxs-lookup"><span data-stu-id="888a4-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="888a4-165">Por ejemplo, representa un cuadro de texto llamando al método Html.TextBox().</span><span class="sxs-lookup"><span data-stu-id="888a4-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="888a4-166">Tenga en cuenta que utilice los delimitadores de secuencia de comandos &lt;% = y %&gt; al llamar a las aplicaciones auxiliares de la Html.TextBox() y Html.Password().</span><span class="sxs-lookup"><span data-stu-id="888a4-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="888a4-167">Estas aplicaciones auxiliares de simplemente devuelven una cadena.</span><span class="sxs-lookup"><span data-stu-id="888a4-167">These helpers simply return a string.</span></span> <span data-ttu-id="888a4-168">Debe llamar a Response.Write () para representar la cadena en el explorador.</span><span class="sxs-lookup"><span data-stu-id="888a4-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="888a4-169">Uso de métodos auxiliares HTML es opcional.</span><span class="sxs-lookup"><span data-stu-id="888a4-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="888a4-170">Facilitan la vida al reducir la cantidad de HTML y secuencias de comandos que necesita para escribir.</span><span class="sxs-lookup"><span data-stu-id="888a4-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="888a4-171">La vista en el listado 5 representa la misma forma exacta que la vista en el listado 4 sin usar aplicaciones auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="888a4-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="888a4-172">**Listing 5 -- \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="888a4-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

<span data-ttu-id="888a4-173">También tiene la opción de crear sus propias aplicaciones auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="888a4-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="888a4-174">Por ejemplo, puede crear un método auxiliar GridView() que muestra automáticamente un conjunto de registros de base de datos en una tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="888a4-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="888a4-175">Se explorará en este tema en el tutorial **crear aplicaciones auxiliares de HTML personalizado**.</span><span class="sxs-lookup"><span data-stu-id="888a4-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="888a4-176">Usar datos de vista para pasar datos a una vista</span><span class="sxs-lookup"><span data-stu-id="888a4-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="888a4-177">Usar datos de la vista para pasar datos de un controlador a una vista.</span><span class="sxs-lookup"><span data-stu-id="888a4-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="888a4-178">Piense en los datos de vista como un paquete que se envía por correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="888a4-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="888a4-179">Todos los datos transferidos desde un controlador a una vista deben enviarse con este paquete.</span><span class="sxs-lookup"><span data-stu-id="888a4-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="888a4-180">Por ejemplo, el controlador en el listado 6 agrega un mensaje para ver los datos.</span><span class="sxs-lookup"><span data-stu-id="888a4-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="888a4-181">**Listado 6 - ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="888a4-181">**Listing 6 - ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

<span data-ttu-id="888a4-182">El controlador de propiedad ViewData representa una colección de pares de nombre y valor.</span><span class="sxs-lookup"><span data-stu-id="888a4-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="888a4-183">En el listado 6, el método Index() agrega un elemento a la colección de datos de vista denominada mensaje con el valor de Hello World!.</span><span class="sxs-lookup"><span data-stu-id="888a4-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="888a4-184">Cuando se devuelve la vista por el método Index(), los datos de vista se pasan automáticamente a la vista.</span><span class="sxs-lookup"><span data-stu-id="888a4-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="888a4-185">La vista en la lista 7 recupera el mensaje de los datos de vista y procesa el mensaje en el explorador.</span><span class="sxs-lookup"><span data-stu-id="888a4-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="888a4-186">**Listing 7 -- \Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="888a4-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

<span data-ttu-id="888a4-187">Tenga en cuenta que la vista aprovecha el método auxiliar de Html.Encode() HTML al representar el mensaje.</span><span class="sxs-lookup"><span data-stu-id="888a4-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="888a4-188">La aplicación auxiliar HTML Html.Encode() codifica caracteres especiales como &lt; y &gt; en caracteres que son seguros mostrar en una página web.</span><span class="sxs-lookup"><span data-stu-id="888a4-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="888a4-189">Cada vez que se representa el contenido que un usuario envía a un sitio Web, debe codificar el contenido para evitar ataques por inyección de código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="888a4-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="888a4-190">(Ya hemos creado el mensaje nosotros mismos ProductController, no queremos t realmente se necesita codificar el mensaje.</span><span class="sxs-lookup"><span data-stu-id="888a4-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="888a4-191">Sin embargo, es un buen hábito siempre llamar al método Html.Encode() al mostrar el contenido de recuperarse de los datos de vista en una vista.)</span><span class="sxs-lookup"><span data-stu-id="888a4-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="888a4-192">En la lista 7, aprovechamos de datos de vista que se va a pasar un mensaje de cadena simple de un controlador a una vista.</span><span class="sxs-lookup"><span data-stu-id="888a4-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="888a4-193">También puede usar los datos de vista para pasar otros tipos de datos, como una colección de registros de base de datos, desde un controlador a una vista.</span><span class="sxs-lookup"><span data-stu-id="888a4-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="888a4-194">Por ejemplo, si desea mostrar el contenido de la tabla de base de datos de productos en una vista, a continuación, deberá pasar la colección de base de datos en la vista registra datos.</span><span class="sxs-lookup"><span data-stu-id="888a4-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="888a4-195">También tiene la opción de pasar los datos de vista fuertemente tipada de un controlador a una vista.</span><span class="sxs-lookup"><span data-stu-id="888a4-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="888a4-196">Se explorará en este tema en el tutorial **descripción fuertemente tipados vista de los datos y vistas**.</span><span class="sxs-lookup"><span data-stu-id="888a4-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="888a4-197">Resumen</span><span class="sxs-lookup"><span data-stu-id="888a4-197">Summary</span></span>

<span data-ttu-id="888a4-198">Este tutorial proporciona una breve introducción a las vistas de ASP.NET MVC, ver los datos y aplicaciones auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="888a4-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="888a4-199">En la primera sección, ha aprendido a agregar nuevas vistas al proyecto.</span><span class="sxs-lookup"><span data-stu-id="888a4-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="888a4-200">Ha aprendido que debe agregar una vista a la carpeta correcta para poder llamarlo desde un controlador determinado.</span><span class="sxs-lookup"><span data-stu-id="888a4-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="888a4-201">A continuación, analizamos el tema de las aplicaciones auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="888a4-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="888a4-202">Ha aprendido cómo las aplicaciones auxiliares HTML le permiten generar fácilmente el contenido HTML estándar.</span><span class="sxs-lookup"><span data-stu-id="888a4-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="888a4-203">Por último, ha aprendido cómo sacar partido de los datos de vista para pasar datos de un controlador a una vista.</span><span class="sxs-lookup"><span data-stu-id="888a4-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="888a4-204">[Anterior](passing-data-to-view-master-pages-cs.md)
> [Siguiente](creating-custom-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="888a4-204">[Previous](passing-data-to-view-master-pages-cs.md)
[Next](creating-custom-html-helpers-vb.md)</span></span>
